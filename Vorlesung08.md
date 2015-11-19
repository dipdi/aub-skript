## Vorlesung 8

#### Bisher:

Wenn wir $h$ aus einer geeigneten Familie von $c$-universellen Hashfunktionen zufällig wählen, ist erwartete Zugriffszeit $\mathcal O(1)$ - bei Platz $\mathcal O(n)$.  
__Egal__ wie $S$ aussieht.

#### Jetzt:

Garantierte Worst-Case Zugriffszeit von $\mathcal O(1)$ bei Platz $\mathcal O(n)$.

---

### Perfektes Hashing

##### Good to know:
$c$ bedeudet Anzahl der Kollisionen.

#### Ziel:

Möchte $h$ finden mit $h(x) \neq h(y) \forall x,y \in S, x \neq y$
und $h$ bildet in eine Hashtafel der Größe $\mathcal O(|S|)$ ab.

##### 1. Versuch:

##### Einstufiges Perfektes Hashing

$c_S(h) = | \{ \{ x, y \} \in \binom S  2 : h \text{ //TODO hier fehlt was}$

$\mathcal H$ ... $c$-universelle Familie von Hashfunktionen.

$h: U \rightarrow \{0,..., m-1\}$

Sei $h \in \mathcal H$ zufällig gewählt.

##### Satz:

Für zufälliges $h \in \mathcal H$ gilt:

//TODO Formel

##### Beweis:

Definiere
$\delta_h (x,y) = \left\{
  \substack { 1 ~~~ \text{falls } h(x)=h(y) \\
              0 ~~~ \text{sonst} ~~~~~~~~~~~~~~~}
\right.$

$c_s(h) = \displaystyle\sum_{ \{x,y\} \in \binom S 2 } \delta_h(x,y)$

$E \Big(c_S(h)\Big) ~= \displaystyle\sum_{ \{x,y\} \in \binom S 2 } E\Big(\delta_h(x,y)\Big) ~= \displaystyle\sum_{ \{x,y\} \in \binom S 2 } \underbrace {Pr \Big(\delta_h(x,y) = 1 \Big)}_{\substack{ \leq \tfrac c m \text{, da nur ein } \tfrac c m \\\text{ Anteil der Hashfunktion in } \\ \mathcal H \text{ für fixe } x, y \text{ auf den gleiche }\\ \text{ Hashtafeleintrag mappen darf }\\ \text{ (Definition von c-Universalität).} } } ~\leq \displaystyle\sum_{ \{x,y\} \in \binom S 2 } \tfrac c m ~=~ \binom n 2 \tfrac c m$

##### Korollar:

Für $m > c \binom n 2$ gibt es ein $h \in \mathcal H$ mit $h |_ S$ injektiv.

##### Beweis:

Durch Einsetzen folgt

$E\big(c_S(h)\big) < \binom n 2 \tfrac c {c \binom n 2} = 1$

> Da Erwartungswert < 1, muss mindestens eine Hashfunktion mit 0 Kollisionen existieren.

##### Probalistische Methode:

Also $E\big(c_S(h)\big) < 1$. Da $c_S(h)$ nur ganzzahlige Werte annimmt, muss es mindest ein $h \in \mathcal H$ geben mit $c_S(h) = 0$. Andernfalls wäre $E(c_S(h)) \geq 1$.

Diese Korollar ist leider nicht besonders konstruktiv, auf den ersten Blick bleibt einem nichts anderes übrig, als alle $h \in \mathcal H$ zu testen um ein $h$ mit $h |_ S$ ($h$ eingeschränkt auf $S$) injektiv zu finden.

Laufzeit dafür wäre $\mathcal O(|\mathcal H| \cdot n + m)$.

Bislang zwei Nachteile:

1. Platzbedarf $m \approx n ^2$, damit so eine Funktion überhaupt sicher exisitiert.

2. Die gewünschte Hashfunktion finden eher schwierig.

##### Korollar:

Falls $m > 2 c \binom n 2$, können wir in erwartet $\mathcal O(n+m)$ Zeit ein $h \in \mathcal H$ finden mit $h|_ S$ injektiv.

##### Beweis:

$E\big(c_S(h)\big) \leq \binom n 2 \cdot \tfrac 2 m \leq \tfrac 1 2$

Markovungleichung: $Pr(X \geq c) \leq \tfrac {E(X)} c$ für nicht ... X //TODO ... fehlt

=> $Pr\Big(c_S(h) \geq 1\Big) \leq \tfrac {\tfrac 1 2} 2 = \tfrac 1 2$ //TODO kann das stimmen?

=> $Pr\Big(c_S(h) = 0\Big) \geq \tfrac 1 2$

//TODO Text von bildet
wähle $h \in X$ zufällig
- falls $h|_ S$ injektiv $\rightarrow$ fertig
- sonst wiederhole

##### Intuitiv:
Für groß genuge Hashtafeln gibt es injektive $h$ für $S$ bzw. für noch ein wenig größere kann man solche auch effizient finden.

Leider ist diese Konstruktion nicht so gut, da sie eine Hashtafel der Größe ~ $n ^2$ voraussetzt.
Hätten jedoch gerne Platz $\mathcal O(n)$.

#### Zweistufiges Perfektes Hashing

//TODO Bild Zweistufiges Hashing

##### Korollar:
Falls $m > \tfrac {n - 1} 2 \cdot c$, dann existiert $h \in \mathcal H$ mit $c_S(h) \leq n$.

##### Beweis:
$E\big(c_S(h)\big) \leq \binom n 2 \tfrac c m < \tfrac 1 2 \tfrac {n(n-1)} {\tfrac {n-1} 2} = n$

Wieder nur Existenzbeweis, Konstruktion aber möglich durch leicht größere Hashtafel.

##### Korollar:
Falls $m > (n-1) c$, gilt für mindestens die Hälfte der $h \in \mathcal H : c_S(h) \leq n$

##### Beweis:
Wie oben.

=> Wir können in erwartet $\mathcal O(n+m)$ Zeit ein $h$ finden mit $s_S(h) \leq n$ und wir brauchen nur Hashtafel der Größe $\mathcal O(n)$. //TODO stimmt $s_S$?

---

Sei $B_i(h) = h | ^{-1}_ S (i) = \{ x \in S ~|~ h(x) = i\}$

Inhalt des Hasheimers, der alle $x \in S$ bekommt mit $h(x) = i$

$S_i(h) = | B_i(h)|$

Es gilt: $c_S(h) = \sum _ i  \binom {S_i (h)} 2$

Wir haben $h$ so gewählt, dass $c_S(h) \leq n$

=> es gilt auch $\sum_ i \binom {S_i (h)} 2 \leq n$

Für jeden Hasheimer $B_i(h)$ erzeugen wir eine Sekundärhashfunktion & -tafel der Größe:

$m_i > 2 \cdot c \binom {S_i(h)} 2$

welche den Inhalt von $B_i(h)$ injektiv in $T_i$ hasht.

Der Platzverbrauch als auch die notwendige Zeit zur Konstruktion ist $\mathcal O(2 \cdot c \cdot \binom {S_i(h)} 2)$

Die Größe der Sekundärhashtabellen (& deren Konstruktionszeit) ist:

$\sum_{i=0}^{m-1} 2\cdot c \cdot \binom {S_i(h)} 2 = 2 \cdot c \cdot \sum_{i=0}^{m-1} \binom {S_i(h)} 2 \leq 2 \cdot c \cdot n = \mathcal O(n)$

---

#### Algorithmus zur Konstruktion einer Zweistufigen perfekten Hashfunktion

#### Annahme:

$S \subseteq U$, $|S| = n$, können $c$-universelle Familie von Hashfunktionen $h: U \rightarrow [0,...,m-1]$ samplen.

1. Finde $h: U \rightarrow \{0,...,c \cdot (n-1)\}$ welches auf $S \leq n$ Kollisionen produziert [geht in erwartet $\mathcal O(n)$ Zeit].

2. Bestimme für $i = 0,...,c \cdot (n-1)$  
$B_i(h)=\{x\in S ~|~ h(x) = i\}$

3. Für jedes $i=0,..., c \cdot (n-1)$ mit $B_i(h) > 1$ finde
$h : U \rightarrow [0, ..., \binom {S_i(h)} 2 \cdot c -1]$ welches injektiv ist für $B_i(h)$.
Das geht in $\mathcal O\left(\binom {S_i(h)} 2\right)$ pro Eimer $i$

---

Zugriff auf ein $x \in S$

Wende $h$ auf $x$ an: $h(x) = i$ (falls $|B_i(h)| \leq 1$ => fertig)

Wende $h_i$ auf $x$ an: $h_i(x) \rightarrow \text{Hashtafeleintrag der } \leq \text{ 1 Elemente enthält}$

=> Zugriffszeit $\mathcal O(1)$
