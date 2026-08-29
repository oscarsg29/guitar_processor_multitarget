# Project Development Guide

## Purpose

The project display name is **SonicFabric**.

This guide defines a step-by-step development procedure for SonicFabric. It combines the project direction already captured in the architecture documents with practical guidance from the reference books listed at the end of this file.

The guide is intentionally staged. SonicFabric should first stabilize common product behavior and shared architecture, then move into target-specific design for desktop, bare metal, and RTOS.

## Guiding Approach

Use this stack of methods:

- Attribute-driven architecture design for deciding what to build next.
- Views-based architecture documentation for communicating the design.
- Interface-first design for preserving portability across targets.
- Test-first validation for shared behavior before hardware-specific validation.
- Target-specific refinement only after shared behavior is stable.

## Step 1: Maintain Project Scope

Keep the base scope clear before expanding features.

Inputs:

- [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md)
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [Terminology.md](Terminology.md)

Actions:

- Confirm whether a requested change is current scope, open topic, deferred feature, or explicitly excluded.
- Keep cabinet modeling and amplifier modeling out of the base architecture until the user reopens that scope.
- Keep tuner and metronome as open topics until they are promoted into requirements.
- Avoid committing to specific named effects while the architecture is still generic.

Exit criteria:

- The change has a clear place in the document set.
- Scope boundaries remain explicit.

## Step 2: Capture Or Refine Requirements

Use requirements to describe product behavior and quality needs before selecting mechanisms.

Inputs:

- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [Terminology.md](Terminology.md)

Actions:

- Add target-neutral requirements to the common requirements file.
- Assign stable `SFAB-*` labels.
- Separate product requirements, base hardware/runtime requirements, and software quality requirements.
- Keep target-specific requirements out of the common file unless they affect common behavior.
- When target-specific detail becomes necessary, create the appropriate target file under `TargetRequirements/`.

Exit criteria:

- Requirements are labeled, testable at the appropriate level, and not tied to an unnecessary target mechanism.

## Step 3: Identify Architectural Drivers

Use architectural drivers to decide which requirements shape the architecture.

Inputs:

- [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md)
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)

Actions:

- Identify requirements that strongly influence structure, interfaces, timing, portability, or testability.
- Keep low-latency real-time audio, reorderable generic effect chains, portability, testability, and platform isolation visible as primary drivers.
- Record new drivers only when they materially shape architecture decisions.

Exit criteria:

- Important design choices can be traced back to requirements, quality attributes, or explicit constraints.

## Step 4: Define Shared Terminology

Keep vocabulary stable before adding diagrams or contracts.

Inputs:

- [Terminology.md](Terminology.md)

Actions:

- Reuse existing terms before inventing new ones.
- Add new terms when diagrams, requirements, or contracts introduce concepts that could be interpreted more than one way.
- Keep feature names out of current-scope terminology unless they are accepted into scope.

Exit criteria:

- New architecture text does not rely on undefined vocabulary.

## Step 5: Document Shared Behavior Views

Describe expected runtime behavior before target-specific behavior.

Inputs:

- [ArchitectureViews.md](ArchitectureViews.md)
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [Terminology.md](Terminology.md)

Actions:

- Use Mermaid diagrams for target-neutral behavior and runtime views.
- Prefer product concept, runtime flow, state model, preset recall, and effect-chain update diagrams.
- Keep target-specific scheduling, interrupts, tasks, drivers, and deployment out of this file.

Exit criteria:

- The expected shared behavior is understandable without knowing the final target implementation.

## Step 5A: Document Use Cases

Capture user-facing workflows without duplicating requirements or architecture behavior.

Inputs:

- [UseCases.md](UseCases.md)
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [Terminology.md](Terminology.md)

Actions:

- Define actors and `SFAB-UC-*` use cases.
- Link use cases to related requirements.
- Keep use cases target-neutral unless target-specific requirements exist.
- Keep deferred features out of primary use cases until they are promoted into scope.

Exit criteria:

- User-facing workflows are explicit and traceable to requirements.

## Step 6: Document Shared Structural Views

Describe the static shared structure after the behavior is clear.

Inputs:

- [StructuralArchitectureViews.md](StructuralArchitectureViews.md)
- [InterfaceContract.md](InterfaceContract.md)
- [Terminology.md](Terminology.md)

Actions:

- Use Mermaid diagrams for shared components, module dependencies, product model structure, DSP engine structure, and platform port boundaries.
- Keep relationships conceptual.
- Avoid C4 terminology unless the project explicitly reopens that approach.
- Avoid deployment diagrams until target-specific requirements exist.

Exit criteria:

- The main shared components, ownership boundaries, and dependency direction are visible.

## Step 7: Define Interface Contracts

Define what components require and guarantee before defining code-level APIs.

Inputs:

- [InterfaceContract.md](InterfaceContract.md)
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [StructuralArchitectureViews.md](StructuralArchitectureViews.md)

Actions:

- Define contract owner, consumers, related requirements, required behavior, and invariants.
- Keep contracts behavior-level; do not introduce C++ signatures, target APIs, memory layout, tasks, interrupts, or deployment details.
- Use UML-style Mermaid diagrams only to communicate ownership and relationships.
- Keep platform ports target-neutral.

Exit criteria:

- Shared components can be developed against stable behavioral expectations.
- Target adapters know what services they must provide without forcing target mechanisms into shared code.

## Step 8: Define Quality Attribute Scenarios

Turn quality attributes into concrete scenarios that can guide design and evaluation.

Recommended next file:

- `QualityAttributeScenarios.md`

Inputs:

- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md)
- [UseCases.md](UseCases.md)
- [ArchitectureViews.md](ArchitectureViews.md)
- [StructuralArchitectureViews.md](StructuralArchitectureViews.md)
- [InterfaceContract.md](InterfaceContract.md)

Actions:

- Define scenarios for latency, preset recall, invalid configuration, chain update, portability, testability, diagnostics, and recovery.
- Use a consistent structure: stimulus, environment, artifact, response, response measure.
- Prioritize scenarios that affect architecture decisions.

Exit criteria:

- Quality requirements are concrete enough to evaluate design tradeoffs.

## Step 9: Select Architectural Tactics And Decisions

Record decisions after requirements, views, contracts, and scenarios provide enough context.

Recommended future file:

- `ArchitectureDecisionRecords.md` or `ArchitectureDecisions/`

Actions:

- Record decisions with context, options considered, selected option, consequences, and related requirements.
- Capture tactics for performance, modifiability, portability, reliability, and testability.
- Avoid premature decisions that belong to target-specific design.

Exit criteria:

- Major architectural choices are explicit and traceable.

## Step 10: Plan Shared Core Implementation

Start implementation from shared behavior, not from a target.

Actions:

- Define the smallest shared core slice that can be tested without hardware.
- Prefer deterministic tests for presets, chain configuration, parameter validation, and effect-chain behavior.
- Implement generic effect infrastructure before named effects.
- Keep target adapters thin until their requirements are defined.

Exit criteria:

- The shared core can be exercised by tests without a desktop audio API, RTOS, or hardware board.

## Step 11: Establish Test Strategy

Use tests to protect common behavior before target-specific validation.

Recommended future file:

- `TestStrategy.md`

Actions:

- Define unit tests for product model, validation, presets, parameters, and chain behavior.
- Define DSP tests using deterministic audio buffers and known inputs.
- Define integration tests around interface contracts.
- Defer hardware-in-loop, RTOS timing, and desktop audio integration tests until target requirements exist.

Exit criteria:

- Shared behavior is testable without live audio hardware.
- Target validation plans are separated from shared-core tests.

## Step 12: Refine Target-Specific Requirements

Only move into target-specific detail after shared behavior and contracts are stable enough.

Recommended future files:

- `TargetRequirements/DesktopRequirements.md`
- `TargetRequirements/BareMetalRequirements.md`
- `TargetRequirements/RTOSRequirements.md`

Actions:

- Keep desktop requirements minimal unless product behavior requires more.
- Capture bare metal and RTOS constraints when timing, memory, scheduling, hardware, diagnostics, watchdog, reset, or power behavior matter.
- Do not fork shared product behavior unless the difference is explicitly accepted and documented.

Exit criteria:

- Each target has enough requirements to justify adapter and deployment design.

## Step 13: Add Target Architecture Views

Add deployment and allocation views only after target requirements exist.

Recommended future files:

- `TargetArchitectureViews/DesktopArchitectureView.md`
- `TargetArchitectureViews/BareMetalArchitectureView.md`
- `TargetArchitectureViews/RTOSArchitectureView.md`

Actions:

- Map shared core, platform ports, target adapters, target runtime, and hardware mechanisms.
- For bare metal and RTOS, describe timing and resource constraints only when required.
- For desktop, avoid unnecessary complexity.

Exit criteria:

- Each target view explains how the same SonicFabric product behavior is realized on that target.

## Step 14: Iterate And Evaluate

Treat architecture as a working asset, not a one-time document.

Actions:

- Revisit requirements when new features or constraints are introduced.
- Revisit views when interfaces or ownership boundaries change.
- Revisit contracts before implementation details leak across boundaries.
- Use quality attribute scenarios to evaluate whether the design still supports the project goals.

Exit criteria:

- Architecture, requirements, contracts, and tests evolve together.

## Reference Use

- Use **Designing Software Architectures: A Practical Approach** as the step-by-step architecture design process reference.
- Use **Software Architecture in Practice** for architectural drivers, quality attributes, tactics, tradeoffs, and evaluation thinking.
- Use **Documenting Software Architectures: Views and Beyond** for documentation structure, views, interface documentation, and behavior documentation.
- Use **Making Embedded Systems** when refining bare metal and RTOS concerns.
- Use **Designing Audio Effect Plugins in C++** later, when implementing DSP effects or desktop audio prototypes.

## References

- Humberto Cervantes and Rick Kazman, *Designing Software Architectures: A Practical Approach*, Addison-Wesley Professional. https://www.pearson.com/en-us/subject-catalog/p/designing-software-architectures/P200000010265/9780138108151
- Len Bass, Paul Clements, and Rick Kazman, *Software Architecture in Practice*, Addison-Wesley Professional. https://www.pearson.com/en-us/subject-catalog/p/software-architecture-in-practice/P200000000111
- Paul Clements, Felix Bachmann, Len Bass, David Garlan, James Ivers, Reed Little, Paulo Merson, Robert Nord, and Judith Stafford, *Documenting Software Architectures: Views and Beyond*, Addison-Wesley Professional. https://www.sei.cmu.edu/library/documenting-software-architectures-views-and-beyond-second-edition/
- Elecia White, *Making Embedded Systems*, O'Reilly Media. https://www.oreilly.com/library/view/making-embedded-systems/9781098151539/
- Will C. Pirkle, *Designing Audio Effect Plugins in C++: For AAX, AU, and VST3 with DSP Theory*, Routledge. https://www.routledge.com/Designing-Audio-Effect-Plugins-in-C-For-AAX-AU-and-VST3-with-DSP-Theory/Pirkle/p/book/9781138591936
