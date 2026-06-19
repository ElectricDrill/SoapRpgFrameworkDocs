---
_layout: landing
---

> [!NOTE]
> Join the Astra Discord server!  
> There is now a dedicated **Discord server** for Astra Framework and its extensions.
> Join to **receive notifications** about new extension releases and important updates, to **ask for new features**, **report bugs**, **share ideas**, and **showcase your Astra creations** with other developers.  
> <span style="font-size:1.18em; font-weight:600;">💬 Join the Discord Server: https://discord.gg/nJVRMkGrZg</span>

## 🛠️ Astra Framework

### 👉 Introduction

**Say goodbye to endless recompilations. Instantly balance your game's stats and formulas with a powerful data-driven system.**

Astra Framework provides a complete, data-driven backbone for your game, letting you define your game's foundations directly in the editor and iterate on **balance during play mode**.

Simple, lightweight, and easy to learn. No configuration required: **works out of the box with zero setup**.

---

### ✨ Key Features

- 📊 **Statistics & Attributes:** Define the core building blocks for your game
- ⭐ **Experience & Levels:** Build a robust progression system
- 🧑‍🎓 **Classes & Progression:** Design custom classes and unique progression paths
- 📈 **Scaling Formulas:** Create complex formulas for damage, healing, and other values using entities' stats and attributes
- ⚡ **Event System:** Enable dynamic game-object interactions with inspector-wired events and listeners
- 🛠️ **Utilities:** Helpful tools to streamline development. Ever used Int and Long variables as ScriptableObjects? Discover new possibilities!

---

### ⚙️ Implemented on top of ScriptableObjects architecture

- 🎨 **Designer-friendly:** Most of the features are ScriptableObjects, easily created and wired in the Unity Inspector. Custom inspectors are available for everything.
- 🧩 **Reusable and modular:** Objects can be reused across game objects and scenes. They are the building blocks for your game. **Instantiate, compose, reuse**. Changes instantly reflect on dependent objects. No duplication, no hassle.
- 🧪 **Easy testing:** Swap objects, switch from class-based stats to fixed ones, or replace complex formulas—all without leaving play mode. Debugging and testing is effortless.

---

### 🔍 Features Breakdown

#### 🕸️ Attributes

Attributes are the core numerical values that define an entity's capabilities. In most games these include strength, dexterity, intelligence, constitution, etc. You define your own attributes, choosing their names and min/max values as needed.

Group attributes into attribute sets—shareable objects reusable across entities.

Supports both flat and percentage modifiers.

#### 📊 Statistics

Statistics are derived values that directly affect gameplay, such as Physical Attack, Armor, Crit Chance, and movement speed. Define custom statistics and organize them into sets tailored for each entity. Statistic sets can be modular—for example, create separate sets for movement, offense, and defense, then assign only what's relevant to each entity.

Statistics can scale with attributes. Define how each statistic scales based on one or more attributes.

Like attributes, statistics can have lower and upper bounds.

Statistics support flat and percentage modifiers, plus powerful stat-to-stat modifiers. Create buffs/debuffs that change a statistic based on another statistic's value.

#### ⭐ Experience & Levels

Define custom level progression curves and experience requirements for each level.

If an entity doesn't level up, set fixed base stats and attributes. Set up simple entities in seconds.

#### 🧑‍🎓 Classes & Progression

Classes (like warrior, mage, rogue, etc.) let you define unique progression paths for attributes and statistics. Assign progression curves for each attribute and statistic to each class for distinct play styles.

Balance progression curves in real-time—**test and iterate instantly during play mode**.

#### 📈 Scaling Formulas

Easily build complex formulas for damage, healing, and other values using stats and attributes. Drag and drop scaling components to visually assemble formulas in the custom inspector.

Formulas are fully customizable. Set a fixed base value or make it scale dynamically with levels. Add scaling components to adjust values based on the caster’s or target’s stats and attributes—or both.

Add and remove temporary scaling components at runtime for advanced mechanics. For example, a potion might grant "Your next base attack deals extra damage equal to 50% of your Constitution." Drinking it adds a temporary scaling component to the base attack formula; after the attack, the temporary scaling component is removed.

Reuse formulas across entities and scenarios for consistency and reduced redundancy.

**See the impact of changes instantly in play mode, enabling rapid iteration and fine-tuning of game balance.**

#### ⚡ Game Events

Game events are ScriptableObjects. Instantiate events in your assets, then drag & drop them into scripts that raise them and into Game Event Listeners that define responses. Testing your game's logic from the inspector is easier than ever.

The framework provides a set of predefined game events (and MonoBehaviour listeners) to get you started quickly:

- **GameEvent:** No context parameters. For simple events like 'Player Jumped', 'Player Slept', etc.
- **IntGameEvent:** One integer context parameter.
- **EntityLeveledUpGameEvent:** An event that carries along an EntityCore and an int as context parameters, where the EntityCore is the entity that leveled up and the int is the new level reached.
- **StatChangedGameEvent:** Notifies when a stat changes for a specific entity.
- **AttributeChangedGameEvent:** Notifies when an attribute changes for a specific entity.

#### 🏭⚡ Game Event Generators

Game Event Generators are powerful tools for generating C# source code for custom game events. Use the custom inspector to create new event types with up to 4 context parameters—these can be native C# types or your own classes.

Game Event Generators are also ScriptableObjects. Instantiate multiple generators to organize different sets of events for your project. Each generator can manage its own context-specific events.

#### ⚛️ Game Actions
Game Actions, deriving from ScriptableObject, allow to incapsulate game logic in reusable assets to share across multiple game objects and scenes.

They are implemented on top of Awaitables to support asynchronous operations and chaining of game logic. They can be even used with UnityEvents, meaning that you can drag&drop them in compatible GameEventListeners and invoke their logic upon event raising.

---

### 🛒 Where to Buy

Unity Asset Store: [Astra Framework](https://assetstore.unity.com/packages/tools/game-toolkits/astra-rpg-framework-334926)

---

### 📬 Information & Contact

Questions, feedback, or bugs? Email us at [electricdrill.info@gmail.com](mailto:electricdrill.info@gmail.com).
