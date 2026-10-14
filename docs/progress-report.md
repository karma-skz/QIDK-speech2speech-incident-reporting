# Progress report

**Evidence through 11 October 2026.** The selected offline path is Qwen3-4B-Instruct-2507 on the QIDK NPU, Whisper-Small through sherpa-onnx/QNN, and CPU rules with user-confirmed suggestions. Most results below are development or small-sample measurements.

## Models and speech engines

| Tried | What worked or failed | Outcome |
|---|---|---|
| **Qwen3-4B-Instruct-2507**, w4a16 GenieX on NPU | v11 scored 43/65 (66% recall), 91% precision and 2/39 hallucinations on the 13 held-out reports; v8 scored 65%, too close to distinguish. The recall patch raised dev recall 47%→65%; v11 reached 74% on dev. | Primary model. GenieX 0.3.5 corrupted Devanagari; the app moved to 0.3.16. Hindi extraction remains weak and sparsely measured. |
| **Qwen3-8B**, w4a16 GenieX on NPU | Dev recall 83%, precision 90%, hallucination 9%, median turn 7.9 s. | Failed speed and hallucination gates; not selected. |
| **Qwen3-1.7B Q4_0**, CPU / llama.cpp | Kit measurement: 62–64% recall, 91% precision, 2.4 s median; NPU use unproven. A separate laptop equipment helper found 17/19 dev items, but gave 19 false alarms on B’s 134 synthetic reports. | User cancelled the second-model CPU chain; not shipped. |
| **1.7B and 4B LoRA fine-tunes**, GPU | 1.7B: 73% recall, 79% precision, 19/81 hallucinations. 4B: 90% recall, 91% precision, 10/81 hallucinations. Neither cleared the gate or was exported. | Not adopted. The 800-row synthetic set was later rejected for near-duplicate overlap; retained only as a parser fixture. |
| **Whisper-Small QNN** on kit; **WhisperKit** inspected | Whisper-Small ran on the NPU: one clean 6.6 s clip decoded in 3.18 s; an app-path sample matched the CLI. WhisperKit API was inspected, but its device/NPU path was not proven. | sherpa-onnx/QNN selected; no robust kit-microphone accuracy score yet. |
| **Whisper base.en, tiny.en, and Indian-English fine-tunes** | Stock base.en ranked best in small read-speech tests: 12.30% WER on an 80-clip public proxy and 8.72% on 10 phone clips. Fine-tunes did not beat it. | GPU/laptop comparisons only; they do not rank the kit’s Whisper-Small model on incident speech. |
| **SenseVoice int8 QNN**, **YAMNet**, **Nemotron-3.5 Hindi ASR** | SenseVoice ran quickly, but English output had 3 errors/15 words, both language tags were wrong, and emotion clips were neutral only. YAMNet proxy checks found raw logits need a sigmoid; rain/horn false positives remain. Nemotron completed 19/19 Hindi/4B loops; one Hindi clip had one word mismatch. | Audio tags are not validated for urgency. Hindi ASR is experimental; none has broad real-kit accuracy evidence. |
| **Supertonic 3** and **Piper** TTS | Supertonic completed Hindi/English technical runs at 0.26–0.39 real-time factor. Piper compiled but was never run or heard on the kit. | Android system voice stays default; voice naturalness and Hindi wording lack native-speaker review. |

## Other results and decisions

- The 4B **FocusedProbe** recovered 13 additional equipment values from 19 dev cases, for 16/19 total, with 1 wrong, 2 missed and 3 false-alarm chips; median added time was 1.1 s. It failed A’s stricter dev gate, but the user kept the equipment/action/people probe and witness-presence chips on by default. Chips require user acceptance. D’s action rules found 13 values with no wrong or false-alarm chips in dev. The English place snapper had no wrong or false alarms in 55 known-site examples; synthetic Hindi/Telugu spellings got 8/10 in each language, but were not recognizer output or native-reviewed.
- Date parsing fixes changed 16 of 114 independent date values, all in the right direction. The urgency advisor scored 96% on tuned dev text but 45–55% on small holdouts, with 25% under-triage; it remains advice and is not validated for speech.
- Consensus-based automatic filling was not adopted: its precision was 88% on an independent set, above the project’s false-fill limit. The 800-row training set was abandoned because near-duplicate phrases crossed its train/validation/test splits.
- The 13 held-out extraction reports have already been used. Current scores are not a fresh blind evaluation; Hindi/Telugu ASR, noisy incident audio, and full final-build kit verification remain open.

Detailed evidence: [results tables](results-tables.md), [kit model comparison](experiments/A-20261011-model-comparison-on-kit.md), [QNN speech test](experiments/A-20261010-sherpa-onnx-qnn-speech-on-kit.md), [ASR proxy](experiments/B-20261011-accent-contest-public-proxy.md), [fine-tune](experiments/C-20261011-finetune-qwen3-4b-v1.md), [dataset overlap](experiments/B-20261011-dataset-overlap/RESULT.md), [Hindi/TTS trial](experiments/A-20261011-hindi-lane-and-supertonic-on-kit.md), and [YAMNet/SenseVoice probe](experiments/B-20261011-signals-probe.md). The user’s decisions to keep the probe and skip the 1.7B chain are in preserved `origin/communication` messages A/040 and E/016.
