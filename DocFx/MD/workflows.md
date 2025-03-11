-# Workflows
<!--
======UTILITIES
- Game Events as SOs
- Int and Long Vars
- Int and Long Refs
- Growth Formulas

======ENTITY
- Make a GameObject an entity
- EntityLevel

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
L'architettura SOAP ci permette di implementare l'Observer pattern attraverso gli scriptable objects. Nel caso piu' semplice, ovvero con eventi senza context, possiamo definire diversi game event come istanze `GameEvent`: una classe che deriva da `ScriptableObjcet`. Ad esempio possiamo creare un'istanza chiamata `PlayerJumped` che rappresenta l’evento “Il giocatore ha effettuato un salto”. Questo evento si occuperà di notificare tutti i sistemi in ascolto quando esso si verifica.
I sistemi si sottoscrivono a questo evento utilizzando il `MonoBehaviour` `GameEventListener`. Possiamo assegnare a questo componente un `GameEvent`, ed esso si occupera' di effettuare la sottoscrizione e di invocare una callback quando l'evento viene invocato. La callback e' un [UnityEvent](https://docs.unity3d.com/ScriptReference/Events.UnityEvent.html), pertanto potremo selezionare una callback da invocare in risposta al nostro evento direttamente dall'inspector.

Il package supporta anche game event con fino a 4 parametri di contesto. Essi sono dei Generics, ma in Unity non è possibile stanziare classi che derivano da ScriptableObject se sono dei generici con parametri di tipo non specificati. Per usarli dobbiamo quindi dichiarare esplicitamente delle classi che derivano dai GameEvent generici e che fissano i parametri di tipo con dei tipi concreti. Per semplificare la definizione di nuovi tipi di evento, quindi con degli specifici tipi come parametri di contesto, il package mette a disposizione i `GameEventGenerator`. Questi generatori, che derivano da SO, permettono di generare le classi concrete dei `GameEvent`.
Vedremo più in dettaglio questi generatori nella sezione (TODO)
Alcuni game event sono gia' definiti e resi disponibili dal package (vedi la pagina [Samples](samples.md)).

### Int and Long Vars
Un’altro comune utilizzo degli `ScriptableObject` nella SOAP architecture è per definire delle variabili. Il maggiore vantaggio di queste variabili sotto forma di SO è che possono venire facilmente condivise tra vari oggetti che magari decidano condividere lo stesso valore. Un comune esempio è il punteggio di gioco del giocatore. Potrebbe esserci il game manager che aggiunge o toglie punti a questa variabile, mentre l’HUD della UI lo utilizza per mostrare a video il suo valore. In questo modo riusciamo a mantenere game manager e UI completamente disaccoppiate, passando i valori condivisi (come le variabili) attraverso l'inspector.

### Int and Long Refs
`IntRef` e `LongRef` permettono di scegliere se usare un valore nativo (`int` o `long`) o se usare un `IntVar`/`LongVar`. Come abbiamo detto nel paragrafo precedente, gli `IntVar` e `LongVar` hanno il vantaggio di essere condivisibili tra diverse componenti/oggetti di gioco, mentre i valori nativi sono piu' immediati da usare e richiedono meno setup (non serve istanziare un'istanza di un `IntVar`/`LongVar` e assegnarla nell'inspector). 

Grazie a un custom property drawer sara' possibile, dall'inspector, spuntare un checkbox nominato `Use constant` per utilizzare un valore nativo anziche' un -`Ref`, e viceversa.

`IntRef` e `LongRef` sono ampiamente utilizzati nei `MonoBheaviour` del pacakge.

### Growth Formulas
Come gia' menzionate in [Introduction](introduction.md), le `GrowthFormula` permttono di definire come un certo valore varia al crescere dei livelli. Una `GrowthFormula` puo' venire istanziata attraverso il menu di contesto della gerarchia andando su `Simple RPG Core -> Grwoth Formula`.  
Il package mette a disposizione un custom property drawer per le `GrowthFormula`.

#### Max level for the values
Nell'inspector di una `GrowthFormula` possiamo infatti passare un `IntVar` per definire fino a che livello far crescere i valori.

#### Use constant at level one
If the checkbox named `Use constant value at level 1` is checked, the respective constant value will be used.

#### Growth equations
The various values of the `GrowthFormula` are defined by a function where values, the y axis, are expressed in function of the levels, the x axis. Such funtion is defined as a system of equations. Each equation is a string that associates a math expression to a range of levels.
The string can be defined by using the [Unity ExpressionEvaluator](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/ExpressionEvaluator.html) syntax. On top of it, the following terms can be used:
- `LVL`: the level at each iteration
- `PRV`: the previous value of the `GrowthFormula` (value evaluated at the previous level)
- `SPRV`: the second previous value of the `GrowthFormula` (value evaluated 2 levels ago)
- `SUM`: the sum of the values of the `GrowthFormula` from level 1 up to the previous level

## Make a `GameObject` an entity
Per rendere un `GameObject` un'entita' dobbiamo aggiungere il `MonoBehaviour` `EntityCore` ad esso. Seleziona il tuo oggetto dalla gerarchia e clicca, nell'inspector, su "Add component". quindi cerca e seleziona `EntityCore`.
![Entity Core Custom Editor](../images/workflows/entity-core-editor.png)  
*Image - Entity Core Custom Editor*

Dall'inspector possiamo configurare una serie di valori. Analizziamoli uno alla volta.

`Level`: definisce il livello dell'entita'. Cambiando il suo valore, possiamo assegnare un livello diverso all'entita' direttamente dall'inspector. Questo puo' essere utile per ai fini di testing.
Noterete il check box `Use Constant`. Se lo spuntate, potrete passare una `IntVar` anziche' usare una costante.
`Current Total Experience`: Rappresenta l'esperienza totale posseduta dell'entita'. Questo valore, qualora `Use constant` sia spuntato, e' readonly. 
> [!WARNING]  
> Se avete invece passato una `LongRef`, il valore contenuto in tale variabile non dovrebbe venire modificato a mano.