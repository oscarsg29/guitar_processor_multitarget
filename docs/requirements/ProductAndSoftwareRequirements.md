# Product and Software Requirements

## Purpose

The project display name is **SonicFabric**.

This document defines the base product and software requirements for SonicFabric, a multi-target guitar processor. Requirements are intentionally target-neutral unless a target constraint is architecturally significant.

Requirement labels are stable identifiers. They should be used when referencing requirements from architecture decisions, tests, design notes, or target-specific documents.

Shared terminology is defined in [Terminology.md](../Terminology.md). User-facing workflows are defined in [UseCases.md](UseCases.md). Architecture-driving quality scenarios are defined in [QualityAttributeScenarios.md](../architecture/QualityAttributeScenarios.md). Behavior-level interface contracts are defined in [InterfaceContract.md](../architecture/InterfaceContract.md).

## Product Requirements

- **SFAB-PR-001**: The processor shall process a continuous guitar audio stream with bounded and predictable latency.
- **SFAB-PR-002**: The processor shall support a chain of generic effects that can be enabled, disabled, bypassed, and reordered.
- **SFAB-PR-003**: The processor shall allow each effect to expose parameters through a common parameter model.
- **SFAB-PR-004**: The processor shall support presets that capture the active chain order, effect states, and parameter values.
- **SFAB-PR-005**: The processor shall allow preset recall without requiring a restart of the audio-processing engine.
- **SFAB-PR-006**: The processor shall provide a way to observe essential runtime state, including active preset, chain configuration, and processing health.
- **SFAB-PR-007**: The processor shall separate product behavior from target-specific I/O, storage, timing, and user-interface mechanisms.
- **SFAB-PR-008**: The processor shall define stable behavior for invalid configurations, missing effects, unsupported presets, and parameter values outside valid ranges.
- **SFAB-PR-009**: The processor shall support operation without cab or amp modeling in the initial architecture.

## Base Hardware and Runtime Requirements

- **SFAB-HWR-001**: Each target shall provide an audio input and audio output path suitable for real-time guitar processing.
- **SFAB-HWR-002**: Each target shall provide enough processing capacity for the configured effect chain.
- **SFAB-HWR-003**: Each target shall provide memory capacity suitable for the active audio buffers, effect state, preset state, and platform adaptation code.
- **SFAB-HWR-004**: Each target shall provide at least one control mechanism for preset selection, effect bypass, chain configuration, or parameter editing.
- **SFAB-HWR-005**: Each target shall provide a persistence mechanism when preset storage and recall are enabled.
- **SFAB-HWR-006**: Each target shall provide a diagnostics mechanism appropriate to its environment.
- **SFAB-HWR-007**: Desktop hardware and runtime requirements shall remain minimal unless product requirements justify additional complexity.
- **SFAB-HWR-008**: Bare metal and RTOS hardware requirements may include more detailed constraints for audio codec integration, clocking, memory limits, control hardware, diagnostics, watchdog behavior, reset behavior, and power behavior.
- **SFAB-HWR-009**: Target-specific hardware requirements shall not change common product behavior unless the difference is explicitly accepted and documented.

## Software Quality Requirements

### Performance

- **SFAB-SQR-PERF-001**: Audio processing shall be suitable for real-time use.
- **SFAB-SQR-PERF-002**: The architecture shall make the audio path explicit so latency, memory use, and processing cost can be reasoned about.
- **SFAB-SQR-PERF-003**: The processing path shall avoid unbounded work during active audio processing.

### Modifiability

- **SFAB-SQR-MOD-001**: Effects shall be added, removed, or replaced through stable interfaces.
- **SFAB-SQR-MOD-002**: Effect ordering shall be represented as data or configuration, not as fixed control flow.
- **SFAB-SQR-MOD-003**: Target-specific code shall be isolated behind platform adaptation boundaries.
- **SFAB-SQR-MOD-004**: Changes to one target shall not require changes to the shared DSP model unless the product behavior changes.

### Portability

- **SFAB-SQR-PORT-001**: The core processing model shall be independent of a specific operating system, scheduler, audio API, filesystem, or user-interface framework.
- **SFAB-SQR-PORT-002**: Platform code shall provide required services through explicit interfaces.
- **SFAB-SQR-PORT-003**: The architecture shall support bare metal, RTOS, and desktop builds without requiring separate product definitions.

### Testability

- **SFAB-SQR-TEST-001**: Core DSP and chain behavior shall be testable without live audio hardware.
- **SFAB-SQR-TEST-002**: Preset loading, parameter validation, bypass behavior, and effect reordering shall be testable through deterministic inputs and outputs.
- **SFAB-SQR-TEST-003**: The architecture shall support automated regression tests for processing behavior and configuration behavior.

### Reliability

- **SFAB-SQR-REL-001**: The system shall define predictable behavior when configuration data is invalid or incomplete.
- **SFAB-SQR-REL-002**: The audio path shall favor bounded execution, explicit ownership, and deterministic state transitions.
- **SFAB-SQR-REL-003**: Runtime control updates shall not corrupt the active processing state.

### Usability

- **SFAB-SQR-USE-001**: Parameter changes, bypass changes, and chain reordering shall have clear observable results.
- **SFAB-SQR-USE-002**: Preset behavior shall be consistent across supported targets, even when each target exposes different control surfaces.
- **SFAB-SQR-USE-003**: The product model shall avoid target-specific terminology in common user-facing concepts.
