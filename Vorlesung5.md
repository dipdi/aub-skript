---
## 5. Vorlesung

Bis jetzt: Randomisierte Algorithmen

Jetzt: Randomisierte Datenstruktur
Skiplisten - eine randomisierte Alternative zu (2, 4)-, AVL-, Rot-Schwarz-, ... Bäumen
---
### Skiplisten

Gegeben:

$S = {x_1, ..., x_n} \subseteq U$ aus einem total geordneten Universum.

Ziel:
Baue Datenstruktur für $S$, sodass man effiziernt
- für gegebenes $x \in U$ das maximale Element $x' \in S$ mit $x' \leq X$ bzw. das minimale Element $x'' \in S$ mit $x \geq X$
 (Suche / Lookup)
- Elemente hinzufügen kann
- Elemente löschen kann

Naiv:
Sortierte Liste:
- Suchzeit: ~$n$
- $O(1)$ falls Position bekannt, sonst $O(n)$ für Einfügen
- dito für Löschen
Platz: $O(n)$

Besser:
(2,3,4)-Baum:
- Platz: $O(n)$
- Suchzeit: $O(log~n)$
- Einfügen / Löschen: $O(log~n)$

C'T:
Skiplisten:

Beispiel:

%GRAFIK

Bei Einfügen eines Elements wird zufällig eine "Turmhöhe" $h$ gewürfelt mit $Pr(h = i) = 2^{-(i + 1)}$.

Verbinde jede Turmetage mit dem nächsten Turm, der auf gleicher Höhe rechts davon sichtbar ist.

Suche nach einem Element mit $x \in U$

```
$v :=$ "$- \infinity$" - Turm
$h := v.height$
while $h \geq 0$ do
  while (x > v.forward[h] -> key) do
    v = v.forward[h]
  od
  h := h - 1
od
return
```

Übung:
Erklären von Einfügen und Löschen zeigen, dass Platzverbrauch O(n) erwartet.

Zu zeigen: Erwartete Suchzeit nach einem Element $x \in U$ ist $O(log~n)$.

Standardbeweis:
Definiere Zufallsvariable $X_{i,k} = \{ \stackrel{1 falls suche nach x Turm i in Höhe k bes} {0 sonst}$
%TODO schön

Laufzeit = $\sum_{i=0}^{j-1} \sum_{h \geq 0} X_{i,k}$

falls x Rang j in der Schlüsselmenge hat.
%TODO Prüfen

Alternativer Beweis:
Lemma:
Die erwartete Höhe eines Turms über einem Element $X_i$ ist $O(n)$
Beweis:
Übung

Was ist die erwatete Maximalhöhe eines vorkommenden Turms?

- Mit hoher Wahrscheinlichkeit ist ein fixer Turm nicht höher als ~ log n. $Pr(h_i \geq k) = 2 ^-k$ => $Pr(h_i \geq 2 \cdot log~n) = \tfrac 1 {n^2}$

- Wahrscheinlichkeit, dass irgendein Turm Höhe \geq 2 log n hat, ist $Pr(h_1 \geq 2 log ~ n ~ \vee ~ h_2 \geq 2 log ~ n ~ \vee ~ ... ~ \vee ~ h_n \geq 2 log ~ n ) \leq n - \tfrac{1}{n^2} = \tfrac{1}{n}$ (wegen Unionband)

Also sind mit hoher Wahrscheinlichkeit ($\geq 1 - \tfrac 1 n$) alle Türme nicht größer als $2 \cdot log~n$.

Idee für Analyse:

Konstruiere Routine, die genau die selben Zellen (Turm + Etage) besucht wie Suchroutine, aber einfacher zu analysieren ist.

```
v := x
h := 0
while v \neq -\infinity \and h \neq h_max do
  if v.height > h then
    h := h + 1
  else
    v = v.backward[h]
  fi
od
```

Besucht offensichtlich gleiche Zellen wie die Suche.

Beobachtung:
Wenn man als Männchen, der dem obigen Algorithmus ausführen muss, in einer Zelle eines Turms sitzt, dann muss man mit einer Wahrscheinlichkeit 1/2 ein Stockwerk hoch gehen und mit einer Wahrscheinlichkeit von 1/2 einen Turm nach links.

=> Erwartete Anzahl Links-Schritte = erwartete Anzahl Hoch-Schritte
=> Erwartete Links-Schritte = $O(log~n)$
=> Gesamgtlaufzeit erwartet $O(log~n)$

### Das Wörterbuchproblem (Hashing)

Gegeben: Ein Universum $U$ (sehr groß) und eine Teilmenge $S \subseteq U$ (eher klein) sowie ein $m \in \mathbb{N}$ ist das Ziel die Bestimmung einer Funktion
$h : U -> \{0,1,...,m-1\}$
sodass
$||$
%TODO Foto

Beispiel:
1) Telefonbuch
Aufgabe: Für gegebenen Namen und Vornamen die entsprechende Telefonnummer finden.

$|U| =$ sehr groß (Anzahl aller Nachnamen und Vornamen)
$|S| = 500 000$ (Anzahl der Nachnamen und Vornamen in Stuttgart)
Ideal: $m = 500 000$
Dann speichere 500 000 Telefonnummern in Array sodass $h$ für gegebenen Nachnamen und Vornamen genau den Index im Array liefert, wo ensprechende Telefonnummer steht.

Falls $h()$ in $O(n)$ ausgewertet werden kann, hat man $O(n)$ Zugriff auf die Information.
