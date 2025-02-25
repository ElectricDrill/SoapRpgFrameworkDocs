# Workflows
<!--
======ENTITY
- Make a GameObject an entity
- EntityLevel
- Int and Long vars
- Growth Formulas
- Events and EventGenerators

======ATTRIBUTES
- Create some Attributes
- Create an AttributeSet
    - Assign Attributes to an AttributeSet
- Add EntityAttributes to an entity

======STATS
- Create some Stats
- Create a StatSet
    - Assign Stats to a StatSet
- Add EntityStats to an entity

======CLASS
- Create a class
- Add EntityClass to an entity

======SCALING FORMULA
- ScalingFormulas
- StatsScalingComponent
- AttributesScalingComponent
-->

## Make a `GameObject` an entity
Per rendere un `GameObject` un'entita' dobbiamo aggiungere il `MonoBehaviour` `EntityCore` ad esso. Seleziona il tuo oggetto dalla gerarchia e clicca, nell'inspector, su "Add component". quindi cerca e seleziona `EntityCore`.

(TODO)Add image of the entity core

Dall'inspector possiamo configurare una serie di valori. Analizziamoli uno alla volta.

`Level`: definisce il livello dell'entita'. Cambiando il suo valore, possiamo assegnare un livello diverso all'entita' direttamente dall'inspector. Questo puo' essere utile per ai fini di testing.
Noterete il check box `Use Constant`. Se lo spuntate, potrete passare una `IntVar` anziche' usare una costante. Per il momento ignoriamo questo tipo che specificheremo nel prossimo paragrafo.
`Current Total Experience`: Rappresenta l'esperienza totale posseduta dell'entita'. Questo valore non puo' venire modificato
