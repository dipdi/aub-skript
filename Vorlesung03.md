---

## 3. Vorlesung

//TODO Anfang fehlt

##### Repeatet MinCut
Zeige, dass WS des Kargers MinCut Alg. das richtige Ergebnis berechnet, mindestens $\tfrac{1}{n^2}$ ist.

Das klingt nicht besonders gut, aber wir können die Erfolgswahrscheinlichkeit beliebig erhöhen durch Wiederholung.

**RepMinCut:**
bestVal = $\infty$; bestSol = $\emptyset$  
**for** i=1 to r **do**  
p = KargerMinCut(G)  
**if** (val(\(p\)) < bestVal)  
bestVal = val(\(p\))  
bestSol = \(p\)  
**endif**  
**rof**  
return bestSol  

Angenommen KargersMinCut berechnet MinCut mit WS $\geq \tfrac{2}{n(n-1)}$.

Die WS, dass in jedem der $r$ Versuche nicht der MinCut gefunden wird ist:

$(1- \tfrac{2}{n(n-1)})^r < (e^{- \tfrac{2}{n(n-1)}})^r$
> weil $<$ bedeutet:
$1 \cdot x < e^{-x} = e^{- \tfrac{rr}{n(n-1)}}$

Obere Schranke für WS, dass sie jedes Mal Pech haben z.B. $r= \tfrac{n(n-1)}{2}$
$\Rightarrow$ WS, dass falsch $1\leq \tfrac{1}{e} \approx 0,36$
oder auch für $r \approx n^2$ log $n$
$\Rightarrow$ Fehler WS $\leq \tfrac{1}{n^e}$

##### Jetzt: Beweisen, dass Erfolgs WS in der Tat $\geq \tfrac{2}{n(n-1)}$
Wir //TODO Wort fehlt hierzu einen MinCut und zeigen, dass die WS nie eine Kante des MinCuts zu kontrahieren $\geq \tfrac{2}{n(n-1)}$ ist.

**Lemma:** Betrachte einen Multigraph $G(V,E)$ mit einem MinCut mit Wert k.
Dann gilt: $G$ hat mindestens $\tfrac{k\cdot n}{2}$ Kanten.
_Beweis:_ Beobachtung: Jeder Knoten hat mindestens Grad k.

Sei $\varepsilon$ Ereignis, dass in i-tem Kontraktionsschritt keine MinCut-Kante erwischt.

Wir betrachten einen fixen MinCut mit Wert k (bzw. dessen k Kanten).
Die WS im ersten Kontraktionsschritt eine MinCut-Kante zu erwischen ist:

$\tfrac{k}{m} \leq \tfrac{2}{n} \Rightarrow Pr(\varepsilon_1) \geq 1 - \tfrac{2}{n}$
> die erste ungleichung gilt weil: $m \geq \tfrac{k \cdot n}{2}$

Falls $\varepsilon_1$ eingetreten ist, existieren vor dem zweiten Schritt mindestens $\tfrac{k \cdot (n-1)}{2}$ Kanten.

Die Ws im zweiten Schritt eine MinCut-Kante zu erwischen (unter der Vorraussetzung, dass $\varepsilon_1$ eingetreten) ist:

$\leq \tfrac{k}{\tfrac{k(n-1)}{2}}=\tfrac{2}{n-1}$

$\Rightarrow Pr(\varepsilon_1|\varepsilon_2) \geq 1 -\tfrac{2}{n-1}$

Falls $\varepsilon_1$ und $\varepsilon_2$ eingetreten sind, existieren vor dem dritten Schritt mindestens $\tfrac{k(n-2)}{2}$ Kanten.
$\Rightarrow$ WS, im dritten _%%TODO (   -  "   -  )_ (unter der Vorraussetzung, dass $\varepsilon_1$ & $\varepsilon_2$ eingetreten sind) ist:

$\leq \tfrac{k}{\tfrac{k(n-2)}{2}}=\tfrac{2}{n-2}$

$\Rightarrow Pr(\varepsilon_3|\varepsilon_1\cap\varepsilon_2) \geq 1 -\tfrac{2}{n-2}$

**Allgemein:**
Vor dem i-ten Schritt haben wir noch $n-i+1$ Kanten.
Falls wir bisher noch keinen Fehler gemacht haben ($\varepsilon,...,\varepsilon_{i-1}$ sind eingetreten) hat der MinCut noch Größe k $\Rightarrow$ wir haben noch $\geq k(n-i+1)$ Kanten
$\Rightarrow Pr(\varepsilon_i|\cap^{i-1}_{j=1}\varepsilon_j) \geq 1 -\tfrac{2}{n-i+1}$

Uns interessiert $Pr(\cap^{n-2}_{j=1}\varepsilon_j)$

$=Pr(\varepsilon_1)\cdot Pr(\varepsilon_2|\varepsilon_1)\cdot Pr(\varepsilon_3|\varepsilon_1\cap\varepsilon_2) \cdot Pr(\varepsilon_4|...)$  $\leq \prod^{n-2}_{i=1}(1-\tfrac{2}{n-i+1})$ $= \prod^{n-2}_{i=1} \tfrac{n-i+1-2}{n-1+1}$ $= \prod^{n-2}_{i=1} \tfrac{n-i-1}{n-1+1}$ $=\tfrac{n-2}{n}\cdot\tfrac{n-3}{n-1}\cdot\tfrac{n-4}{n-2}\cdot\tfrac{n-5}{n-3}\cdot\tfrac{n-6}{n-4}\cdot...\cdot\tfrac{2}{4}\cdot\tfrac{1}{3}$ $=\tfrac{2}{n(n-1)}$
$\Rightarrow Pr($ Algorithmus berechnet wirklich MinCut $) \geq \tfrac{2}{n{n-1}}$

$k = 2$

=> Wahrscheinlichkeit dass wir mehr als $log_2 n$ mal werfen müssen, bis Kopf kommt ist $\leq {\tfrac 1 2} ^{log_2 n} = \tfrac 1 n$
