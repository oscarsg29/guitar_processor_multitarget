# Agent Guide

## Project Identity

- Project display name: **SonicFabric**
- Requirement label prefix: `SFAB`
- Repository purpose: multi-target guitar audio processor for bare metal, RTOS, and desktop versions.

## Canonical Documents

- [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md): architecture direction, scope boundaries, design principles, architectural drivers, target strategy, and open topics.
- [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md): product requirements, base hardware/runtime requirements, software quality requirements, and all stable `SFAB-*` requirement labels.
- [UseCases.md](UseCases.md): user-facing workflows with `SFAB-UC-*` labels and links to related requirements.
- [ArchitectureViews.md](ArchitectureViews.md): target-neutral Mermaid diagrams for product concepts, runtime flow, state behavior, preset recall, and effect-chain updates.
- [StructuralArchitectureViews.md](StructuralArchitectureViews.md): target-neutral Mermaid diagrams for static shared components, layers, module dependencies, product model structure, DSP engine structure, and platform port boundaries.
- [InterfaceContract.md](InterfaceContract.md): behavior-level interface contracts between shared components and platform ports.
- [ProjectDevelopmentGuide.md](ProjectDevelopmentGuide.md): step-by-step SonicFabric development procedure based on the architecture, embedded, documentation, testing, and DSP references.
- [ProjectChecklist.md](ProjectChecklist.md): living checklist for completed and next project steps.
- [Terminology.md](Terminology.md): canonical vocabulary for product, architecture, target, excluded-scope, and deferred feature terms.
- [README.md](README.md): short repository description.

## Agent Workflow

Before changing architecture, requirements, or diagrams:

- Read the relevant canonical document first.
- Check and update [ProjectChecklist.md](ProjectChecklist.md) on every prompt that completes, defers, or changes project work.
- Use [ProjectDevelopmentGuide.md](ProjectDevelopmentGuide.md) to choose the next architecture or development step.
- Use [Terminology.md](Terminology.md) before introducing new terms.
- Prefer fewer assumptions and ask clarifying questions when a choice affects architecture, requirements, scope, terminology, target behavior, or learning value.
- Explain architecture reasoning briefly when making changes so the user can learn the process, not only receive finished documents.
- Avoid duplicating canonical content across documents; summarize and link to the owning document instead.
- Add or update requirements only in [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md).
- Add or update user-facing workflows only in [UseCases.md](UseCases.md).
- Add or update behavior/runtime diagrams in [ArchitectureViews.md](ArchitectureViews.md).
- Add or update static structural diagrams in [StructuralArchitectureViews.md](StructuralArchitectureViews.md).
- Add or update shared interface contracts in [InterfaceContract.md](InterfaceContract.md).
- Add or update architecture scope, principles, drivers, and open topics only in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md).
- Keep [AGENTS.md](AGENTS.md) as a navigation and preservation guide; do not duplicate full canonical sections here.

## Diagram Guidance

- Use Mermaid diagrams in Markdown for architecture communication unless the user requests another format.
- Use Mermaid UML-style diagrams when communicating interface ownership, component relationships, data contracts, or use-case relationships.
- Add missing diagrams when they make the architecture easier to understand, especially use case diagrams, component diagrams, activity diagrams, state diagrams, sequence diagrams, and interface ownership diagrams.
- Put behavior/runtime diagrams in [ArchitectureViews.md](ArchitectureViews.md).
- Put static component, layer, dependency, and port boundary diagrams in [StructuralArchitectureViews.md](StructuralArchitectureViews.md).
- Put interface ownership and contract relationship diagrams in [InterfaceContract.md](InterfaceContract.md).
- Keep diagrams target-neutral unless the user asks for target-specific architecture or the relevant target requirements already exist.
- Do not add diagrams that duplicate an existing view; update or simplify the owning diagram instead.

## Architecture Boundaries To Preserve

- Keep common product behavior target-neutral.
- Treat effects as generic, interchangeable processing units until the user explicitly opens specific effect scope.
- Keep effect order represented as configuration/data, not fixed control flow.
- Isolate target-specific mechanisms through platform ports and target adapters.
- Keep desktop requirements minimal unless product requirements justify complexity.
- Expect bare metal and RTOS requirements to become more detailed as timing, memory, scheduling, hardware integration, diagnostics, watchdog behavior, reset behavior, or power behavior become architecturally significant.
- Do not fork common product behavior per target unless the difference is explicitly accepted and documented.
- Do not add cabinet modeling or amplifier modeling assumptions until the user reopens that topic.
- Treat tuner and metronome as open topics, not current scope, until the user promotes them into requirements.

## Requirement Labeling

Use stable labels in [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md):

- Product requirements: `SFAB-PR-*`
- Base hardware/runtime requirements: `SFAB-HWR-*`
- Software quality requirements: `SFAB-SQR-*`
- Use cases: `SFAB-UC-*` in [UseCases.md](UseCases.md)

When adding requirements:

- Do not reuse labels.
- Keep labels stable after creation.
- Add new requirements in the most specific existing section.
- Prefer target-neutral requirements in the base file.
- Put future target-specific requirements in target-specific files unless the requirement affects common behavior.

## Future Target Documents

Expected future requirement files:

- `TargetRequirements/DesktopRequirements.md`
- `TargetRequirements/BareMetalRequirements.md`
- `TargetRequirements/RTOSRequirements.md`

Expected future target architecture views:

- `TargetArchitectureViews/DesktopArchitectureView.md`
- `TargetArchitectureViews/BareMetalArchitectureView.md`
- `TargetArchitectureViews/RTOSArchitectureView.md`

Do not create these files until the user asks for target-specific detail or the common behavior is stable enough to justify them.

## Editing Guidance

- Keep architecture docs concise and at the correct abstraction level.
- Do not introduce implementation detail before the requirements or architecture views justify it.
- Keep Markdown ASCII-only unless existing content requires otherwise.
- Use relative Markdown links between project documents.
