## Vorlesung 6

### Das Wörterbuchproblem

Gegeben:  
Universum $U$ (sehr groß)  
Teilmenge $S \subseteq U$ (mittelgroß) $|S|=n$  
$m \in \mathbb{N}$

Ziel: Finde
$h: U \rightarrow \{0, 1, ..., m-1\}$ sodass

$\forall ~ 0 \leq i < m: ~ \Big| \{ x \in S ~|~ h(x) = i \} \Big| \leq \lfloor \tfrac {n} {m} \rfloor$

---

Bspw:
$U=\mathbb{N}$  
$S={1,77,23,99} ~~(n=4)$  
Gute Funktion h fpr $S$ wäre zum Beispiel  
$h(x)=x ~mod~5$

$h(1)=1$  
$h(2)=2$  
$h(23)=3$  
$h(99)=4$  

Gleiche Hachfunktion für:
$S ={12,22,17,32,52}$

Anwendungsbeispiele:
1) Telefonbuch:  
Mit Name und Vorname die zugehörige Telefonnummer rausfinden  
$U \widehat{=}$ Menge aller Zahlen die irgendeine Name und Vorname Kombination entsprechen  
$S \subseteq U~\widehat{=}$ Name und Vorname in Stuttgart $|n|= 500000$  
Wähle $m=500000$  
Könnten wir eine Hashfunktion $h:U\rightarrow {0,...,m-1}$ abbildet mit:
$\forall 0 \leq i < m: ~~|{x\in S | h(x) = i}|\leq \tfrac{n}{m}=1$  
Das ist toll, wir können ein Array anlegen mit 500000 Einträgen. Die Telnr. einer "Name und Vorname" speichern wir an Position h(Name und Vorname), des Arrays.

2) Stowasser (Latein-Deutsch Wörterbuch)  
$U...$ Menge aller Wörter  
$S...$ Menge aller Lateinwörter  
...  
3) Beim randomisierten CP-Alg:  
$U...$ Menge aller Paare $(i,j)$, $i \in \mathbb{Z}, j \in \mathbb{Z}$  
$S...$ Menge der Paare $(i,j)$, für die in enspr. Gitterzelle mind. 1 Punkt liegt.  
4) OpenStreetMapDaten  
basiert auf Knoten & Kanten
Anzahl der Knoten ungefähr 4-5Mrd.  
$U...\mathbb{N}$  
$S...$ Menge der Knoten IDs im aktuellen Kartenausschnitt.  
Bei guter Hashfunktion $h$ könnte man in $\mathcal{O}(1)$ Information mit $S$ assoziieren oder auslesen.

---

**Betrachten wir ein Wörterbuch als abstrakte Datenstruktur.**  
Diese sollte folgende Operationen unterstützen:  

- $S$= MAKESET() ... (ein Wörterbuch erzeugen)  
- inSet($x,S$) ... fügt item $x$ in $S$ ein  
> item $x$: besteht aus ($K$,inf)  
> $K$ = Schlüssel; inf= Information  

- delete($x,S$) ... löscht item $x$ aus Wörterbuch  
- lookup($K,S$) ... gibt item $x$ = ($K$,inf) aus, falls vorhanden

**Naive Implementierungen**
1) Array/Feld für die Items, Zähler für Anzahl Items in $S$

| $0$ | $1$ | ... | $n-1$ |
|---|---|---|---|
| $i_0 = (K_0, inf_0)$ | $i_1= (K_1, inf_1)$ | ... | $i_{n-1}= (K_{n-1}, inf_{n-1})$ |

- insert($x,S$) ... Überprüfe ob Schlüssel von $x$ schon enthalten, falls ja überschreiben des entschprechenden items, falls nein, Item hinten anhängen & Zähler eins erhöhen  
- delete($x,S$) ... item finden, austausch mit letztem Item, Zähler eins runter  
- lookup($k,S$) .. alle Items durchgehen, Item mit passendem Schlüssel zurüchgeben  
- MAKESET() ...  
> Vorteile: Einfach, platzsparend

2) Verkettete Liste (einfach oder doppelt)  
... ähnlich, aber braucht nicht unbedingt zusätzlichen Speicher
3) Direkte Adressierung  
Annahme: Schlüsseluniversum endlich, $[0,...,n-1]$  
Wir spendieren ein Array der Größe $K$.  
An Position $i$ des Arrays steht das Item mit Schlüssel $i$ (falls vorhanden) oder Sonderwert "NIL" falls keines vorhanden.  
Insert/Delete/Lookup haben alle Zeitverbrauch $\mathcal{O}(1)$.  
*Problem:* Platzverbrauch ~Größe des Schlüssel**universums**  
> zB bei verketteter Liste war Platzverbrauch ~Anzahl zu verwaltender Elemente  


**Ziel:** Datenstruktur für Wörterbuch, welches  
- $\mathcal{O}(1)$ Zugriffszeit für insert/delete/lookup
- $\mathcal{O}(n)$ Platzverbrauch  

garantiert.

---

> **Achtung:** er mixt Schlüssel und Item

Idealierweise hätten wir gerne für ein gegebenes  
$S \subseteq U = {0,...,2^64-1}, |S|=n$
eine hashfunktion $h$, welches $S$ injektiv auf {0,...,n-1} abildet, d.h.
$\forall x,y \in S~:~h(x)\neq h(y)$
Mit einem solchen h könnten wir $S$ wie folgt verwalten:  
- lege Array mit n Einträgen für die Items an
- Item mit Schlüssel $x$ wird an Position $h(x)$ gespeichert ( an keiner Stelle wird mehr als 1 Item gespeichert)
- lookup($x,S$): Schaue an Position $h(x)$ nach:

**Vorteile:**
- Zugriffszeit $\mathcal{O}(1)$
- Größe $\mathcal{O}(|S|)$

----

- $U$ ... Schlüsseluniversum
- $T[0,...t-1]$ ... Feld auch hashfeld genannt
- $S \subseteq U$ ... Teilmenge von Schlüsseluniversum, welche gehasht werden soll

**Gesucht:** $h:U \rightarrow [0,...,t-1]$  
$C^{h}_{x}(S):={y \in S| h(y) = h(x)}$ ... die Menge an Elementen in S, die von h auf derselben Hashtafeleintrag gemappt werden wie x

Idealierweise $C^{h}_{x}(S)\leq1$ ($\Rightarrow$ h injektiv für S)

Falls $t<|S|$, hätte man gerne $|C^{h}_{x}(S)$

**Frage: Gibt es eine Superhashfunktion** $h^*$, welche für jedes $S \subseteq U$ garantiert, dass
$|C^{h}_{x}(S)| \leq \tfrac{\lceil|S|\rceil}{t}~~~~?$

**Bsp:** $U= \mathbb{N}$  
$S={20,13,6,18,28,31}, t=5$  
$h(x)=x~mod~5$

TABELLE

Für $S={11,33,22,19,5}$ wäre daraus h perfekt.  
Für $S={16,5,1,26,31,13}$ ist dieses Katastrophal.  

**Satz:** Seien $U,t$ und $h$ gegeben, $|U|=K$.  
Dann gibt es für alle n mit $1 \leq n \leq \tfrac{K}{t}$  
ein $S \leq U$ mit $|S| = n$

Schreibe auch $S \in (^K_n)~~~~~~(^{70}_{12})$

mit $|C^{h}_{x}(S)|=n ~~~\forall x \in S$, d.h. alle Elemente aus $S$ werden in den gleichen Hashtafeleintrag gehasht.

> Eventuell Prüfungsrelevant

**Beweis:**   
Nach Schubfachprinzip existiert ein i  
$0 \leq i < t \text{ sodass } (h^{-1}(i)) \geq \tfrac{|U|}{t}$
> ${x \in U| h(1)= i}$

$\Rightarrow \tfrac{|U|}{t} = \tfrac{K}{t} \geq n$. Wähle $S$ aus $(^{h^{-1}(i)}_n)$

$\Rightarrow$ Es gibt also keine Superhashfkt die für jedes $S$ gut ist; man sollte also die Hashfkt. $h$ randomisiert oder unter Berücksichtigung von S bestimmen.
