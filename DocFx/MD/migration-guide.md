# Migration Guide

## Migrating to v1.4.0

v1.4.0 introduces the new GameAction system and moves Game Events to a single-context parameter (Parameter Object pattern). This change improves extensibility and simplifies integration with GameAction because both systems now use a single context and are compatible with UnityEvent.

### Key benefits
- Easier, non-breaking extension of event data via a single context object and polymorphism.
- Seamless interoperability with GameAction and UnityEvent-based listeners.

### Breaking change
With the deprecation of the legacy `EntityLeveledUpGameEvent`, `EntityLeveledDownGameEvent` and their dedicated listeners, `EntityStats` and `EntityAttributes` now subscribe the new EntityCore's single-parameter events internally. All your `EntityCore` instances must be updated to use the new `EntityLevelUpGameEvent` and `EntityLevelDownGameEvent` instances. (Notice that the name of the deprecated events contained "Level**ed**" while the new events contain "Level".)

### Migration steps - breaking changes
1. **Back up your project before making changes.**
2. Either create the new `EntityLevelUpGameEvent` and `EntityLevelDownGameEvent` instances from the context menu (`Create > Astra RPG Framework > Game Events > Generated > Entity Level Up` and `Entity Level Down`), or import the new event instances from the updated v1.4.0 `Utils` samples (`Utils > EventInstances > 1param > Entity Level Up Game Event` and `Entity Level Down Game Event`). If you re-import samples, prefer removing duplicate assets from the newly imported samples. Keep only the new events instances.
3. For each entity (prefab or scene object) open the `EntityCore` inspector and assign to the new `On Level Up` and `On Level Down` event fields to the new single-parameter event instances just created. The fields for the new events are marked as required (red asterisk), while the deprecated events fields are optional and marked with `(Legacy/Deprecated)`. The inspector, before assigning the new events, should look like this:  
   ![EntityCore inspector with new event fields](../images/migration-guide/entitycore-new-events.png)


### Migration steps - changing deprecated events
Manual and obsolete `EntityLeveledUpGameEvent`, `EntityLeveledDownGameEvent` (and the respective listeners) you used across your project are still supported, **but will not be any more in future versions of the framework**; At the moment, `EntityLevel` will raise both the deprecated event as well as the new event when the level changes. However, migrate your project to the new events when it is convenient for you to use the new single-parameter events and listeners and align with the new standard.

To update them, I suggest to search your project and scenes for usages of the deprecated `EntityLeveledUpGameEvent` and `EntityLeveledDownGameEvent`. To do this, I would suggest you to use the Unity Editor search functionality:
1. Right-click on the scriptable object instance of the deprecated event you want to search for (e.g., `Entity Leveled Up Game Event`).
2. Select `Find References In Project`. A window like this will open:  
   ![Find References In Project window](../images/migration-guide/find-references-project.png)
3. Inspect the found references to identify where you used the deprecated event and update them to use the new `EntityLevelUpGameEvent` or `EntityLevelDownGameEvent` and/or the respective listeners instead. You'll catch also the GameEvent listeners as they reference the event instance.
4. Right-click again on the scriptable object instance of the deprecated event and select `Find References In Scene` to find any usage in the currently opened scene. You will see the hierarchy window changed, but no meaningful references will be shown. However, you can click on the top-right button to open a window similar to the previous one, showing all references in the scene:  
   ![Find References In Scene window](../images/migration-guide/find-references-scene.png)  
   ![References in Scene](../images/migration-guide/references-in-scene.png)
5. Inspect the found references in the scene and update them to use the new `EntityLevelUpGameEvent` or `EntityLevelDownGameEvent` and/or the respective listeners instead.
6. Repeat steps 4-5 for each scene in your project.
7. Repeat steps 1-6 for the other deprecated event.

## Migrating to v1.1.0

The rebranding of SOAP RPG Framework to Astra RPG Framework involves several changes that need to be addressed when updating existing projects. This guide outlines the necessary steps to ensure a smooth transition.

### 1. Backup Your Project
Before making any changes, it's crucial to back up your project. Whether you are using version control or simply creating a copy of your project folder, having a backup will allow you to revert to the previous state if anything goes wrong during the migration process.

### 2. Update the Package from the Package Manager
Open Unity and navigate to the Package Manager (`Window -> Package Manager`). In the "In This Project" section, locate the SOAP RPG Framework package. Update it to version 1.1.0, which is now listed as Astra RPG Framework.

### 3. Update Namespaces in Your Code
If your project contains scripts that reference the old SOAP RPG Framework namespaces, you'll need to update these references to the new Astra RPG Framework namespaces. For doing this, you can use your IDE's find-and-replace-all functionality (`Ctrl + Shift + R` in VSCode):
1. Open the find-and-replace dialog in your IDE.
2. Type `ElectricDrill.SoapRpgFramework` and assert that the "Match Case" option is enabled.
3. Inspect the found occurrences to ensure they are correct.
4. Replace all occurrences that makes sense to you with `ElectricDrill.AstraRPGFramework`.
5. Save all modified files.
6. Return to Unity and recompile the project. If this is not automatic, you can force recompilation with `Ctrl + R`.

If your project compiles correctly, you're done!
If you get errors related to duplicate `.asmdef` files, follow the next steps.

### 4. Remove duplicate `.asmdef` files in the package
1. In the hierarchy of your project, navigate to the `Packages` folder and locate the `Astra RPG Framework` package.
2. Open the `Runtime` folder.
3. Ensure that there is only one `.asmdef` file named `com.electricdrill.astra-rpg-framework.Runtime`. If you find also a file named `com.electricdrill.soap-rpg-framework.Runtime`, delete it.
4. Open now the `Editor` folder of the package.
5. Ensure that there is only one `.asmdef` file named `com.electricdrill.astra-rpg-framework.Editor`. If you find also a file named `com.electricdrill.soap-rpg-framework.Editor`, delete it.
6. Click on the `com.electricdrill.astra-rpg-framework.Editor` file to select it. In the Inspector window, in the `Assembly Definition References` section, add the reference to `com.electricdrill.astra-rpg-framework.Runtime` if it's not already present, and delete any reference to `com.electricdrill.soap-rpg-framework.Runtime` if present.
7. Click on apply to save the changes.

> [!NOTE]
> Unity sometimes has unpredictable behaviors when updating `.asmdef` files in and their references. After applying the changes mentioned above, try also to close and reopen Unity to ensure that the changes are correctly applied. If you still encounter issues, fix the new problems and re-start Unity again. At that point, the changes should be correctly applied.

### 5. Recompile the Project
If Unity didn't automatically recompile the project after these changes, you can force recompilation.

Your project should now be successfully migrated to Astra RPG Framework v1.1.0.  
Happy developing!

---

### Troubleshooting

#### I re-imported Samples that I already had from v1.0.0 and now I have errors
If you used the ScriptableObject based samples of the `Utils` folder in your project, you can keep using them as they are fully compatible with the new version. In fact, **you should not re-import them as that would create duplicates in your project**.
> [!WARNING]
> Do not delete any ScriptableObject based asset that you are using in your project! Delete the duplicates from the newly imported samples instead. This will prevent data loss in your project.

Moreover, it was noticed that re-importing samples in a project that was using v1.0.0 of the package, resulted in old namespaces being used in the samples scripts. This is likely a Unity bug as does not happen on a fresh project.
If this happens, please rename the old `ElectricDrill.SoapRpgFramework` namespace in the scripts of the samples to `ElectricDrill.AstraRPGFramework` manually, analogously to what is described in step #3.

### Still Need Help?
For any issue during the migration, feel free to reach me out by sending me an email at electricdrill.info@gmail.com
