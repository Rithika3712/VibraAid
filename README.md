# VibraAid

### VIBRA: Variable-Condition Intelligent Motor Diagnostics Using Edge AI

A proposed hardware–software system for monitoring induction motors
under changing operating conditions using multimodal sensing and
local Edge-AI analysis.

---

## Project Information

| Field | Details |
|---|---|
| Project Name | VibraAid |
| Technical Title | VIBRA: Variable-Condition Intelligent Motor Diagnostics Using Edge AI |
| Competition | Vishwakarma Awards 2026–27 |
| Application ID | VK26-IITI-AHM-0402 |
| Theme | AI in Hardware & Manufacturing |
| Sub-Theme | Predictive Maintenance & Machine Diagnostics |
| Development Status | Design-stage project; prototype validation pending |

This repository documents the project design and will host original
engineering files, implementation code, and experimental evidence as
they become available.

## Problem Statement

Induction motors operate under changing loads, speeds, and electrical
conditions. These changes can produce signal variations that resemble
faults, creating uncertainty in maintenance decisions.

VibraAid aims to distinguish normal operating variation from abnormal
motor behaviour by interpreting sensor measurements alongside the
motor's operating context.

## Proposed Solution

The proposed system will:

- Acquire motor vibration and surface-temperature measurements.
- Read available operating-context information from a compatible VFD.
- Extract features and evaluate motor behaviour locally.
- Identify deviations from learned healthy operating conditions.
- Provide advisory status, supporting measurements, and event records.

Reduced false alarms and earlier maintenance intervention are project
objectives—not validated results.

## Proposed System Architecture

```text
ADXL345 vibration sensor ── SPI ────────────────┐
                                               │
DS18B20 temperature probe ── 1-Wire ────────────┤
                                               ▼
Compatible VFD ── Isolated RS485 ── UART ── ESP32-S3
                                               │
                                               ▼
                                     Data-quality checks
                                               │
                                               ▼
                             Features + operating context
                                               │
                                               ▼
                              Condition-aware anomaly scoring
                                               │
                                               ▼
                              Advisory status + event logging
                                               │
                                               ▼
                                        Local dashboard
```

### Measurement Scope

- Vibration is proposed to be measured using an ADXL345 accelerometer.
- Temperature is proposed to be measured at a repeatable motor-surface
  location using a DS18B20 probe.
- VFD telemetry will provide only the parameters supported by the
  selected drive and its communication interface.
- VFD output frequency is not a direct measurement of shaft speed.
- Slow VFD telemetry is not high-rate electrical waveform acquisition.
- Direct current/voltage waveform sensing is outside this preliminary
  implementation unless separately engineered and documented.

## Proposed Hardware

| Component | Role |
|---|---|
| ESP32-S3-DevKitC-1 N8R8 | Embedded acquisition and processing |
| ADXL345 breakout | Three-axis vibration sensing |
| DS18B20 probe | Motor-surface temperature trends |
| Isolated TTL-to-RS485 converter | Compatible VFD telemetry |
| Regulated low-voltage supply | Monitoring electronics |
| Enclosure and rigid sensor bracket | Electronics mounting and repeatable sensing |

Component availability, exact variants, power requirements, and
compatibility must be confirmed before assembly.

The induction motor, VFD, loading arrangement, guarding, and reference
instruments are separate test infrastructure.

## Proposed Software Stack

- Embedded firmware: C/C++ using ESP-IDF or the Arduino framework
- Data preparation and analysis: Python
- Feature processing: NumPy and SciPy
- Model evaluation: scikit-learn and/or TensorFlow
- Embedded model runtime: LiteRT for Microcontrollers, if suitable
- Monitoring interface: Streamlit

These are planned implementation choices. Dependencies and setup
instructions will be added when runnable code is committed.

## Diagnostic Workflow

1. Acquire sensor measurements and operating-context data.
2. Check data validity, missing samples, clipping, and telemetry age.
3. Identify the current operating condition.
4. Extract vibration and temperature features.
5. Compare behaviour against a validated healthy baseline.
6. Apply persistence checks before issuing an advisory.
7. Record supporting measurements for technician review.

### Candidate Features

- RMS acceleration
- Crest factor
- Kurtosis
- Selected spectral-band energy
- Temperature trend
- Available VFD-reported operating parameters

### Model Development

The initial approach will evaluate healthy-data anomaly detection.

A lightweight statistical detector will serve as a baseline. A small
feature-based model may be deployed on the controller after memory,
latency, and detection performance are evaluated.

No trained model or diagnostic accuracy is claimed at this stage.

## Proposed Advisory States

| State | Meaning |
|---|---|
| Within Learned Range | Valid measurements fall within the evaluated baseline |
| Inspection Advised | Persistent abnormal behaviour requires technician review |
| Sensor Fault | Required sensor data is missing or invalid |
| Context Unavailable | Operating-context telemetry is missing or stale |
| Outside Validated Range | The operating condition is outside evaluated coverage |

“Within Learned Range” does not guarantee that the motor is fault-free.

## Mechanical Design

The proposed mechanical arrangement includes:

- A serviceable electronics enclosure
- Insulating controller standoffs
- A rigid vibration-sensor mount near the selected bearing housing
- Repeatable temperature-probe attachment
- Secured cables and strain relief
- Clear access to motor cooling and maintenance points

Original CAD, dimensions, material choices, and mechanical validation
will be documented when available.

AI-generated illustrations are conceptual visuals, not CAD exports,
fabrication evidence, or stress-analysis results.

## Repository Organization

Suggested structure; folders will be added as genuine work is available:

```text
VibraAid/
├── README.md
├── .gitignore
├── docs/              # Proposal, presentation and design notes
├── hardware/
│   ├── cad/           # Original models and dimensioned drawings
│   ├── schematics/    # Native EDA files and schematic exports
│   └── bom/           # BOM, supplier links and dated quotations
├── firmware/          # Embedded acquisition and processing
├── software/
│   ├── analysis/      # Feature extraction and evaluation
│   └── dashboard/     # Monitoring interface
├── models/            # Model definitions and validated artifacts
├── data/              # Shareable recordings and metadata
├── tests/             # Acquisition and integration checks
└── evidence/          # Genuine progress screenshots and photographs
```

## Validation Plan

The prototype will be evaluated under approved laboratory conditions.

| Evaluation Area | Planned Metric |
|---|---|
| Healthy operating variation | False alerts per operating hour |
| Abnormality detection | Event-level precision and recall |
| Condition awareness | Comparison with a fixed-threshold baseline |
| Processing performance | Inference and end-to-end alert latency |
| Acquisition reliability | Missing samples and communication errors |
| Repeated installation | Measurement consistency after remounting |

### Data Integrity

- Preserve original recordings and acquisition metadata.
- Record motor, load, speed/context, sensor, and mounting information.
- Split data by independent recording run.
- Keep overlapping windows from one run in the same partition.
- Keep test data separate from training and threshold selection.
- Clearly distinguish measured results, targets, and illustrative data.

## Development Roadmap

| Phase | Planned Activities | Deliverable |
|---|---|---|
| Month 1 | Finalize requirements, hardware, CAD, schematic and safety review | Reviewed design package |
| Month 2 | Assemble sensing unit and verify acquisition | Working acquisition setup and baseline recordings |
| Month 3 | Develop model, deploy inference and integrate dashboard | Integrated monitoring prototype |
| Month 4 | Conduct supervised validation and document limitations | Demonstration and evaluation package |

This is a proposed schedule, not a record of completed milestones.

## Current Status

- Project concept and preliminary architecture documented.
- Proposed hardware and implementation approach identified.
- Prototype assembly, data acquisition, model training, and performance
  validation remain pending confirmation.

Only dated, committed artifacts should be treated as evidence of
completed work.

## Safety and Limitations

VibraAid is an experimental advisory-monitoring project.

- It does not replace certified motor protection, overload protection,
  emergency stops, or machine interlocks.
- It must not automatically control motor operation in this initial design.
- Motor/VFD work requires qualified supervision and approved laboratory
  procedures.
- Isolate equipment before installing or adjusting sensors.
- Keep cables, mounts, and enclosures clear of rotating components.
- Preserve the communication isolation barrier.
- Do not treat a prototype breadboard as an industrial installation.
- Do not create unsafe faults to collect training data.
- Detection coverage is limited to the conditions actually evaluated.

No safety certification, ingress-protection rating, diagnostic accuracy,
downtime reduction, or maintenance-cost saving is claimed.

## Documentation and Evidence

As work progresses, this repository will include:

- Original CAD and schematic files
- BOM with exact variants and dated sourcing information
- Firmware and model-development code
- Genuine design and assembly evidence
- Experimental recordings and metadata
- Evaluation procedures and measured results

AI-assisted visuals will be labelled “AI Generated.” Progress evidence
must represent actual team work.

## References

### Project Concept and Methodology

- [Induction Motor Fault Diagnosis and Condition Monitoring Review](https://www.mdpi.com/1424-8220/25/19/5942)
- [Review of Fault Diagnosis Methods for Induction Machines](https://www.mdpi.com/1996-1073/17/11/2728)
- [Edge Application of Machine Learning for Motor Diagnostics](https://www.mdpi.com/1424-8220/23/5/2649)

### Hardware and Implementation

- [ESP32-S3-DevKitC-1 Documentation](https://docs.espressif.com/projects/esp-dev-kits/en/latest/esp32s3/esp32-s3-devkitc-1/index.html)
- [ADXL345 Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/adxl345.pdf)
- [DS18B20 Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ds18b20.pdf)
- [Waveshare Isolated TTL-to-RS485 (B)](https://www.waveshare.com/wiki/TTL_TO_RS485_(B))
- [LiteRT for Microcontrollers](https://ai.google.dev/edge/litert/microcontrollers)
- [Robu.in Component Sourcing](https://robu.in/)

Consult the manual for the exact VFD and interface-module variant used.
External references support the design rationale; they do not establish
VibraAid's measured performance or originality.

## Licensing

No open-source license has been selected for this repository at present.
Do not assume unrestricted reuse permission.

Third-party code, documentation, and images remain subject to their
respective licenses and attribution requirements.