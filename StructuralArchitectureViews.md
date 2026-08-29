# Structural Architecture Views

## Purpose

The project display name is **SonicFabric**.

This document captures target-neutral static structure for SonicFabric. These views describe the shared components, ownership boundaries, module dependencies, and platform port boundaries that should remain common before target-specific deployment decisions are made.

These views support the architecture principles in [ArchitectureAndDesignPrinciples.md](ArchitectureAndDesignPrinciples.md), the behavior/runtime views in [ArchitectureViews.md](ArchitectureViews.md), the requirements in [ProductAndSoftwareRequirements.md](ProductAndSoftwareRequirements.md), and the shared vocabulary in [Terminology.md](Terminology.md).

## Shared Component View

This view shows the main shared components and their ownership relationships. It is conceptual and does not define concrete classes, files, threads, tasks, interrupts, or processes.

```mermaid
flowchart TD
    SonicFabric[SonicFabric Shared Core]

    SonicFabric --> ProductModel[Product Model]
    SonicFabric --> DSPEngine[DSP Engine]
    SonicFabric --> PlatformPorts[Platform Ports]

    ProductModel --> PresetManager[Preset Manager]
    ProductModel --> ChainManager[Chain Manager]
    ProductModel --> ParameterModel[Parameter Model]
    ProductModel --> ConfigValidator[Configuration Validator]
    ProductModel --> DiagnosticsFacade[Diagnostics Facade]

    ChainManager --> EffectRegistry[Effect Registry]
    ChainManager --> EffectSlots[Effect Slots]
    EffectSlots --> GenericEffects[Generic Effects]

    DSPEngine --> AudioBufferModel[Audio Buffer Model]
    DSPEngine --> ChainExecutor[Effect Chain Executor]
    DSPEngine --> RuntimeHealth[Runtime Health]

    PlatformPorts --> AudioIOPort[Audio I/O Port]
    PlatformPorts --> ControlPort[Control Port]
    PlatformPorts --> PersistencePort[Persistence Port]
    PlatformPorts --> TimePort[Time or Sample-Clock Port]
    PlatformPorts --> DiagnosticsPort[Diagnostics Port]
    PlatformPorts --> ResourcePort[Resource Capability Port]
```

## Shared Module Dependency View

This view shows intended dependency direction between shared modules. Dependencies should point toward stable contracts and avoid direct target mechanisms.

```mermaid
flowchart TD
    ProductModel[Product Model]
    PresetManager[Preset Manager]
    ChainManager[Chain Manager]
    ParameterModel[Parameter Model]
    ConfigValidator[Configuration Validator]
    EffectRegistry[Effect Registry]
    DSPEngine[DSP Engine]
    DiagnosticsFacade[Diagnostics Facade]
    PlatformPorts[Platform Ports]

    PresetManager --> ProductModel
    PresetManager --> ConfigValidator
    PresetManager --> PersistencePort[Persistence Port]

    ChainManager --> ProductModel
    ChainManager --> ConfigValidator
    ChainManager --> EffectRegistry
    ChainManager --> DSPEngine

    ConfigValidator --> ParameterModel
    ConfigValidator --> EffectRegistry

    DSPEngine --> EffectRegistry
    DSPEngine --> DiagnosticsFacade

    DiagnosticsFacade --> DiagnosticsPort[Diagnostics Port]
    PersistencePort --> PlatformPorts
    DiagnosticsPort --> PlatformPorts
```

## Product Model Structure

This view shows the static product concepts owned by the product model.

```mermaid
flowchart TD
    ProductModel[Product Model]

    ProductModel --> Presets[Presets]
    ProductModel --> ChainConfig[Chain Configuration]
    ProductModel --> Parameters[Parameters]
    ProductModel --> RuntimeState[Runtime State]

    Presets --> ActivePreset[Active Preset]
    Presets --> StoredPresetReference[Stored Preset Reference]

    ChainConfig --> OrderedSlots[Ordered Effect Slots]
    OrderedSlots --> EffectSlot[Effect Slot]
    EffectSlot --> GenericEffectRef[Generic Effect Reference]
    EffectSlot --> EnabledState[Enabled State]
    EffectSlot --> BypassState[Bypass State]

    Parameters --> ParameterValues[Parameter Values]
    Parameters --> ParameterMetadata[Parameter Metadata]
    Parameters --> ValidationRules[Validation Rules]

    RuntimeState --> ProcessorState[Processor State]
    RuntimeState --> RuntimeHealth[Runtime Health]
```

## DSP Engine Structure

This view shows the static responsibilities inside the DSP engine. It does not prescribe algorithms, buffer sizes, memory allocation strategy, or target scheduling.

```mermaid
flowchart TD
    DSPEngine[DSP Engine]

    DSPEngine --> AudioBufferModel[Audio Buffer Model]
    DSPEngine --> ChainExecutor[Effect Chain Executor]
    DSPEngine --> EffectLifecycle[Effect Lifecycle]
    DSPEngine --> ParameterApplication[Parameter Application]
    DSPEngine --> BypassHandling[Bypass Handling]
    DSPEngine --> ProcessingHealth[Processing Health]

    ChainExecutor --> EffectSlots[Effect Slots]
    EffectSlots --> GenericEffects[Generic Effects]

    ParameterApplication --> ParameterValues[Parameter Values]
    BypassHandling --> BypassState[Bypass State]
    ProcessingHealth --> RuntimeHealth[Runtime Health]
```

## Platform Port Boundary View

This view shows the shared core depending on target-neutral port contracts, with target adapters outside the shared core. Target mechanisms remain outside this static structure until target-specific architecture views are created.

```mermaid
flowchart LR
    subgraph SharedCore[SonicFabric Shared Core]
        ProductModel[Product Model]
        DSPEngine[DSP Engine]
        DiagnosticsFacade[Diagnostics Facade]
    end

    subgraph PortContracts[Platform Port Contracts]
        AudioIO[Audio I/O]
        Control[Control]
        Persistence[Persistence]
        Time[Time / Sample Clock]
        Diagnostics[Diagnostics]
        Resources[Resource Capabilities]
    end

    subgraph TargetAdapters[Target Adapters]
        DesktopAdapter[Desktop Adapter]
        BareMetalAdapter[Bare Metal Adapter]
        RTOSAdapter[RTOS Adapter]
    end

    SharedCore --> PortContracts
    TargetAdapters --> PortContracts
```

## Deferred Target Deployment Views

Deployment and allocation diagrams are deferred until target-specific requirements exist. Future deployment views should describe how the shared core, platform ports, and target adapters map to concrete desktop, bare metal, or RTOS execution environments without redefining common product behavior.
