## Vorlesung 7

### Das Wörterbuchproblem

#### Annahme:

Universum $\mathcal U = \{0,1,...,N-1\}$  
Bspw. alle OSM IDs.

Abzuspeichernde Schlüsselmenge $S \subseteq \mathcal U$ mit $|S| = n$  
Bspw. alle im Raum Stuttgart vorkommende OSM IDs.

$n \ll N$

Hashtafel:

$T:$
|0|1|2|3|4|
|-|-|-|-|-|

#### Gesucht:

Gute Hashfunktion $h: ~ \mathcal U \rightarrow \{0, ..., m-1\}$ für $S$

Wir wollen dann Informationen zu $x \in S$ in Hashfeldeintrag $h(x)$ speichern.

#### Letztes Mal:

##### Theorem:

Es gibt keine immer ( $\forall S \subseteq \mathcal U$ ) "gute" Hashfunktion.

##### Beispiel:

$U = \{0, ..., 2^{64} -1\}$

$S = \{3, 7, 9, 10\}$

$h(x) = x ~mod~ 5$

$T:$
|0|1|2|3|4|
|-|-|-|-|-|
|_10_||_7_|_3_|_9_|

Für dieses $S$ ist $h$ sehr gut.  
Für $S = \{6,11,31,46\}$ aber nicht.

Problem, wenn $h(x_1) = h(x_2)$ für $x_1 \neq x_2$.

#### Hashing mit Verkettung

Jede Tafelposition ist Kopf einer verketteten Liste. Alle $x \in S$ mit $h(x) = i$ werden in $i$-ter Liste gespeichert.

Platzbedarf: $\mathcal O(|T| + |S|) = \mathcal O(m + n) = \mathcal O(n (1 + \tfrac 1 \beta))$ mit $\beta = \tfrac n m$ (Belegungsstufe).

Je kleiner $\beta$, desto platzineffizienter wird das Wörterbuch (aber dann evtl. einfacher, gute Hashfunktion zu finden).

Zugriffszeiten:  
> unter Annahme dass $h(x)$ in $\mathcal O(1)$ ausgewertet werden kann.

Zugriff auf $x \in S$

$\mathcal O(1 +$ Position von $x$ in Liste $L_{h(x)})$

Zugriff auf $x \notin S$

$\mathcal O(1+ |L_{h(x)}|)$

---

#### Zum Aufwärmen:

##### Annahme:

$h$ verteilt $\mathcal U$ gleichmäßig über $T$, d.h. $\forall i | \{x \in \mathcal U | h(x) = i \} | \leq \lceil \tfrac N m \rceil$

Bsp: $h(x) = x ~mod~ m$

##### Satz:
Sei $x$ ein zufälliges Element aus $\mathcal U \backslash S$ und $n \leq \tfrac N 2$. Dann ist die erwartete Suchzeit für $x$ in $\mathcal O(1 + \beta)$.

##### Beweis:
$l_i = \#$ Elemente aus $S$, die in $L_i$ gespeichert werden, mit $i = 0, ..., m - 1$.

Es gilt: $\sum_{i=0}^{m -1} l_i = n$

##### Die erwartete Suchzeit ist:

$(\sum_{i=0}^{m -1} Pr(h(x) = i) \cdot l_i) + 1$

$\mathcal U_i \subseteq \mathcal U$ Menge aus $\mathcal U$, welche von $h$ auf $i$ gemappt wird.

$Pr(h(x) = i) = \tfrac {\mathcal U_i \backslash S}{\mathcal U \backslash S} \leq \tfrac {\mathcal U_i} {\mathcal U \backslash S} \leq \tfrac{\lceil \tfrac N m \rceil } {\tfrac N 2} \leq \tfrac {\tfrac N {m+1} } {\tfrac N 2} = \tfrac 2 m + \tfrac 2 N \leq \tfrac 2 m + \tfrac 1 n$

##### Beweis:
//TODO Beweis vom Foto

Das heißt für zufällige Elemente nicht aus $S$ ist erwartete Zugriffszeit ok.

##### Problem:
In der Praxis sind Zugriffe nicht zufällig.

##### Satz:
Sei $x$ ein zufälliges Element aus $S$. Dann ist die erwartete Zugriffszeit für $x$:
$\mathcal O(1 + \tfrac 1 n \sum_{0=1}{m-1}(\tfrac{l_i(l_i + 1)}2))$.

###### Anmerkung:
Satz nicht besonders aussagekräftig, da $\sum l_i = n$ fast nichts aussagt über $\sum\tfrac{l_i(l_i + 1)}2$

###### Beweis:
Wenn $x$ das $j$-te Element in seiner Liste ist, dann ist Suchzeit $\mathcal O(1 +j)$. Also ist die erwartete Suchzeit:
$\mathcal O(\tfrac 1 n \sum_{i=0}^{m-1} \sum_{j=n}^{l_i} (1+j)) = \mathcal O(\tfrac 1 n \sum_{i=0}^{m-1} \tfrac{l_i (l_i + 1)} 2)$.

Satz: Sei $S$ eine zufällige gleichverteilte Teilmenge aus $\mathcal U$ der Größe $n$: Dann ist die erwartete Zugriffszeit auf ein zufälliges $x \in S$.

$\leq 1 + \beta \cdot \tfrac 3 2 \cdot e ^ \beta$
(für nicht zu großes $\beta$, z.B. $\beta = 1$, ist das $\mathcal O(1)$).

Wieder unbefriedigend: In der Anwendung ist weder $S \subseteq \mathcal U$ zufällig, noch greift man typischerweise zufällig auf die Elemente in $S$ zu.

### Universelles Hashing

Sei $\mathcal H$ eine Menge von Funktionen von $\mathcal U$ nach $\{0,1,...,m-1\}$

#### Definition:
Für $c > 1$ heißt $\mathcal H$ c-universell falls für alle $x, y \in \mathcal U$, $x \neq y$ gilt:

$\tfrac {|\{h \in \mathcal H ~:~ h(x) = h(y)\}|} {|\mathcal H|} \leq \tfrac c m$

#### Satz:
Für $a,b \in \{0, ... N-1\}$ sei $h_{a,b} : x \rightarrow ((ax + b) ~mod~ N) ~mod~ m$ und sei $N~ prim$

##### Dann ist die Klasse:

$\mathcal H = \{ h_{a,b} ~:~ 0 \leq a, b \leq N-1 \}$

$c$-universell mit $c = \tfrac {\lceil \tfrac N m \rceil} {\tfrac N m} \approx 1$

##### Beweis: Siehe Übung.
//TODO Lösung der Übung hier einfügen.

#### Satz:
Benutzt man Hashing mit Verkettung und wählt $h \in \mathcal H$ zufällig gleichverteilt (mit $\mathcal H$ $c$-universell), dann ist die __erwartete__ Zugriffszeit für Zugriffe:

$\mathcal O(1 + c \cdot \beta)$ für __beliebige__ Mengen $S \subseteq U$, $|S| = n$

#### Beweis:
Zeit für Zugriff auf $x$

$\leq 1 + \# y \in S$ mit $h(x) = h(y)$

Definiere
$\delta_h (x,y) = \left\{ \begin{matrix}
  {{1 ~~~ \text{falls } h(x)=h(y)} \\
  {0 ~~~ \text{sonst} ~~~~~~~~~~~~~~~~~~~}}
\end{matrix}\right.$

##### Uns interessiert:

$\tfrac 1 {\mathcal H} \sum_{h \in \mathcal H} \sum_{y \in S} \delta_h(x,y) = \tfrac 1 {\mathcal H} \sum_{y \in S} \sum_{h \in \mathcal H}  \delta_h(x,y)$

> Falls $x \neq y$: Anzahl der Hashfunktionen $h \in \mathcal H$ die $x$ in gleichen Hashtafeleintrag mappen wie $y$  
> Sonst (falls $x=y$) : $|\mathcal H|$

Da $\mathcal H$ $c$-universell:

$\leq \sum_{y \ in S} [ \text{falls } x = y \text{ then } 1 \text{ else } \tfrac c m ] \leq \left\{
  \begin{matrix}
    { 1 + \tfrac {c(n-1)} {m} ~~~ \text{für} ~~~  x\in S } \\
    { \tfrac c m \cdot n ~~~ \text{für} ~~~ x\notin S }
  \end{matrix}
  \right\}
\leq 1 + c \cdot \beta$

Noch besser wäre nicht nur __erwartete__ Zugriffszeit $\mathcal O(1)$, sondern __worst-case__ Zugriffszeit $\mathcal O(1)$

=> Perfektes Hashing
