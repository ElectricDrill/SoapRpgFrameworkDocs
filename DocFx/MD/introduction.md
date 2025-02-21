# ToC
1. Purpose of SRPGCore in one quote
2. Why should I use SRPGCore?
4. How is SRPGCore organized?
5. How is SRPGCore implemented?
6. Flexibility of SRPGCore
7. Limitations of SRPGCore

# Introduction
Simple RPG Core eases managing classes, experience, attribute scores, statistics, and their growth with levels, as well as scaling values based on them.

## Why should I use Simple RPG Core?
It simplifies and speeds up game development by relieving you from the responsibility of implementing your own system for levels, attributes, statistics, and scaling calculations.
This package allows you to focus on the relevant aspects of your game development, offloading an important part of the development that remains common and monotonous in many games.

## Vocabulary of Simple RPG Core
The package is developed around the concept of _entity_, so let's clarify what we mean by this term in the context of Simple RPG Core. In its most minimal version, an entity is a `GameObject` that has a set of statistics. Optionally, an entity can have attributes, can level up, and can have a class.
Let's clarify what we mean by each mentioned term.

### Statistics (Stat)
A statistic is a value that quantifies an aspect of the entity. The meaning of this aspect is solely due to the concept it refers to.
#### Examples
In an RPG, a statistic can be `physical damage`. The concept of physical damage refers the player to the amount of damage inflicted by physical attacks, whether with weapons or without. Other statistics can be `ability power`, `defense`, `speed`, `armor penetration`, `range`, etc.

### Attributes
An attribute is a value that can influence the value of one or more statistics. The weight of its influence on the statistics can be variable.
#### Examples
In an RPG, attributes can be: `strength`, `dexterity`, `intelligence`, `constitution`, etc. Considering the previous example of statistics, `strength` could influence `physical damage`, `dexterity` would increase `speed`, `intelligence` would increase `ability power`, and `constitution` would increase `defense`.

### Experience and Level
The entity can gain experience and level up. This functionality is used by the class to express how attributes and statistics grow with levels, for that particular class.

### Class
The class is associated with a set of statistics and optionally a set of attributes. The class describes how statistics and attributes vary with levels.
#### Examples
In RPGs most common classes are: `warrior`, `rogue`, `mage`, `paladin`, and so on. These classes have different attribute values. For example, a warrior will have more `strength` and `constitution` than a mage. The `rogue` might have the highest `dexterity`, etc.

## How is Simple RPG Core organized and how does it work?

### Entity
A `GameObject` becomes an entity once the `EntityCore` and `EntityStats` `MonoBehaviour`s (Mono) are added to it. `EntityCore` comes with a built-in `EntityLevel` (plain C# `class`) that manages the experience and the level of the entity.

### Stat
A `Stat` is a class that derives from SO and represents a statistic in the game. Each statistic has a name (the name given to the SO instance of the created `Stat`), and we can choose whether to provide it with a maximum and/or minimum value. Additionally, we can define how that statistic grows or is reduced, depending on certain `Attributes`.

### StatSet
A `StatSet` is a class that derives from SO and defines a set of `Stats`.

### EntityStats
`EntityStats` allows us to configure:
- the base statistics
- the flat modifiers
- the _StatToStat_ modifiers
- the percentage modifiers
We will see what these modifiers are in the section (TODO).

The base statistics can be _fixed_, or instead derive from a class if the entity has one assigned. If we use the fixed ones, we must also provide a `StatSet`, while if we use those of a class, the class's `StatSet` will be used. If the entity levels up and we want its statistics to grow with levels, we are forced to use a class, as the _fixed_ statistics are immutable.

### Class
`Class` derives from SO and represents a game class. Each class has a name, a `GrowthFormula` that defines how the base Max HP grows with levels, a `StatSet`, optionally an `AttributeSet`, and associates each `Stat` of the provided StatSet with a `GrowthFormula` that describes how the statistic varies with levels. Similarly, if an `AttributeSet` is provided, it will be possible to associate a `GrowthFormula` for each `Attribute` present in the set, to describe how the attributes vary with levels.

### EntityClass
`EntityClass` derives from Mono and allows us to assign a `Class` to our entity.

### EntityAttributes
Optionally, we can add the Mono `EntityAttributes` to our entity if we want to give it attributes. `EntityAttributes` allows us to specify how many attribute points to provide at each new level. These points can be spent on various attributes to increase their value. For `EntityAttributes` we can configure:
- the base attributes
- the flat modifiers
- the percentage modifiers
Similarly to `EntityStats`, we can decide whether the base attributes are _fixed_ or if they instead derive from the class associated with `EntityClass`.

## How is Simple RPG Core implemented?
The package is developed following the principles of SOAP (Scriptable Object Architecture Pattern). The main benefits provided by this architecture are:
- separation of game logic from data
- game designers can make changes from the inspector without touching the code
- greater testability. Instances can be replaced with the convenient drag and drop mechanism provided by the Unity inspector
- greater reusability. Each object is a `ScriptableObject` (SO) that can be reused by many components

## Flexibility of Simple RPG Core
Although the package is specifically designed for RPG games or games with progression systems, its flexibility allows it to be used in almost any game. As it allows creating attributes like `strength`, `dexterity`, `agility`, etc., and statistics such as `physical attack`, `magic power`, `physical defense`, etc., in RPG, Roguelike, MMO games, etc., nothing prevents it from being used, for example, to implement a firearm. The attributes could be `weight`, `size`, `ergonomics`, etc., and the statistics `recoil`, `handling`, `stability`, `intimidation`, etc. Attributes can influence statistics. A heavier weapon could reduce `handling` but increase `stability`. A larger weapon could reduce `handling` but increase `intimidation`. A more ergonomic weapon could reduce `recoil` and increase `handling`. And so on... The weapon's levels, if present, influence the attributes and statistics, progressively improving them. Classes could represent weapon types (assault rifles, snipers, shotguns, etc.), and each class could have its own set of dedicated attributes and statistics. For example, shotguns could have, in addition to the aforementioned ones, the `barrel length` attribute that influences the `pellet spread` statistic.
