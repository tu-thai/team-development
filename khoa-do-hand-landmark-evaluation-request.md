# Request: Study and Evaluate the Renesas Palm Detection + Hand Landmark Example

- **Assignee:** Khoa Do
- **Requested and directed by:** Tu Thai (task, scope, technical direction)
- **Reviewed by:** Duy Phan (technical review of firmware setup, measurements, and report)
- **Theme:** AI engineering: model evaluation, on-target benchmarking
- **Timebox:** about 2 weeks of effort, in two phases, each ending with a checkpoint

## 1. Background

Renesas has released a new FSP 6.6.0 version of its two-stage hand tracking example in the RUHMI framework repo (release note dated 2026-09-04, last repo update 2026-10-03):

- Example: <https://github.com/renesas/ruhmi-framework-mcu/tree/main/application_examples/palm_detection_hand_landmark>
- Release note: <https://github.com/renesas/ruhmi-framework-mcu/blob/main/application_examples/release_note.md>
- Repo root (RUHMI AI MCU compiler): <https://github.com/renesas/ruhmi-framework-mcu>

This is effectively a public reference for the same pipeline our demo uses. Customers will compare us with it, so we must know it in detail.

### What the example contains

| Item | Notes |
| :--- | :--- |
| Models | Palm Detection **Full** INT8 (192×192) + Hand Landmark **Sparse** INT8 (224×224), from PINTO0309's `hand-gesture-recognition-using-onnx` (Apache-2.0), derived from MediaPipe, quantized by Renesas |
| Model delivery | Precompiled RUHMI C only (`ruhmi_conversion_results/`, `ruhmi_landmark_results/`). No `.tflite`/`.onnx` and no compile config shipped |
| Pipeline | Palm detect → rotated crop → landmark (21 keypoints, x/y + relative z), up to 2 hands. **Tracking-first:** once a hand is tracked, the crop comes from landmarks and the detector runs only periodically |
| Smoothing | 1€ filter (independent implementation) on hand ROI and landmark points |
| Key source | `palm_detection/MainLoop_obj.cc` (pipeline), `ai_inference_thread_entry.c` (result publishing), `display_layer/detection_screen_mipi.c` (UI) |
| Memory layout | Palm input image in SDRAM; model weights in OSPI, copied to SDRAM; tensor arena in on-chip SRAM, **shared** by both models |
| Platform | EK-RA8P1 (built-in camera and LCD), FreeRTOS, e² studio 2026-07, FSP 6.6.0, LLVM/ATfE 22.1.0 |
| License | Code BSD-3-Clause; models Apache-2.0 (via MediaPipe) |

### What Renesas reports

| Case | AI pipeline time | Notes |
| :--- | :---: | :--- |
| 0 hands | ~38 ms | Detector runs every frame |
| 1 hand | ~30 ms | Previous release: ~63 ms |
| 2 hands | ~53 ms | Previous release: ~84 ms |
| Camera frame period | 26 ms (38 fps) | Fixed |

Three things need checking, not just reproducing:

1. **The timing window is narrow.** `ai_inference_time_ms` covers tensor copy, inference, and pre/post-processing. It leaves out camera capture, camera post-processing, and LCD refresh. The real end-to-end latency (frame captured → result on screen) is unknown.
2. **The speed-up comes mostly from tracking, not the models.** We need to know what tracking-first costs in robustness: fast motion, hands entering/leaving, two hands crossing.
3. **No accuracy figures are given.** The README says the models "have not been retrained or fine-tuned … provided as-is".

## 2. Objectives

1. Understand how the example works end to end: models, pre/post-processing, tracking, smoothing, memory placement, task structure.
2. Measure its real on-target performance on EK-RA8P1, stage by stage and end to end.
3. Assess its quality (detection, landmark stability, tracking robustness) on scenes close to our use case.
4. Compare it head-to-head with our Conversational Hand Gestures vision pipeline, and say what we should adopt, what we do better, and where we stand.

## 3. Scope

**In scope**
- Building, flashing, and running the example as released (FSP 6.6.0)
- Code study of the AI pipeline, tracking logic, and 1€ filter
- Identifying which model parts run on the Ethos-U55 and which fall back to the CPU (the palm model has a CPU subgraph, `compute_sub_0001.c`)
- On-target benchmarking with added instrumentation
- A quality test on our own recorded scenes
- A side-by-side comparison with the vision pipeline of our demo's **FreeRTOS version** (Duy provides the build)

**Out of scope (for now)**
- Gesture recognition, including Renesas's separate gesture recognition example
- Retraining, fine-tuning, or re-quantizing the models
- Merging the example into our demo firmware
- Benchmark accuracy on public hand datasets (most, e.g. FreiHAND, are non-commercial licensed; we can revisit)

## 4. Tasks

### Phase 1 — Study, build, and benchmark

#### Step 1 — Study
- Read the example README, the release note, and the RUHMI repo root README.
- Read `MainLoop_obj.cc`, `palm_preprocess.c`, `palm_postprocess.c`, `landmark_preprocess.c`, `landmark_postprocess.c`, and `ai_inference_thread_entry.c`. Write up the pipeline in your own words: anchors and decoding (2016 anchors), NMS, rotated crop, landmark output, when the detector is re-run, and how tracking is lost and re-acquired.
- Note every tunable parameter (score thresholds, NMS, detector interval, 1€ filter `min_cutoff`/`beta`, max hands) and its default.
- Map the memory placement (`application_config.h`).
- Map the FreeRTOS task structure: every task, its priority, stack size, and what it does.
- Study the other RTOS objects in use (semaphores, mutexes, queues, event groups, task notifications, software timers, and any others) and how they're used: which task waits on or signals each one, from task or ISR context, and how camera, AI, and display are synchronized. The `src/` code makes heavy use of event groups (including from ISRs); objects created in FSP's `configuration.xml` count too. Draw the task/object interaction as a diagram.

#### Step 2 — Build and baseline run
- Install e² studio 2026-07, FSP 6.6.0, and LLVM/ATfE 22.1.0. Make sure this doesn't break the build of our demo; how you do that is up to you.
- Import the project zip, build, flash, and run. Note any build or import problems (the README already warns about a `Cannot find smart bundle` issue and a reset needed after flashing in Release builds).
- Reproduce Renesas's 0/1/2-hand timing figures using their own console output.
- Record flash/OSPI, SRAM, and SDRAM usage from the map file.

#### Step 3 — Instrumented benchmark
- Add timing so each stage is measured separately: palm preprocess, palm inference, palm postprocess, landmark preprocess, landmark inference, landmark postprocess, filtering, and display.
- Measure **end-to-end latency** on the chip too, from camera frame capture to the display showing the result for that frame. Timestamp each frame when capture completes (camera/VIN interrupt) and again when the display buffer showing its result is presented (GLCDC). Use a hardware timer through FSP (for example GPT) or the existing `time_counter` module. Agree the method with Duy first.
- Measure how often the detector actually runs in normal use, and CPU load per task if FreeRTOS run-time stats are available.
- List the operators on NPU vs CPU for both models. *Hint:* start from the RUHMI-generated code. `sub_XXXX_command_stream.c` / `sub_XXXX_invoke.c` are subgraphs that run on the Ethos-U55, `compute_sub_XXXX.c` are subgraphs that fall back to the CPU, and `model.c` shows the order they run in. Time each subgraph call separately, including the cache maintenance between them.

**Phase 1 checkpoint with Tu:** code study notes, baseline vs instrumented timings, memory figures. Before the quality test, we'll agree the test scenes.

### Phase 2 — Quality and comparison

#### Step 4 — Quality test
- Record a small internal test set using your own hands. Cover: single hand at 30-80 cm, two hands, fast motion, hands entering and leaving the frame, hands crossing, partial occlusion, varied lighting, and plain vs cluttered background.
- For each scene, note: detection success, time to (re)acquire a hand, tracking losses, wrong-hand swaps, and landmark jitter (visual, plus keypoint variance on a still hand if you can log it).
- Try a few parameter changes (detector interval, 1€ filter settings) and show the latency vs robustness trade-off.

#### Step 5 — Comparison with our demo
- Run the vision pipeline of our demo's FreeRTOS version under the same scenes and lighting. Duy will give you the build and point you to the timing hooks. Both sides run FreeRTOS, so compare task structure, priorities, and RTOS object usage too.
- Compare: model variants (Full/Lite, Sparse/Full), input sizes, per-stage timing, end-to-end latency, memory, tracking strategy, smoothing, and robustness.

## 5. Deliverables

Put all deliverables in the `renesas-hand-landmark-eval` repo:

1. **Evaluation report** (`renesas-hand-landmark-example-evaluation-report.md`), delivered **step by step**, not all at once. When you finish each step, add that step's section to the report, commit it to the repo, tell Tu and Duy, and carry on with the next step. Don't wait for the review; fold in any feedback when it comes.

   | After step | Add this section to the report |
   | :--- | :--- |
   | Step 1 — Study | Pipeline summary in your own words, including tracking and filtering; tunable parameters and defaults; memory placement; FreeRTOS task structure and RTOS objects, with the interaction diagram |
   | Step 2 — Build and baseline run | Build notes and problems; Renesas's 0/1/2-hand figures vs your baseline run; memory table |
   | Step 3 — Instrumented benchmark | Timing table by stage and end to end, for 0/1/2 hands; detector run rate; NPU vs CPU operators |
   | Step 4 — Quality test | Quality results per scene; parameter trade-off (latency vs robustness) |
   | Step 5 — Comparison with our demo | Side-by-side table: Renesas example vs our demo |
   | Final | Findings (gaps, surprises, limitations) and a recommendation: what to adopt from it, what we keep, and what to tell customers who've seen it |

   Keep earlier sections up to date if later steps change what you found.
2. **Instrumented project (full working code):** the complete e² studio project with your timing instrumentation, not just a patch. It must import, build, flash, and run on EK-RA8P1 from a clean checkout, and print the stage-by-stage timings. Include a short README covering the toolchain versions, build steps, and how to read the timing output.
3. **Raw results:** logs, the scene list, and links to the recordings (stored internally only)

## 6. Questions the report must answer

1. Can we reproduce Renesas's 30/38/53 ms figures? If not, why not?
2. What is the real end-to-end latency, from camera frame capture to the result on screen?
3. How much of the speed-up comes from tracking-first, and what does it cost in robustness?
4. Which model operators fall back to the CPU, and how much time do they cost?
5. How does it compare with our demo on speed, memory, and quality? Where are we ahead, and where behind?
6. What should we adopt (tracking-first, 1€ filter, shared tensor arena, model variants), and what would it take to bring it into the FreeRTOS version of our demo?

## 7. Constraints and notes

- **Licensing:** the code is BSD-3-Clause and the models are Apache-2.0. Read and test freely. Before copying any code or models into Gnomons firmware, flag it to Tu so we keep attribution and notices right.
- **Recordings:** use your own hands only. Keep recordings internal. Do not publish them or use them in customer demos.
- **Toolchain versions:** use exactly e² studio 2026-07, FSP 6.6.0, and ATfE 22.1.0 first. Try other versions only as a separate experiment.
- **Fair comparison:** run both pipelines on the same board, same scenes, same lighting, and same measurement method. Note any difference you can't remove.
- **Record everything:** for every number, record the tool versions, the commit or zip used, the build config (Debug/Release), and how it was measured.

## 8. Working arrangement

- **Direction:** Tu sets scope and technical direction. Bring questions about *what* to measure or *why* to Tu.
- **Review:** Duy reviews the build setup, instrumentation, and each report section as you deliver it. Bring *how* questions and code to Duy's mentoring slot.
- **Checkpoints:** end of Phase 1 (report sections for steps 1-3) and end of Phase 2 (report sections for steps 4-5, plus findings and recommendation). Each step's section is reviewed as it comes in; the checkpoints are for discussing the phase as a whole.
- **Blockers:** raise them straight away; don't wait for the checkpoint. Examples: no EK-RA8P1 board free, build/import failures, no access to our demo build.

## 9. Done when

- [ ] Example built and run on EK-RA8P1, with Renesas's timing figures reproduced (or the gap explained)
- [ ] Stage-by-stage and end-to-end timings, plus memory figures, measured by us
- [ ] Instrumented project delivered as full working code that builds and runs on EK-RA8P1 from a clean checkout
- [ ] Quality test done on our own scenes, with the parameter trade-off shown
- [ ] Side-by-side comparison with our demo completed
- [ ] Report delivered section by section, each section reviewed by Duy
- [ ] Final report discussed with Tu, with a clear recommendation
