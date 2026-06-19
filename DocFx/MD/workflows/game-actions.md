# Game Actions

*🏷️ Version 1.4.0+*

*Relative path:* `Game Actions`

A `GameAction` is a `ScriptableObject` that encapsulates reusable logic which can be executed by different components or systems in the game. Game actions make it easy to assign behavior to `GameObject`s through the Inspector, promoting code reuse and separation of concerns.

`GameAction`s accept a generic context parameter via `GameAction<TContext>`. The recommended context type for built-in actions is `IHasEntity`: because most `GameEventListener` payloads in the framework implement this interface, `IHasEntity`-based actions connect directly to any `GameEventListener` response without needing to extract a `Component` reference first. `Component`-based variants remain available for cases where a raw `Component` context is passed, but the `IHasEntity` variants should be the first choice when wiring actions to events.

At authoring time, polymorphic action references are usually exposed through the non-generic `GameActionBase`, which exists so Inspector fields can accept actions with different concrete context types. At runtime, the action still executes through its concrete `GameAction<TContext>` implementation, so context compatibility continues to matter.

## Built-in action variants

The `IHasEntity`-based variants are listed under `Astra Framework/Game Actions/Context: Entity/` in the asset creation menu.

The framework includes several built-in `GameAction` implementations for `IHasEntity` contexts:

- **Toggle Entity Active GO** (`Context: Entity/Toggle Entity Active GO`): Sets the active state of the entity's `GameObject` to a configured value
- **Toggle Entity Renderers** (`Context: Entity/Toggle Entity Renderers`): Enables or disables all renderers on the entity's `GameObject` and its children
- **Toggle Entity Colliders** (`Context: Entity/Toggle Entity Colliders`): Enables or disables all colliders on the entity's `GameObject` and its children
- **Destroy Entity Game Object** (`Context: Entity/Destroy Entity Game Object`): Destroys the `GameObject` of the entity exposed by the context
- **Composite** (`Context: Entity/Composite`): Executes a sequence of `GameAction<IHasEntity>` actions in order, passing the same context to each
- **Delayed** (`Context: Entity/Delayed`): Executes a `GameAction<IHasEntity>` after a specified delay (in seconds)
- **Conditional** (`Context: Entity/Conditional`): Executes a `GameAction<IHasEntity>` only when a configured condition evaluates to true against the entity
- **Do Nothing** (`Context: Entity/Do Nothing`): Does nothing. Useful as a placeholder, for testing workflows, or to satisfy required fields that must reference a `GameAction` (for example, default on-death and on-resurrection actions in Astra Health)
- **Increase Counter** (`Context: Entity/Increase Counter`): Increases the value of a `LongRef` by a specified amount. Negative values decrease the counter. Useful for tracking statistics like enemies defeated, entities spawned, or interactions performed
- **→ Component Projection** (`Context: Entity/Projections/→ Component Projection`): Bridges an `IHasEntity` context to an inner `GameAction<Component>`, by passing `context.Entity` as the component. Use this to reuse existing `Component`-based actions on entity event listeners. Note that rich payload data (damage amounts, level deltas, etc.) is discarded. Use this only for structural actions that only need to know which entity was involved

Equivalent `Component`-based variants exist for all of the above (under `Context: Component/`). Use them when a raw `Component` is the only context available and no `IHasEntity` payload is in scope.

`GameAction` assets are also taggable, so larger projects can organize action libraries with the same pill-based workflow used by other Astra assets. This becomes especially useful once you start pairing actions with tag-based filtering rules. For the complete authoring flow, see [Game Tags](game-tags.md) and [Conditions](conditions.md).

`GameAction`s may target infrastructure and flow control or actual gameplay mechanics. Most of the built-in examples above are infrastructure-oriented. Examples of gameplay-oriented `GameAction`s include:

- **Grant Experience Game Action**: Grants a specified amount of experience to an entity implementing `IEntityLevel`
- **Drop Loot Game Action**: Spawns collectible loot
- **Teleport To Base Game Action**: Teleports a character back to a base location

## Executing `GameAction`s from code

To invoke a `GameAction`, call `ExecuteAsync` and pass the required context parameter. For example, if `delayedAction` is a `DelayedEntityContextGameAction` and `entityContext` is an `IHasEntity` payload:

```csharp
// entityContext is an IHasEntity payload (for example from a GameEventListener response)
Awaitable delayedActionAwaitable = delayedAction.ExecuteAsync(entityContext, destroyCancellationToken);
DoWorkHere();
await delayedActionAwaitable;
Debug.Log("Delayed action completed.");
```

The `destroyCancellationToken` parameter is optional. Use it when the action should be cancelled if the `GameObject` owning the invoker component is destroyed before execution completes. If you do not provide it, the action runs to completion regardless of the invoker's lifecycle.

If a `GameAction` should complete independently of the invoker component's lifecycle, use [RunFireAndForget](xref:ElectricDrill.AstraRpgFramework.GameActions.GameAction`1.RunFireAndForget(`0,UnityEngine.MonoBehaviour)). This method uses the `GameActionRunner` `MonoBehaviour` (adding it to the specified `GameObject` if missing) to execute the action. For example:

```csharp
// Assume 'context' is the required context parameter for onDeathGameAction
// 'persistentGameObject' owns the GameActionRunner and executes the action
onDeathGameAction.RunFireAndForget(context, persistentGameObject);
```

With this approach, even if the invoker component is destroyed (for example because the entity died), the action still runs to completion on the `persistentGameObject`. This is useful for actions such as particle effects or sounds that should persist after the original entity is gone.

## Owner-aware execution from `GameEventListener`s

*🏷️ Version 2.0.0+*

When a `GameAction` is invoked from a `GameEventListener`, prefer the listener's owner-aware action list over binding `ExecuteAsync` directly through the UnityEvent when the logic depends on `Holder`-aware conditions or owner propagation. The owner-aware path dispatches the selected `GameActionBase` assets with the listener as runtime owner, and wrapper actions such as composite, delayed, conditional, and projection actions preserve that owner while forwarding execution.

For the condition-side resolution model, see [Conditions](conditions.md). For the deeper runtime details behind this execution path, see [Owner-aware `GameAction` execution](advanced-topics.md#owner-aware-gameaction-execution).

## API references and custom actions

For more details, refer to the API documentation for game actions:

- [GameAction base constructs](xref:ElectricDrill.AstraRpgFramework.GameActions)
- [Concrete implementations of GameAction with open generic types](xref:ElectricDrill.AstraRpgFramework.GameActions.Actions)
- [Concrete implementations of GameAction with `IHasEntity` context (primary instantiable `ScriptableObject`s)](xref:ElectricDrill.AstraRpgFramework.GameActions.Actions.WithIHasEntity)
- [Concrete implementations of GameAction with `Component` context (legacy and specialized use)](xref:ElectricDrill.AstraRpgFramework.GameActions.Actions.Component)

If you want to create custom `GameAction`s, review the implementation of the built-in actions to understand the intended patterns.

> [!NOTE]
> The built-in library currently focuses on a compact set of generic actions. Project-specific gameplay behaviors are expected to be implemented as custom `GameAction`s when needed.

## `GameAction`s under the hood

`GameAction`s are designed to support asynchronous workflows such as waiting for a delay or coordinating animation-driven logic. Synchronous actions are still valid, but the base execution model is asynchronous.

`GameAction`s are implemented on top of Unity's [Awaitable](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/Awaitable.html). Executing a `GameAction` returns an `Awaitable` that can be awaited to determine when the action completes, without blocking the main game thread.

Awaitables are a modern alternative to coroutines. They use an internal pooling system to reduce allocation overhead, which is useful in workflows with many asynchronous actions.

> [!WARNING]
> Do not await the same `Awaitable` instance more than once. Because Awaitables are pooled, awaiting the same instance multiple times can cause undefined behavior, exceptions, or deadlocks. Always treat the value returned by `ExecuteAsync` as single-use.

For more information on Awaitables, see Unity's documentation: [Awaitable Documentation](https://docs.unity3d.com/6000.3/Documentation/Manual/async-await-support.html).

## `GameAction`s with UnityEvents and `GameEventListener`s

UnityEvents can invoke methods that start `GameAction`s, so a `GameAction` can be triggered from a UnityEvent response. Because `GameEventListener`s use UnityEvents for their responses, you can wire a `GameAction` to a `GameEventListener` provided the action's context type matches the event payload.

`GameEventListener`s also expose an owner-aware action list that accepts `GameActionBase` references. This is usually the better Inspector workflow for event-driven actions because it preserves the listener component as runtime owner, which allows nested actions and `Holder`-based conditions to resolve consistently.

> [!NOTE]
> UnityEvents do not await asynchronous return values. Invoking a `GameAction` from a UnityEvent therefore behaves as fire-and-forget from the event's point of view.
>
> If you need follow-up logic after the action finishes, prefer one of these patterns:
> - Use a composite `GameAction` that first executes the desired action and then runs the follow-up action as the next step
> - Use a custom `MonoBehaviour` that executes the `GameAction`, awaits its completion, and performs the continuation in code
