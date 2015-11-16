# Algorithmen und Berechenbarkeit Skript
Algorithmen und Berechenbarkeit WS 2015, Universität Stuttgart
bei Stefan Funke

## Contribute
Bitte schreibt selbst mit oder ergänzt unseres. An vielen Stellen findet sich ein //TODO.  
Dazu könnt ihr die Issues auf Github verwenden oder sogar dieses Repo forken und einen Pull-Request machen.  
Das hier ist kein Mitschreibeservice. Keiner kann dafür garantieren, dass dieses Skript korrekt ist.

## How-To
Wir verwenden Markdown und Mathjax.
Markdown ist schön kurz und Mathjax mächtig genug um auch alle Formeln zu erschlagen.
### Markdown
[Wikipedia](https://de.wikipedia.org/wiki/Markdown)  
Markdown lässt sich zum Beispiel mit [Atom](https://github.com/atom/atom) in HTML konvertieren, die meisten Ausgaben orientieren sich an Bootstrap.  
In diesem Repository soll aber kein HTML Code zu finden sein. Jeder kann die Konvertierung für sich selbst machen.

Hier gibts ein nützliches [Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
### Mathjax
ist eine JavaScript Engine zum Rendern von mathematischen Ausdrücken, die z.B in [Tex](https://de.wikipedia.org/wiki/TeX) geschrieben sind. Im Prinzip lässt sich der Mathecode zwischen `$$..$$` in Mathjax verwenden. Folgende Funktionen werden [unterstützt](http://docs.mathjax.org/en/latest/tex.html#supported-latex-commands).  
Mathjax kann noch weitaus mehr als nur Tex interpretieren, wir denken aber, dass viele Tex-Erfahrung haben. Deshalb haben wir uns für diese Syntax entschieden. Der Vorteil ist auch die große Dokumentation im Internet z.B. auf [Wikipedia](https://de.wikipedia.org/wiki/Hilfe:TeX) oder die Suche bei [Stackexchange](tex.stackexchange.com).  
Es ist nicht möglich Erweiterungen wie TikZ zu verwenden.  
Um Mathjax zu rendern braucht es das [Plugin](https://atom.io/packages/markdown-preview-plus) Markdown-Preview-Plus in Atom.  

Verwendet `$...$` für Inline Math, `\[...\]` funktioniert wie Equation.
### Atom
Wir empfehlen Atom. Wer eine andere gute Lösung findet um Markdown und Mathjax zu verwenden teilt das gerne hier.  
Export zu pdf möglich mit pandoc  
Oder Export als HTML und mit dem Browser als pdf speichern. Dabei wird auch das gerenderte Mathjax richtig mitgenommen.

### Beispiel

```
**Berechnung von $\delta_{i+1}$:**

1. Lokalisiere $p_{i+1}$ im Gitter
2. inspiziere nur Punkte in benachbarten Gitterzellen von $p_{i+1}$ (und eigene Zelle)
3. falls neue CP-Distanz, setze $\delta_i$, entsprechend.;  ansonsten $\delta_{i+1} =\delta_i$

>	Lemma (Übung): In einer Gitterzelle liegen <= 4 Punkte.

```
