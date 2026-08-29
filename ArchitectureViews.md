# Architecture Views

## Purpose

The project display name is **SonicFabric**.

This document captures target-neutral product behavior and runtime architecture views for SonicFabric. The diagrams describe expected core behavior shared by bare metal, RTOS, and desktop versions. Target-specific details should be added later only after the shared product behavior is stable.

These views support the architecture principles in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md), the requirements in [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md), the structural views in [StructuralArchitectureViews.md](StructuralArchitectureViews.md), and the shared vocabulary in [Terminology.md](Terminology.md).

## Product Concept View

This view shows the common product concepts that should remain consistent across targets.

```mermaid
flowchart TD
    Product[SonicFabric]
    Product --> Presets[Presets]
    Product --> Chain[Effect Chain]
    Product --> Parameters[Parameters]
    Product --> RuntimeState[Runtime State]
    Product --> Diagnostics[Diagnostics and Health]

    Presets --> ActivePreset[Active Preset]
    Presets --> StoredPresets[Stored Presets]

    Chain --> EffectSlot[Effect Slot]
    EffectSlot --> GenericEffect[Generic Effect]
    EffectSlot --> BypassState[Bypass State]
    EffectSlot --> EnabledState[Enabled State]

    Parameters --> ParameterValue[Parameter Value]
    Parameters --> ParameterMetadata[Parameter Metadata]
    Parameters --> Validation[Validation Rules]

    RuntimeState --> RunningState[Processor State]
    RuntimeState --> ChainState[Chain Configuration]
```

## Layer and Dependency View

This view describes the intended dependency direction. Shared product and DSP behavior should depend on stable platform ports, not on target mechanisms.

```mermaid
flowchart TD
    ProductModel[Product Model<br/>presets, chains, parameters, common behavior]
    DSPEngine[DSP Engine<br/>audio buffers, effect chain, effect lifecycle]
    PlatformPorts[Platform Ports<br/>audio, control, persistence, time, diagnostics]
    TargetAdapters[Target Adapters<br/>bare metal, RTOS, desktop]
    TargetMechanisms[Target Mechanisms<br/>drivers, OS APIs, filesystems, UI frameworks]

    ProductModel --> DSPEngine
    ProductModel --> PlatformPorts
    DSPEngine --> PlatformPorts
    TargetAdapters --> PlatformPorts
    TargetAdapters --> TargetMechanisms
```

## Runtime Audio and Control Flow

This view separates the real-time audio flow from the control/configuration flow. Control changes affect product state and chain configuration, but the audio path remains explicit and bounded.

```mermaid
flowchart LR
    AudioIn[Audio Input] --> AudioPort[Audio I/O Port]
    AudioPort --> DSPEngine[DSP Engine]
    DSPEngine --> Chain[Effect Chain]
    Chain --> AudioOut[Audio Output]

    ControlInput[Control Input] --> ControlPort[Control Port]
    ControlPort --> ProductModel[Product Model]
    PresetStore[Preset Store] --> PersistencePort[Persistence Port]
    PersistencePort --> ProductModel

    ProductModel --> ChainConfig[Validated Chain Configuration]
    ChainConfig --> DSPEngine

    DSPEngine --> Health[Runtime Health]
    ProductModel --> Health
    Health --> DiagnosticsPort[Diagnostics Port]
```

## Processor State Model

This view describes the common processor lifecycle before any target-specific startup, scheduling, or shutdown mechanism is chosen.

```mermaid
stateDiagram-v2
    [*] --> Uninitialized
    Uninitialized --> Configured: initialize core state
    Configured --> Running: start processing
    Running --> Stopped: stop processing
    Stopped --> Running: restart processing
    Stopped --> Configured: reconfigure

    Running --> FaultOrFallback: unrecoverable invalid state or processing fault
    Configured --> FaultOrFallback: invalid configuration
    FaultOrFallback --> Configured: recover with valid configuration
    FaultOrFallback --> Stopped: stop after fault
```

## Preset Recall Flow

This view defines the common behavior expected when a preset is recalled while the product is active.

```mermaid
sequenceDiagram
    participant UserControl as Control Input
    participant Product as Product Model
    participant Store as Preset Store
    participant Validator as Configuration Validator
    participant Engine as DSP Engine
    participant Diag as Diagnostics

    UserControl->>Product: request preset recall
    Product->>Store: load preset data
    Store-->>Product: preset data
    Product->>Validator: validate preset and chain state

    alt preset is valid
        Validator-->>Product: accepted configuration
        Product->>Engine: apply validated chain configuration
        Engine-->>Product: configuration active
        Product->>Diag: report active preset
    else preset is invalid
        Validator-->>Product: rejected configuration
        Product->>Engine: keep current valid configuration
        Product->>Diag: report recall failure
    end
```

## Effect Chain Update Flow

This view defines the expected behavior for effect enable, bypass, parameter, and reorder changes.

```mermaid
sequenceDiagram
    participant UserControl as Control Input
    participant Product as Product Model
    participant Validator as Configuration Validator
    participant Engine as DSP Engine
    participant Diag as Diagnostics

    UserControl->>Product: request chain update
    Product->>Validator: validate requested update

    alt update is valid
        Validator-->>Product: accepted update
        Product->>Engine: apply validated update
        Engine-->>Product: update active
        Product->>Diag: report chain state
    else update is invalid
        Validator-->>Product: rejected update
        Product->>Engine: keep current valid chain
        Product->>Diag: report rejected update
    end
```
