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

## How is Simple RPG Core organized and how does it work?
The package is developed around the concept of entities, so let's clarify what we mean by this term in the context of Simple RPG Core. In its most minimal version, an entity is a `GameObject` that has a set of statistics. Optionally, an entity can have attributes, can level up, thus having experience and a level, and can have a class.
Let's clarify what we mean by each mentioned term.

### Statistics
A statistic is a value that quantifies an aspect of the entity. The meaning of this aspect is solely due to the concept it refers to.
### Examples
In an RPG, a statistic can be `physical damage`. The concept of physical damage refers the player to the amount of damage inflicted by physical attacks, whether with weapons or without. Other statistics can be `ability power`, `defense`, `speed`, `armor penetration`, `range`, etc.

### Attributes
An attribute is a value that can influence the value of one or more statistics. The weight of its influence on the statistics can be variable.
#### Examples
In an RPG, these can be `Strength`, `Dexterity`, `Intelligence`, `Constitution`, etc.
Considering the previous example of statistics, `Strength` could influence `physical damage`, `Dexterity` would increase `speed`, `Intelligence` would increase `ability power`, and `Constitution` would increase `defense`.

### Experience and Level
The entity can gain experience and level up. This functionality is used by the class to express how attributes and statistics grow with levels, for that particular class.

### Class
The class is associated with a set of statistics and optionally a set of attributes. The class describes how statistics and attributes vary with levels.
### Examples

## How is Simple RPG Core implemented?
Il package e' sviluppato seguendo i principi SOAP (Scriptable Object Architecture Pattern). I maggiori benefici forniti da questa architettura sono:
- separazione della logica di gioco dai dati
- i game designer possono effettuare modifiche dall'inspector, senza dover toccare il codice
- maggiore testabilita'. Si possono sostituire le istanze con il comodo meccanismo drag and drop fornito dall'inspector di Unity
- maggiore riusabilita'. Ogni oggetto e' uno scriptable object che puo' venire riutilizzato da molte componenti

## Flexibility of Simple RPG Core
Although the package is specifically designed for RPG games or games with progression systems, its flexibility allows it to be used in almost any game.
As it allows creating attributes like `strength`, `dexterity`, `agility`, etc., and statistics such as `physical attack`, `magic power`, `physical defense`, etc., in RPG, Roguelike, MMO games, etc., nothing prevents it from being used, for example, to implement a firearm. The attributes could be `weight`, `size`, `ergonomics`, etc., and the statistics `recoil`, `handling`, `stability`, `intimidation`, etc. Attributes can influence statistics. A heavier weapon could reduce `handling` but increase `stability`. A larger weapon could reduce `handling` but increase `intimidation`. A more ergonomic weapon could reduce `recoil` and increase `handling`. And so on...
The weapon's levels, if present, influence the attributes and statistics, progressively improving them.
Classes could represent weapon types (assault rifles, snipers, shotguns, etc.), and each class could have its own set of dedicated attributes and statistics. For example, shotguns could have the attribute `barrel length` that influences the statistic `pellet spread`.
