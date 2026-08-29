# Architecture and Design Principles

## Purpose

The project display name is **SonicFabric**.

This document defines the base architecture direction for SonicFabric, a multi-target guitar processor. The same product concept must support bare metal, RTOS, and desktop builds while preserving a common architecture vocabulary and a shared audio-processing model.

The architecture guidance follows the framing from *Software Architecture in Practice*: architecture is driven by the system structures that matter, the externally visible behavior of elements, the relations between those elements, and the quality attributes that the product must achieve. This document therefore focuses on architecturally significant requirements, not implementation details for any one target.

The product and software requirements are maintained in [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md). Target-neutral behavior and runtime diagrams are maintained in [ArchitectureViews.md](ArchitectureViews.md). Target-neutral structural diagrams are maintained in [StructuralArchitectureViews.md](StructuralArchitectureViews.md). Shared terminology is defined in [Terminology.md](Terminology.md).

## Document Ownership

- This file owns architecture direction, scope boundaries, design principles, architectural drivers, and open topics.
- `ProductAndSoftwareRequirements.md` owns requirement statements and `SFAB-*` requirement labels.
- `ArchitectureViews.md` owns behavior and runtime Mermaid architecture diagrams.
- `StructuralArchitectureViews.md` owns static shared component, module dependency, and port boundary Mermaid architecture diagrams.
- `Terminology.md` owns shared vocabulary.
- `AGENTS.md` owns guidance for future agents and should reference canonical documents rather than duplicate their content.

## Product Scope

The product is a real-time guitar audio processor that accepts an instrument input signal, applies a configurable chain of generic effects, and produces an audio output suitable for monitoring, recording, or integration with a host environment.

The initial scope includes:

- Real-time audio input, processing, and output.
- A configurable sequence of generic audio effects.
- Runtime control of effect parameters.
- Preset storage and recall.
- A shared processing model usable across bare metal, RTOS, and desktop versions.

The initial scope excludes:

- Cabinet modeling.
- Amplifier modeling.
- Commitment to specific named effects.
- Target-specific driver, scheduler, GUI, storage, or deployment details.

## Stakeholders

- Player: needs responsive, predictable sound changes while playing.
- Preset author: needs repeatable parameter editing, ordering, and recall.
- Developer: needs portable core logic, testable DSP behavior, and clear module boundaries.
- Integrator: needs target-specific adaptation points without changing common product behavior.
- Maintainer: needs a design where new effects, controls, and targets can be added without broad rewrites.

## Architectural Drivers

The initial architectural drivers are:

- Low-latency real-time audio processing.
- A reorderable generic effect chain.
- Shared product behavior across bare metal, RTOS, and desktop versions.
- Isolation of platform-specific services from reusable DSP and product logic.
- Deterministic testing of core behavior outside live hardware.
- Future extensibility for additional effect types and control surfaces.

## Base Software Structure

The architecture should be described using a small number of useful views:

- Product view: the common user-visible concepts such as presets, chains, effects, parameters, and bypass state.
- Module view: the main responsibilities and dependencies between common DSP logic, product configuration, control handling, and platform adaptation.
- Uses/layer view: the allowed dependencies between product logic, DSP logic, platform abstractions, and target implementations.
- Runtime view: the high-level flow from audio input through effect-chain processing to audio output, including runtime control updates.
- Allocation view: the boundary between shared code and each target's platform services.

These views should stay abstract until a target-specific design decision needs to be made. The goal is to support reasoning about quality attributes, not to document every class, task, interrupt, thread, or API.

## Layering Direction

The architecture should use layers to protect shared product and DSP behavior from target-specific mechanisms. The initial layering direction is:

- Product model: presets, chains, effect configuration, parameter semantics, and common user-visible behavior.
- DSP engine: audio buffer processing, effect-chain execution, effect lifecycle, bypass behavior, and parameter application.
- Platform ports: target-neutral interfaces required by the shared product and DSP layers.
- Target adapters: bare metal, RTOS, and desktop implementations of platform ports.

Dependencies should point toward stable abstractions. Product and DSP code may depend on platform ports, but should not depend directly on device drivers, RTOS APIs, desktop audio APIs, GUI frameworks, filesystems, or logging backends.

## Ports and Adapters

Ports and adapters are the preferred boundary pattern for supporting multiple targets from one product architecture.

Initial platform ports to consider:

- Audio input and output.
- Control input.
- Preset persistence.
- Time or sample-clock services.
- Diagnostics, logging, and tracing.
- Runtime health reporting.
- Resource capability reporting.

Target adapters should satisfy these ports using mechanisms appropriate to the target. Desktop adapters should remain simple unless the product requirements demand more complexity. Bare metal and RTOS adapters may need more detailed treatment because hardware timing, memory limits, scheduling, fault handling, and peripheral integration are likely to become architecturally significant.

## Interface Contracts

The shared architecture should define contracts before target-specific design begins for:

- Effect lifecycle.
- Effect-chain configuration.
- Parameter metadata and validation.
- Preset content and compatibility.
- Audio buffer format.
- Platform service ports.
- Diagnostics and error reporting.

These contracts should describe required behavior and invariants, not implementation mechanisms.

## Target Requirement Direction

Base requirements describe common product behavior. Target-specific software and hardware requirements should be documented separately when each target is designed.

Expected future target requirement documents:

- `TargetRequirements/DesktopRequirements.md`
- `TargetRequirements/BareMetalRequirements.md`
- `TargetRequirements/RTOSRequirements.md`

Target-specific requirements may constrain implementation choices, but they should not fork common product behavior unless the difference is explicitly accepted and documented.

## Design Principles

- Keep the audio-processing path explicit and small.
- Treat effects as interchangeable processing units with a common lifecycle and parameter contract.
- Represent the effect chain as ordered configuration so product behavior is not tied to compile-time ordering.
- Keep platform services outside the core processing model.
- Use ports and adapters to keep target-specific mechanisms outside shared product and DSP logic.
- Define layering rules and allowed dependencies before implementation grows around implicit coupling.
- Prefer deterministic state transitions over implicit shared mutable state.
- Make quality-attribute tradeoffs visible when choosing abstractions, buffering, memory ownership, or control-update mechanisms.
- Document only architectural structures that help reason about product behavior, latency, portability, modifiability, reliability, or testability.
- Do not introduce amp or cab modeling assumptions into the base architecture.

## Initial Architectural Decisions

- The product will use a generic effect-chain model.
- Effects will be reorderable in the chain.
- Presets will describe product state rather than platform-specific storage details.
- The common architecture will support bare metal, RTOS, and desktop targets through platform adaptation boundaries.
- The common architecture will use platform ports and target adapters to separate shared behavior from target mechanisms.
- Target-specific software and hardware requirements will be documented separately from common product requirements.
- The base architecture will not select specific effects, cab modeling, or amp modeling.

## Open Topics

- Concrete latency budgets.
- Supported audio sample rates and block sizes.
- Parameter smoothing policy.
- Preset format and compatibility policy.
- Error handling and recovery policy.
- User interface and control model.
- Control input sources.
- State synchronization between audio processing and control changes.
- Logging, tracing, and diagnostics policy.
- Test strategy across unit, integration, simulation, and target validation.
- Layering rules and allowed dependencies.
- Port and adapter boundaries for platform services.
- Module decomposition view.
- Runtime interaction view for audio, control, and preset changes.
- Interface contracts for effects, parameters, presets, audio buffers, and platform services.
- Quality attribute scenarios and architectural tactics.
- Tuner scope, behavior, signal interaction, and target exposure.
- Metronome scope, timing behavior, audio/control interaction, and target exposure.
- Base hardware assumptions and target capability profiles.
- Desktop minimum runtime environment.
- Bare metal hardware capability requirements.
- RTOS hardware and scheduling capability requirements.
- Target-specific software requirements for desktop, bare metal, and RTOS.
- Target-specific architecture views for bare metal, RTOS, and desktop.
- Amp and cab modeling scope.
