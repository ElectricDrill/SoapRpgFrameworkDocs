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

## Some utilities
Pressoche' ogni classe fornita da questo package utilizza degli eventi o delle variabili sotto forma di `ScriptableObject`. Pertanto introduciamo rapidamente questi concetti cosi' da aver chiaro di cosa stiamo parlando quando li ritroveremo nei prossimi paragrafi.

### Game events as `ScriptableObjects`
L'architettura SOAP ci permette di implementare l'Observer pattern attraverso gli scriptable objects. Nel caso piu' semplice, ovvero con eventi senza context, possiamo definire diversi game event come istanze GameEvent: una classe che deriva da `ScriptableObjcet`. Ad esempio possiamo creare un'istanza chiamata `PlayerJumped` che serve a notificare i sistemi in ascolto quando il nostro personaggio effettua un salto.
I sistemi si sottoscrivono a questo evento utilizzando il `MonoBehaviour` `GameEventListener`. Possiamo assegnare a questo componente un `GameEvent`, ed esso si occupera' di effettuare la sottoscrizione e di invocare una callback quando l'evento viene invocato. La callback e' un [UnityEvent](https://docs.unity3d.com/ScriptReference/Events.UnityEvent.html), pertanto potremo selezionare una callback da invocare in risposta al nostro evento direttamente dall'inspector.

Il package supporta anche game event con fino a 4 parametri di contesto. Alcuni game event sono gia' definiti e resi disponibili dal package (vedi la pagina [Samples](samples.md)).

### Int and Long Vars

### Int and Long Refs


## Make a `GameObject` an entity
Per rendere un `GameObject` un'entita' dobbiamo aggiungere il `MonoBehaviour` `EntityCore` ad esso. Seleziona il tuo oggetto dalla gerarchia e clicca, nell'inspector, su "Add component". quindi cerca e seleziona `EntityCore`.

(TODO)Add image of the entity core

Dall'inspector possiamo configurare una serie di valori. Analizziamoli uno alla volta.

`Level`: definisce il livello dell'entita'. Cambiando il suo valore, possiamo assegnare un livello diverso all'entita' direttamente dall'inspector. Questo puo' essere utile per ai fini di testing.
Noterete il check box `Use Constant`. Se lo spuntate, potrete passare una `IntVar` anziche' usare una costante.
`Current Total Experience`: Rappresenta l'esperienza totale posseduta dell'entita'. Questo valore non puo' venire modificato
