# Package Contents

The package ships more than the sample scene. Besides examples, it includes the runtime and editor subsystems that Astra RPG Framework uses internally and exposes for your own project workflows.

For a guided tour of the sample content, see [Samples](samples.md).

## Main package subsystems

| Subsystem | What it contains | Learn more |
| --- | --- | --- |
| Entities, stats, attributes, classes, scaling | The core progression model, including `EntityCore`, value containers, class data, growth formulas, and scaling assets. | [Workflows](workflows.md) |
| Game Events | Predefined event types, listeners, and the generator-based workflow for creating additional typed events. | [Workflows](workflows.md#game-events) |
| Global framework configuration | The shared config used to dispatch built-in framework events such as spawned, level up/down, stat changed, and attribute changed. | [Workflows](workflows.md#global-framework-events-and-reactive-filtering) |
| Game Actions | Reusable asynchronous actions, projections, wrappers, and owner-aware event-driven execution flows. | [Workflows](workflows.md#game-actions), [Advanced topics](advanced-topics.md#owner-aware-gameaction-execution) |
| Game Tags | Tag assets, tag sets, and inspector header pills used to organize Astra assets and drive tag-based logic. | [Game Tags](game-tags.md) |
| Conditions and reactive filtering | Polymorphic condition trees, conditional actions, and the trigger/filtering model used by reactive systems. | [Conditions](conditions.md) |

## Editor tooling included in the package

The package also contains editor-side tooling under `Editor/` that powers several authoring workflows documented elsewhere in this site:

- the Project Settings page for the global Astra RPG Framework configuration
- inspector support for `SerializeReference`-based authoring flows
- the injected header pills used by taggable assets and components
- helpers used by condition and game-action inspectors

These editor tools are part of the package experience and are one of the reasons the framework can expose rich inspector-driven workflows without requiring custom tooling in your own project.
