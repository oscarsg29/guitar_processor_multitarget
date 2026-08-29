# Project Checklist

## Purpose

The project display name is **SonicFabric**.

This checklist tracks progress through the SonicFabric development procedure defined in [ProjectDevelopmentGuide.md](ProjectDevelopmentGuide.md). Keep this file updated as work is completed so the next step is visible without reconstructing context from prior prompts.

## Update Rule

- Mark a checkbox when the corresponding work is completed in the repository.
- Add a short note when a checkbox is partially complete or intentionally deferred.
- Keep this checklist aligned with [ProjectDevelopmentGuide.md](ProjectDevelopmentGuide.md).
- Do not use this file as the source of truth for requirements, architecture decisions, terminology, diagrams, or contracts; link to the canonical document instead.

## Current Next Step

- [ ] Start Step 9 by deciding whether to use `ArchitectureDecisionRecords.md` or an `ArchitectureDecisions/` directory.

## Cross-Cutting Documentation Rules

- [x] Define the rule that canonical content should be summarized and linked, not duplicated across documents.
- [x] Link [UseCases.md](UseCases.md) from [AGENTS.md](AGENTS.md).
- [x] Define Mermaid UML diagram guidance in [AGENTS.md](AGENTS.md).
- [x] Define the rule to add missing diagrams when they clarify use cases, components, activities, states, sequences, interfaces, or ownership.
- [x] Define the rule to reduce assumptions, ask clarifying questions more often, and explain architecture reasoning for learning.

## Use Cases

- [x] Create [UseCases.md](UseCases.md).
- [x] Define `SFAB-UC-*` use-case label convention.
- [x] Define primary actors without target-specific detail.
- [x] Add UML-style Mermaid use-case diagram.
- [x] Add UML-style Mermaid deferred use-case diagram.
- [x] Define primary target-neutral use cases.
- [x] Link use cases to related requirements instead of duplicating requirement text.
- [x] Keep tuner, metronome, cabinet modeling, amplifier modeling, and named effect types deferred.

## Step 1: Maintain Project Scope

- [x] Define the project display name as **SonicFabric**.
- [x] Define current product scope in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md).
- [x] Define current exclusions for cabinet modeling, amplifier modeling, and named effects.
- [x] Keep tuner and metronome as open/deferred topics rather than current requirements.
- [x] Define document ownership in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md).

## Step 2: Capture Or Refine Requirements

- [x] Create [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md).
- [x] Define stable `SFAB-*` requirement label convention.
- [x] Define base product requirements.
- [x] Define base hardware/runtime requirements.
- [x] Define software quality requirements.
- [x] Keep target-specific software requirements deferred.

## Step 3: Identify Architectural Drivers

- [x] Capture initial architectural drivers in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md).
- [x] Keep low-latency real-time audio visible as a driver.
- [x] Keep reorderable generic effect chains visible as a driver.
- [x] Keep shared product behavior across bare metal, RTOS, and desktop visible as a driver.
- [x] Keep platform isolation visible as a driver.
- [x] Keep deterministic testing visible as a driver.

## Step 4: Define Shared Terminology

- [x] Create [Terminology.md](Terminology.md).
- [x] Define product terms.
- [x] Define architecture terms.
- [x] Define target terms.
- [x] Define excluded-scope terms.
- [x] Define deferred feature terms for tuner and metronome.

## Step 5: Document Shared Behavior Views

- [x] Create [ArchitectureViews.md](ArchitectureViews.md).
- [x] Add Product Concept View.
- [x] Add Runtime Audio and Control Flow.
- [x] Add Processor State Model.
- [x] Add Preset Recall Flow.
- [x] Add Effect Chain Update Flow.
- [x] Keep behavior/runtime diagrams target-neutral.

## Step 6: Document Shared Structural Views

- [x] Create [StructuralArchitectureViews.md](StructuralArchitectureViews.md).
- [x] Add Shared Component View.
- [x] Add Layer and Dependency View.
- [x] Add Shared Module Dependency View.
- [x] Add Product Model Structure.
- [x] Add DSP Engine Structure.
- [x] Add simplified Platform Port Boundary View.
- [x] Add Deferred Target Deployment Views placeholder.
- [x] Avoid C4 terminology for now.
- [x] Defer deployment diagrams until target-specific requirements exist.

## Step 7: Define Interface Contracts

- [x] Create [InterfaceContract.md](InterfaceContract.md).
- [x] Define contract ownership rules.
- [x] Add UML-style Mermaid Interface Ownership Overview.
- [x] Add UML-style Mermaid Product Contract Relationships.
- [x] Add UML-style Mermaid DSP Contract Relationships.
- [x] Add UML-style Mermaid Platform Port Contract Relationships.
- [x] Define Product Model contracts.
- [x] Define DSP Engine contracts.
- [x] Define Platform Port contracts.
- [x] Define Runtime Health and Processor State data contracts.
- [x] Defer target-specific interfaces.

## Step 8: Define Quality Attribute Scenarios

- [x] Create [QualityAttributeScenarios.md](QualityAttributeScenarios.md).
- [x] Define a quality attribute scenario template.
- [x] Add latency scenario.
- [x] Add preset recall scenario.
- [x] Add invalid configuration scenario.
- [x] Add effect-chain update scenario.
- [x] Add portability scenario.
- [x] Add testability scenario.
- [x] Add diagnostics scenario.
- [x] Add recovery/fallback scenario.
- [x] Prioritize scenarios that affect architecture decisions.

## Step 9: Select Architectural Tactics And Decisions

- [ ] Decide whether to use `ArchitectureDecisionRecords.md` or an `ArchitectureDecisions/` directory.
- [ ] Define an architecture decision record format.
- [ ] Record decisions already made.
- [ ] Capture tactics for performance.
- [ ] Capture tactics for modifiability.
- [ ] Capture tactics for portability.
- [ ] Capture tactics for reliability.
- [ ] Capture tactics for testability.

## Step 10: Plan Shared Core Implementation

- [ ] Define the smallest shared core implementation slice.
- [ ] Define initial shared-core module layout.
- [ ] Define initial build strategy.
- [ ] Define how shared core can be tested without hardware.
- [ ] Keep target adapters thin until target requirements exist.

## Step 11: Establish Test Strategy

- [ ] Create `TestStrategy.md`.
- [ ] Define product model unit test scope.
- [ ] Define configuration validation test scope.
- [ ] Define preset test scope.
- [ ] Define parameter test scope.
- [ ] Define effect-chain behavior test scope.
- [ ] Define deterministic DSP test input strategy.
- [ ] Define interface contract integration test strategy.
- [ ] Defer hardware-in-loop and target integration tests until target requirements exist.

## Step 12: Refine Target-Specific Requirements

- [ ] Create `TargetRequirements/DesktopRequirements.md` when desktop-specific requirements are needed.
- [ ] Create `TargetRequirements/BareMetalRequirements.md` when bare-metal-specific requirements are needed.
- [ ] Create `TargetRequirements/RTOSRequirements.md` when RTOS-specific requirements are needed.
- [ ] Keep desktop requirements minimal unless product behavior requires more.
- [ ] Capture bare metal and RTOS constraints only when they become architecturally significant.
- [ ] Preserve common product behavior across targets.

## Step 13: Add Target Architecture Views

- [ ] Create `TargetArchitectureViews/DesktopArchitectureView.md` when desktop architecture is ready.
- [ ] Create `TargetArchitectureViews/BareMetalArchitectureView.md` when bare metal architecture is ready.
- [ ] Create `TargetArchitectureViews/RTOSArchitectureView.md` when RTOS architecture is ready.
- [ ] Add deployment/allocation views only after target requirements exist.
- [ ] Map shared core, platform ports, target adapters, target runtime, and hardware mechanisms.

## Step 14: Iterate And Evaluate

- [ ] Revisit requirements when new features or constraints are introduced.
- [ ] Revisit views when interfaces or ownership boundaries change.
- [ ] Revisit contracts before implementation details leak across boundaries.
- [ ] Use quality attribute scenarios to evaluate whether the design still supports project goals.

## Repository Hygiene

- [x] Add PDF ignore rule to `.gitignore`.
- [ ] Decide whether `CodingStyles.md` should be tracked.
- [x] Commit and push current documentation changes.
