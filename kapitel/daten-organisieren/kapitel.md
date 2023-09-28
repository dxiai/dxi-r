# Dokumentation {#sec-chapter-dokumentation}

Traditionell wurde die Auswertung von Daten und die Dokumentation in getrennten Arbeitsschritten durchgeführt. Dazu wurden während der Auswertung mussten alle notwendigen Materialien für Berichte und Publikationen erzeugt werden und anschliessend während der Dokumentation in ein anderes Dokument eingebettet werden. Diese Trennung der Arbeitsschritte erforderte von Autor:innen bei der Dokumentation grosse Sorgfalt, weil die analytische Vorgehensweise aus den erzeugten Materialien nicht mehr nachvollziehbar war und nicht erzeugte Materialien oft ohne konkrete Belege berichtet wurde. 

Mit zunehmender Bedeutung der Datenwissenschaften für industrielle Anwendungen, wurde das Bedürfnis nach Werkzeugen zur schnellen Erstellung von daten-basierten Berichten immer grösser, weil erstens regelmässige Berichte mit gleichen analytischen Methoden erstellt werden müssen und zweitens die aktuellen wissenschaftlichen Standards für jede Aussage in einem analytischen Bericht Belege in der Datenbasis erfordern. Dieses Bedürfnis ist nicht neu und einzelne Speziallösungen lassen sich bis in die Frühzeit der Digitalisierung zurückverfolgen. Jedoch wurden erst seit Mitte der 2010er-Jahre Werkzeuge zur datenbasierten Dokumentation entwickelt und systematisch eingesetzt. Diese Entwicklungen münden in den aktuell verwendeten *Datendokumenten*. 

::: {#def-datendokumente}
Ein **Datendokument** ist ein Dokument, dass Datentransformationen, -Visualisierungen und -Auswertungen in die Dokumentation einbindet. 
:::

Datendokumente eignen sich besonders für Labor- oder Projektberichte, weil die Auswertung direkt in den Bericht einfliesst. Datendokumente verbinden sog. beschreibenden Text mit Code-Fragmenten, so dass die Ausgabe der Code-Fragmente direkt Teil des Berichts wird.

Datendokumente verbinden beschreibende und formatierte Textblöcke mit Code-Blöcken. Die Formatierung der Textblöcke erfolgt in der Regel mithilfe von Markdown. Markdown hat gegenüber anderen Textformatierungen den Vorteil, dass es ähnlich wie Programm-Code leicht erlernbar und leicht versionierbar ist.

Datendokumente werden in der Regel als Web-Seiten oder als PDF-Dokumente bereitgestellt. Dieses Bereitstellen ist die **Präsentation** eines Datendokuments.

Bei Datendokumenten werden zwei leicht unterschiedliche Ansätze verfolgt.

- ***R-Markdown*** [@grolemund_introduction_2014] verwendet ausschliesslich Markdown als Dokumentenformat. Die Ergebnisse von Code-Blöcken sind nicht Bestandteil des eigentlichen Dokuments, sondern werden erst für die Präsentation erzeugt. Die Codezellen bilden gemeinsam ein Programm bzw. Script, dass immer in der Reihenfolge der Code-Zellen ausgeführt wird. Ein R-Markdown-Dokument ist immer ein gültiges Markdown Dokument und kann mit jedem Markdown-fähigen Betrachter (z.B. GitHub) angezeigt werden. 

- ***Jupyter Notebooks*** [@jupyter_development_team_notebook_2015] unterscheiden zwischen *Textzellen* und *Code-Zellen*. Textzellen enthalten nur in Markdown formatierte Textelemente. Werden Markdown-Code-Blöcke in  Textzellen verwendet, dann werden diese nicht ausgeführt. Code-Zellen enthalten nur ausführbaren Code. Wird dieser ausgeführt werden, dann werden die Ergebnisse als Teil des Dokuments gespeichert. Die Code-Zellen eines Jupyter-Notebooks können in beliebiger Reihenfolge ausgeführt werden. Deshalb wird zusätzlich für ausgeführte Code-Zellen eine Nummer festgehalten, aus der die Reihenfolge der Ausführung hervorgeht. Jupyter Notebooks verwenden ein spezielles Datenformat und benötigen spezielle Betrachter-Software. 

::: {.callout-note}
Der Begriff *R-Markdown* wird hier als Referenz für den verfolgten Ansatz verwendet. Datendokumente in R-Markdown sind nicht auf R beschränkt, sondern können beliebige andere Programmiersprachen in Code-Blöcken verwenden. 
:::

::: {.callout-note}
Weil *Jupyter Notebooks* die Ergebnisse von Code-Zellen im Dokument speichern, ändert sich das Dokument, selbst wenn keine inhaltlichen Änderungen vorgenommen wurden. Diese Eingenschaft von Jupyter Notebooks erschwert die Versionierung ein wenig. 
:::

Für Laborberichte können Datendokumente direkt verwendet werden. Für die Publikation von Projektberichten sind diese Systeme nicht unmittelbar geeignet, sondern müssen durch zusätzliche Tools ergänzt werden, um die zusätzlichen Elemente dieser Berichte erzeugen zu können. Dazu gehören Bild- und Tabellenbeschriftungen, Querverweise, Quellenverweise und das Literaturverzeichnis sowie Inhaltsverzeichnisse und Indizes.

R-Markdown-Dokumente können mit der Software `quarto` [@posit_software_pbc_quarto_2023] in ein geeignetes Präsentationsformat gebracht werden. 

Für *Jupyter Notebooks* übernimmt diese Funktion das Werkzeug `Jupyter Books` [@the_jupyter_book_community_jupyter_2023].

## Mathematische Formeln in Datendokumenten

In Datendokumenten lassen sich mathematische Formeln darstellen. Diese Formeln werden im sog. **LaTeX-Math-Mode** eingegeben. Diese Formeln werden bei der Präsentation in die korrekte mathematische Darstellung überführt. 

Der LaTeX-Math-Mode ist eine Formelbeschreibungssprache [@american_mathematical_society_users_2020; @hogholm_mathtools_2022], mit der die exotischen und in nicht mathematischen Texten wenig bis nie verwendeten Symbole und deren Anordnung gezielt erzeugt werden können. Der LaTeX-Math-Mode wird von vielen Systemen zur Darstellung von Formeln verwendet. Deshalb lohnt sich eine Auseinandersetzung mit den Grundkonzepten dieser Technik. 

Der LaTeX-Math-Mode kennt zwei Modi: den **inline Modus**, wenn eine Formel wie oben in den Fliesstext eingebettet ist, und den **Gleichungsmodus**, wenn eine Formel wie eine Abbildung hervorgehoben und beschriftet wird. Der inline Modus wird durch ein einfaches Dollar-Zeichen (`$`) oder mit der Zeichenfolge Backslash-runde Klammer (`\(` und `\)`) eingeleitet und abgeschlossen. Der Gleichungsmodus wird durch ein doppeltes Dollar-Zeichen  (`$$`) eingeleitet und abgeschlossen. Die Formelbeschreibung ist unabhängig vom Modus, wobei die Darstellung dem zur Verfügung stehenden Platz berücksichtigt. 

Mit dem LaTeX-Math-Mode wird die Formel mit ihren Bestandteilen und ihren Beziehungen beschrieben. Die folgenden Grundregeln sind für den Math-Mode wichtig: 

- Zahlen und Buchstaben und andere Sonderzeichen auf der Tastatur werden als solche dargestellt.
- Sonderzeichen, Operatoren und besondere Formatierungen werden durch einen Back-Slash (`\`) eingeleitet, der von einem Schlüsselwort gefolgt wird.
- zusammengehörende Teilausdrücke werden durch geschweifte Klammern zusammengefasst (`{}`). Teilausdrücke, die nur aus einem Symbolbestehen müssen nicht in Klammern gesetzt werden.
- Das Dach (`^`) bedeutet den folgenden Teilausdruck hochstellen.
- Der Unterstrich (`_`) bedeutet den folgenden Teilausdruck tiefstellen.

::: {#exm-math-mode}
## LaTeX-Math-Mode
Die Formel $\sum_{i=1}^{n}{(\frac{x_i}{2})^2}$ lässt sich mit einer normalen Tastatur nicht eingeben. Deshalb wird der Math-Mode zur Formelbeschreibung verwendet. Im inline Modus wird die Formeldarstellung so gewählt, dass die Formel ungefähr in die aktuelle Textzeile passt. Im Gleichungsmodus wird die gleiche Formel möglichst übersichtlich angezeigt.

$$
\sum_{i=1}^{n}{(\frac{x_i}{2})^2}
$$

Die Formel wird wie folgt im Math-Mode beschrieben:

```latex
\sum_{i=1}^{n}{(\frac{x_i}{2})^2}
```

Die Formel beginnt mit dem grossen griechischen Sigma ($\Sigma$) für das Summensymbol. Das wird durch `\sum` erzeugt. Eine Summe besteht aus drei Teilausdrücken: 

1. Dem Initialwert *unter* dem Summenzeichen.
2. Dem Endwert *über* dem Summenzeichen.
3. Dem Summenterm hinter dem Summenzeichen.

Entsprechend wird der Initialwert mit `_` tiefgestellt, der Endwert mit `^` hochgestellt und der Summenterm wird hinter die Summe gefügt.

Der Summenterm wird durch eine Potenz und einem Bruch gebildet. Die Potenz wird durch das Hochstellen gekennzeichnet. Die runden Klammern werden als einfache runde Klammern eingegeben 

Der Bruch ist eine besondere Darstellung und benötigt das Schlüsselwort `frac` für *fraction* (dt. Fraktur/Bruch). Ein Bruch besteht immer aus zwei Teilausdrücken, die nacheinander in geschweiften Klammern angegeben werden müssen. Der Zähler besteht aus dem Teilausdruck $x_i$. Entsprechend muss das i gegenüber dem x tiefgestellt werden. Weil es sich jeweils um einzelne Symbole handelt, müssen die Teilausdrücke nicht geklammert werden. 
:::
