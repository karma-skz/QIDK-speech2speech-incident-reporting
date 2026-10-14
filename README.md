# Bayaan - An Offline Voice Incident Report Assistant

Bayaan is an Android app built for the Qualcomm QIDK HDK8750 (Snapdragon 8 Elite, SM8750P). It turns a spoken incident description into a structured report, asks follow-up questions for missing details, and saves the reviewed report on the device. After setup, the main flow runs offline.

## Current implementation

- **Speech recognition:** Whisper-Small through sherpa-onnx with QNN on the Hexagon NPU.
- **Report extraction and follow-up:** Qwen3-4B-Instruct-2507, w4a16, through GenieX 0.3.16 on the NPU.
- **Grounding and suggestions:** deterministic on-device rules plus suggestion chips that require user acceptance.
- **Spoken replies:** Android's offline system voice by default. Optional Piper and Supertonic voices run on the CPU and are off by default.
- **Hindi:** an optional Nemotron ASR lane is experimental; Hindi extraction and speech quality are not broadly validated.

Model weights and native runtimes are downloaded and staged separately; they are not stored in this repository. The implementation and evaluation have meaningful limits: the 13-report held-out extraction set has already been used, kit-microphone accuracy is not broadly measured, and signal tags are not validated for urgency. See the progress report for results and caveats.

## Build and run

See the [submission setup guide](docs/submission/README.md) for device requirements, model/runtime setup, build, install, and offline run instructions. The app is under `app/`; shared report logic and tests are under `core/`.

## Project notes

- [Progress report](docs/progress-report.md) - tried models and approaches, what worked, what failed, and the resulting choices.
- [Evaluation results](docs/results-tables.md) - detailed scores and comparisons.
- [Design spec](docs/superpowers/specs/2026-10-10-incident-assistant-design.md) - original product and technical decisions.
- [Feasibility report](exa-results/qidk-incident-assistant-feasibility-2026-10-10.md) - initial platform research and constraints.
- [Qualcomm problem statement](Qualcomm_Problem_Statement_for_MegathonX.pptx).
