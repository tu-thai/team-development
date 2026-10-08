# Request: Study and Evaluate PoseNet from the Renesas RUHMI Model Zoo

- **Assignee:** Khoa Do
- **Requested and directed by:** Tu Thai (task, scope, technical direction)
- **Reviewed by:** Duy Phan (technical review of scripts, results, and report)
- **Track:** Track 1 (AI engineering): model evaluation, then conversion/quantization
- **Timebox:** 2 weeks *(to agree)*, with a checkpoint at the end of week 1
- **Status:** Draft

## 1. Background

The Conversational Hand Gestures demo runs palm detection and hand landmark models on the RA8P1 (Cortex-M85 + Ethos-U55). PoseNet (MobileNetV1-0.5) is one of the models we've picked to extend it. Pose estimation would give the device body context, not just the hand.

Renesas publishes a ready-made PoseNet package in the RUHMI Model Zoo:

- Package: <https://github.com/renesas/ruhmi-model-zoo/tree/main/vision/pose_estimation/posenet>
- Repo root (setup, RUHMI compiler install): <https://github.com/renesas/ruhmi-model-zoo>
- RUHMI AI MCU compiler: <https://github.com/renesas/ruhmi-framework-mcu>

Before we build on it, we need our own independent view of what it actually delivers. Renesas's README numbers are a starting point, not something to accept as given.

### What the package contains

| Item | Notes |
| :--- | :--- |
| Model | PoseNet MobileNetV1-0.5, input `1x257x257x3`, 17 COCO keypoints, multi-person, output stride 16 |
| Formats | TFLite FP32 and INT8 (`python/model/`) |
| Python | `download_model.py` (weights to ONNX to TFLite, INT8 calibration on COCO val2017), `inference.py` (single-image inference) |
| Compile config | `python/config.yaml` for `ruhmi_tools/mcu_compile.py` (CPU or NPU target) |
| Embedded C | `preprocessing.c` (resize + normalize + INT8 quantize), `postprocessing.c` (multi-person decoder), `model_metadata.h`, precompiled `src_mcu/` (CPU) and `src_mcu_npu/` (Ethos-U55) |
| Toolchain used by Renesas | MERA `2.6.0+pkg.4815`, FSP 6.5.1 |

### What Renesas reports

| Runtime | AP (OKS) | AP50 | AP75 | AR |
| :--- | :---: | :---: | :---: | :---: |
| TFLite FP32 | 7.71 | 21.87 | 3.69 | 13.45 |
| TFLite INT8 | 7.31 | 21.57 | 3.43 | 13.17 |
| MERA INT8 | 7.21 | 21.28 | 3.56 | 13.10 |

On-target latency, model only (Cortex-M85 @ 1 GHz, Ethos-U55 @ 500 MHz, OSPI + external SDRAM): **CPU 372 ms, NPU 5 ms**.

Two things here need explaining, not just reproducing:

1. **The AP is very low.** Find out why. Possible causes are the small model and input, the decoder settings, or the evaluation method.
2. **5 ms on the NPU is "AI-only".** It leaves out preprocessing and the CPU-side multi-person decoder. Those may well dominate the end-to-end time.

## 2. Objectives

1. Understand how the package works end to end: model, pre/post-processing, conversion, compilation, integration.
2. Reproduce the accuracy figures on the host independently, FP32 vs INT8.
3. Measure real on-target performance on RA8P1: latency **including** pre/post-processing, plus memory footprint.
4. Judge whether PoseNet fits the hand gesture demo, and what it would cost to add.

## 3. Scope

**In scope**
- Host-side setup, inference, and accuracy evaluation (Python)
- Rebuilding the INT8 model with `download_model.py` and comparing it with the shipped INT8 model
- Compiling with RUHMI for both CPU and NPU targets
- On-target benchmarking on an RA8P1 board, using the Renesas inference benchmarking project as the base
- A written evaluation report with a recommendation

**Out of scope (for now)**
- Retraining or fine-tuning the model
- Integrating it into the gesture demo firmware or µT-Kernel task design
- Evaluating alternative pose models (we may do this as a follow-up, depending on findings)

## 4. Tasks

### Step 1 — Study (days 1-2)
- Read the package README, the repo root README, and the RUHMI compiler install guide.
- Read `inference.py`, `posenet/decode_multi.py`, `preprocessing.c`, and `postprocessing.c`. Write up the pipeline in your own words: input normalization, the four output heads (heatmap, offsets, forward and backward displacement), and how the decoder builds poses.
- Note the decoder parameters (`max-pose`, `min-pose-score`, `min-part-score`, NMS radius) and their defaults.

### Step 2 — Host reproduction (days 2-5)
- Set up the inference venv (Python 3.10) and install Git LFS so the `.tflite` files are real, not pointer stubs.
- Run `inference.py` on the sample images with both FP32 and INT8. Save the annotated outputs.
- **Write an evaluation script.** The package ships none, but `pycocotools` is already in `requirements.txt`. Run FP32 and INT8 over COCO val2017 person keypoints and report AP, AP50, AP75, APm, APl, and AR.
- Compare your numbers with Renesas's table. Explain any gap, and explain why the AP is low in absolute terms.
- Run a small sweep of decoder thresholds to see how sensitive AP is to them.

### Step 3 — Conversion and quantization (days 5-7)
- Rebuild FP32 and INT8 with `download_model.py`. Try at least two `--calib-num` values (for example 100 and 1000) and compare accuracy against the shipped INT8 model.
- Set up the compiler venv (`.mera_venv`) and compile for `target: cpu` and `target: npu`. Check that your generated C matches the shipped `src_mcu/` and `src_mcu_npu/`, or explain the differences.
- Record which operators run on the Ethos-U55 and which fall back to the CPU.

**Checkpoint with Tu (end of week 1):** host results plus compile results. Before you go to the board, we'll agree on what to measure there.

### Step 4 — On-target benchmark (week 2)
- Port both builds into the RUHMI inference benchmarking project on RA8P1.
- Measure, separately: preprocessing, model inference, postprocessing (decoder), and end-to-end time. Use a realistic camera input size (for example 640x480 RGB888).
- Record memory: weights and code (flash/OSPI), tensor arena and working buffers (SRAM/SDRAM). Check whether it can run without external SDRAM, since the demo already uses memory for the hand models and camera/display.
- Check that on-target outputs match the host INT8 output on the same image (keypoints within a stated tolerance).

### Step 5 — Fit assessment for the gesture demo (end of week 2)
- Run a qualitative check on images closer to our use case: close range, upper body only, a single person facing the camera, and partial occlusion by hands. Note where it fails. Use our own or freely licensed images, not COCO.
- Estimate the cost of adding PoseNet next to palm detection and hand landmark: extra latency per frame, NPU sharing, and memory headroom.

## 5. Deliverables

Put these in the team repo as Duy directs:

1. **Evaluation report** (`posenet-evaluation-report.md`) covering:
   - Pipeline summary in your own words
   - An accuracy table: Renesas's figures vs ours, FP32 vs INT8 vs MERA
   - A latency and memory table for CPU and NPU, broken down by stage
   - Findings: gaps, surprises, limitations
   - A recommendation (use as is / use with changes / look for an alternative), with reasons
2. **Scripts:** the COCO evaluation script and any benchmarking helpers, runnable from a clean venv, with a short README
3. **Raw results:** logs and JSON outputs, so the tables can be regenerated

## 6. Questions the report must answer

1. Can we reproduce Renesas's accuracy numbers? If not, why not?
2. Why is the absolute AP so low, and does that matter for our use case (single child, close range, upper body)?
3. How much accuracy does INT8 lose compared with FP32, and does calibration size change that?
4. What is the real end-to-end time per frame on the NPU, including pre/post-processing? What share of it is the decoder?
5. Does it fit in memory alongside the hand models, and is external SDRAM required?
6. Should we use it as the pose model for the demo, and what would need to change (input size, decoder, single-pose mode)?

## 7. Constraints and notes

- **COCO licensing:** the README states that COCO 2017 keypoint validation images are for academic research only and must not be redistributed commercially. Use them only for internal evaluation. Do not commit COCO images to any Gnomons repo or include them in deliverables or demos.
- **Toolchain versions:** Renesas used MERA 2.6.0 and FSP 6.5.1. Use the same versions first; only try newer ones as a separate experiment.
- **Separate venvs:** keep the inference venv and the compiler venv separate, as the repo recommends. Their dependencies conflict.
- **Record everything:** for every number, record the tool versions, the model file (and its hash), and the exact command. Reproducibility counts as much as the numbers.

## 8. Working arrangement

- **Direction:** Tu sets scope and technical direction. Bring questions about *what* to measure or *why* to Tu.
- **Review:** Duy reviews scripts, the firmware benchmark setup, and the final report. Bring *how* questions and code to Duy's mentoring slot.
- **Checkpoints:** end of week 1 (host + compile results) and end of week 2 (report draft).
- **Blockers:** raise them straight away; don't wait for the checkpoint. Examples: no board available, compiler install failures, LFS problems.

## 9. Done when

- [ ] Host accuracy reproduced (or the gap explained) for FP32 and INT8, using our own evaluation script
- [ ] CPU and NPU builds compiled by us and benchmarked on RA8P1, with stage-by-stage timing and memory figures
- [ ] Report reviewed by Duy and discussed with Tu, with a clear recommendation
