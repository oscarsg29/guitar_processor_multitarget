# Terminology

## Purpose

The project display name is **SonicFabric**.

This document defines shared terminology for SonicFabric architecture, requirements, diagrams, and future implementation notes. Use these terms consistently before introducing target-specific vocabulary.

## Product Terms

- **SonicFabric**: The project and product display name for the multi-target guitar audio processor.
- **Processor**: The SonicFabric system that receives guitar audio, applies the configured processing chain, and produces audio output.
- **Guitar Audio Stream**: A continuous audio signal originating from an instrument input and processed in real time.
- **Audio Input**: The source side of the audio path provided by a target.
- **Audio Output**: The destination side of the audio path provided by a target.
- **Effect**: A generic audio-processing unit that transforms or passes audio according to its current state and parameters.
- **Generic Effect**: An effect considered by role and interface only, without committing the architecture to a specific named effect type.
- **Effect Chain**: An ordered sequence of effects applied to the audio stream.
- **Effect Slot**: A position in the effect chain that contains an effect and its chain-specific state.
- **Bypass State**: The state indicating whether an effect slot is bypassed while remaining present in the chain.
- **Enabled State**: The state indicating whether an effect slot is active and available for processing.
- **Parameter**: A controllable value exposed by an effect or product feature.
- **Parameter Value**: The current value assigned to a parameter.
- **Parameter Metadata**: Descriptive information about a parameter, such as valid range, default value, unit, and control behavior.
- **Preset**: A saved product configuration containing chain order, effect states, and parameter values.
- **Active Preset**: The preset currently applied to the product model.
- **Stored Preset**: A preset available from a persistence mechanism but not necessarily active.
- **Chain Configuration**: The product data that describes effect order, effect state, and parameter values for the effect chain.
- **Validated Chain Configuration**: A chain configuration that has passed product validation and is acceptable for use by the DSP engine.
- **Use Case**: A user-facing workflow that describes an actor goal and links to related requirements without duplicating the requirement text.

## Architecture Terms

- **Product Model**: The target-neutral representation of SonicFabric product behavior, including presets, chains, effect configuration, parameter semantics, and user-visible state.
- **Shared Core**: The target-neutral SonicFabric code and design responsibilities shared by all targets.
- **Preset Manager**: The product responsibility for loading, selecting, applying, and reporting preset state.
- **Chain Manager**: The product responsibility for maintaining effect-chain order, effect-slot state, and validated chain updates.
- **Parameter Model**: The product responsibility for parameter values, parameter metadata, and validation rules.
- **Effect Registry**: The product or DSP responsibility that identifies the generic effects available to a configuration.
- **DSP Engine**: The target-neutral audio-processing core responsible for audio buffers, effect-chain execution, effect lifecycle, bypass behavior, and parameter application.
- **Audio Buffer Model**: The shared representation of audio sample blocks or streams used by the DSP engine.
- **Effect Chain Executor**: The DSP responsibility for applying the validated effect chain to audio.
- **Effect Lifecycle**: The shared states and transitions an effect follows before, during, and after processing.
- **Bypass Policy**: The shared rule for how audio behaves when an effect slot is bypassed.
- **Processing Health**: DSP-related runtime health information, such as processing status, rejected updates, overload indicators, or fault/fallback signals.
- **Diagnostics Facade**: The shared interface used by product and DSP logic to report diagnostics without depending on a target-specific diagnostics mechanism.
- **Platform Port**: A target-neutral interface required by shared product or DSP code.
- **Resource Capability Port**: The platform port that reports resource availability or capability information when needed by shared behavior.
- **Time Or Sample-Clock Port**: The platform port that provides timing or sample-clock information needed by shared behavior.
- **Target Adapter**: A target-specific implementation of one or more platform ports.
- **Target Mechanism**: A concrete platform facility, such as a device driver, RTOS API, desktop audio API, GUI framework, filesystem, logging backend, or hardware peripheral.
- **Audio I/O Port**: The platform port that provides audio input and output services to the shared processing model.
- **Control Port**: The platform port that provides control input to the product model.
- **Persistence Port**: The platform port that provides storage and retrieval services for presets or other persistent product state.
- **Diagnostics Port**: The platform port that reports diagnostics, logging, tracing, and runtime health information.
- **Configuration Validator**: The product component or responsibility that checks presets, chain configuration, effect availability, and parameter values before they are applied.
- **Runtime State**: The current externally observable state of the processor, including whether it is configured, running, stopped, or in fallback/fault handling.
- **Processor State**: The lifecycle state of the processor, such as uninitialized, configured, running, stopped, or fault/fallback.
- **Runtime Health**: The observable condition of the running system, including processing status, rejected updates, configuration errors, overload indicators, or target-specific health signals.
- **Fault Or Fallback State**: A processor state entered when the current configuration, runtime condition, or processing state cannot continue normally and the system must recover, continue in a reduced mode, or stop.

## Target Terms

- **Target**: A supported execution environment for SonicFabric.
- **Desktop Target**: A SonicFabric target running on a desktop operating system. Its hardware and runtime requirements should remain minimal unless product requirements justify more complexity.
- **Bare Metal Target**: A SonicFabric target running directly on hardware without a general-purpose operating system.
- **RTOS Target**: A SonicFabric target running on a real-time operating system.
- **Target-Specific Requirement**: A requirement that applies to one target and should be documented separately from the common product requirements unless it changes common product behavior.
- **Target Allocation View**: An architecture view showing how shared SonicFabric elements map to one target's adapters, runtime, and hardware mechanisms.
- **Interface Contract**: A behavior-level agreement between components or between shared code and platform ports. It describes ownership, consumers, required behavior, invariants, and failure behavior without defining implementation signatures.

## Excluded Terms For Current Scope

- **Cabinet Modeling**: Explicitly outside the current base architecture scope.
- **Amplifier Modeling**: Explicitly outside the current base architecture scope.
- **Named Effect Type**: A specific effect such as delay, reverb, modulation, distortion, filter, or compressor. The current base architecture should use generic effects without committing to specific named effects.

## Deferred Feature Terms

- **Tuner**: A possible future feature for estimating and presenting instrument pitch. Its product behavior, signal-path interaction, and target exposure are open topics.
- **Metronome**: A possible future feature for producing or presenting timing reference events. Its timing model, audio/control interaction, and target exposure are open topics.
