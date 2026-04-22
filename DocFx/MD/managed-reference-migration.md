# Managed Reference Migration (v2.0.0)

> [!NOTE]
> This page is relevant only if your project uses `[SerializeReference]` fields holding instances of renamed framework types, or if you wrote a custom `IDisplaySONameProvider<T>` implementation. If neither applies, no action is required beyond what the main [Migration Guide](migration-guide.md#migrating-to-v200) describes.

v2.0.0 renames a set of ScriptableObject types and replaces the generic `IDisplaySONameProvider<T>` with a non-generic equivalent. Unity's `[MovedFrom]` attribute patches most C# script references automatically, but serialized data stored through `[SerializeReference]` uses the full assembly-qualified type string — and `[MovedFrom]` cannot rewrite generic arguments inside that string (Unity bug [UUM-44729](https://issuetracker.unity3d.com/issues/movedfrom-attribute-is-not-working-for-generic-types)). This guide covers the two cases that need extra work:

1. [Migrating `IDisplaySONameProvider<T>` custom implementations](#1-migrating-idisplaysonamet-custom-implementations)
2. [Running the migration tool](#2-running-the-migration-tool)

## 1. Migrating `IDisplaySONameProvider<T>` custom implementations

If you implemented `IDisplaySONameProvider<T>` yourself, change the class signature and cast the argument internally:

```csharp
// Before (v1.x)
public class MyStatDisplay : IDisplaySONameProvider<StatSO>
{
    public string GetDisplayName(StatSO stat) => stat.name.ToUpperInvariant();
}

// After (v2.0.0)
public class MyStatDisplay : IDisplaySONameProvider
{
    public string GetDisplayName(ScriptableObject asset)
    {
        return asset is StatSO stat ? stat.name.ToUpperInvariant() : "None";
    }
}
```

Update any `[SerializeReference]` field declarations accordingly:

```csharp
// Before
[SerializeReference, TypeSelectable(typeof(DefaultDisplaySONameProvider<StatSO>))]
private IDisplaySONameProvider<StatSO> _provider;

// After
[SerializeReference, TypeSelectable(typeof(DefaultDisplaySONameProvider))]
private IDisplaySONameProvider _provider;
```

`DefaultDisplaySONameProvider` carries `[MovedFrom]` targeting `DefaultDisplaySONameProvider\`1`, so the migration tool handles the serialized managed reference for the built-in default. You only need to update the field declarations in your own code.

## 2. Running the migration tool

The migration tool at `Tools → Astra RPG Framework → Migrate Managed References (SO Rename)` rewrites managed references in `.asset`, `.prefab`, and `.unity` files that `[MovedFrom]` alone cannot patch.

> [!IMPORTANT]
> Do not enter Play Mode and do not commit your project until you have run the migration tool and verified the result. Managed references that still point at old type strings appear as missing and may cause `NullReferenceException`s at runtime.

**Steps:**

1. Close Play Mode, save all open scenes, and commit (or stash) your current work as a safety net.
2. Open your project in Unity.
3. Open every scene you want to migrate, **or** enable **Scan closed scenes** inside the tool (step 5) to let it open scenes automatically.
4. Go to `Tools → Astra RPG Framework -> v2.0.0 Migration -> Migrate Managed References (SO Rename)`.
5. With **Dry-run** toggled **ON**, press **Scan**. Enable **Scan closed scenes** if needed. Review the `[plan]` lines — each one lists an asset and the managed reference that will be rewritten.
6. If the plan looks correct, turn **Dry-run** OFF and press **Apply**. The tool asks for confirmation that a backup is in place before it proceeds.
7. The tool writes a full log to `Library/AstraMigrationReport_<timestamp>.txt`.
8. **Run Scan again** with Dry-run ON: the result must show `0 reference(s)`. The tool is idempotent.
9. Enter Play Mode and verify that UI elements displaying stat, attribute, or class names still work correctly.
10. Commit the migrated assets.

### What the tool rewrites

The tool applies two rules:

- **Rule 1 — drop generics on `DefaultDisplaySONameProvider`**: rewrites the former closed-generic `DefaultDisplaySONameProvider\`1[[...]]` to the new non-generic `DefaultDisplaySONameProvider`. This is the primary fix for the UUM-44729 failure mode.
- **Rule 2 — SO-suffix rename inside generic arguments**: for any custom closed generic whose type string includes one of the renamed framework types as a generic argument (for example `SomeUserProvider\`1[[Stat, …]]`), the old name is rewritten to its `SO`-suffixed equivalent.

> [!NOTE]
> Non-generic `[SerializeReference]` fields whose concrete type is one of the renamed ScriptableObjects (for example, a field that directly serialized a `Stat` instance) are already handled by `[MovedFrom]` and do not need the tool.

### Custom closed generics not covered by the rules

If the tool logs a `[skip]` line for a custom closed-generic type, the type was not covered by the built-in rules. Refactor it to a non-generic form following the recipe in [section 1](#1-migrating-idisplaysonamet-custom-implementations), then re-run the tool.

## Guardrail warning

Starting with v2.0.0, the Editor emits a one-shot warning at startup when it detects a `[SerializeReference, TypeSelectable]` field typed as a closed generic:

> `[Astra] {OwnerType}.{field} uses [SerializeReference, TypeSelectable] on a closed generic ({targetType}). Unity [MovedFrom] does NOT propagate into generic arguments (UUM-44729): rename of any type inside the generic args will silently drop the serialized value.`

Treat this as a blocker — refactor to a non-generic interface or abstract base class before renaming any type involved.

## Support

If the migration tool reports failures or unexplained `[skip]` lines, send the report file at `Library/AstraMigrationReport_<timestamp>.txt` along with a brief description of your setup to electricdrill.info@gmail.com.
