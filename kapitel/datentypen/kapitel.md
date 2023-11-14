---
# bibliography: references.bib

abstract: ""

execute: 
  echo: false
---
# Datentypen {#sec-chapter-datentypen}

R ist eine **vektororientierte Programmiersprache**. Das bedeutet zum einen, dass alle skalaren Werte von R wie ein-dimensionale Vektoren behandelt werden. Zum anderen ist die Syntax von R auf die Arbeit mit Vektoren optimiert, so dass sich entsprechende Aufgaben in R leichter ausdrücken lassen als in anderen (nicht-vektororientierten) Programmiersprachen.

Die Idee der Vektororientierung hat Auswirkungen auf die Datentypen, denn die fundamentalen Datentypen legen in R nur die *zulässigen Wertebereiche* fest. Die Werte selbst werden von R immer als Datenstruktur behandelt, auch wenn diese nicht explizit als solche gekennzeichnet wurden. Bei der praktischen Arbeit tritt diese Besonderheit meist nicht in den Vordergrund und wird in der R-Syntax nicht hervorgehoben. Die entsprechenden Sonderfälle werden im @sec-chapter-vektoroperationen behandelt.

## Fundamentale Datentypen

### Undefinierte Werte

R kennt zwei voneinander verschiedene Werte für undefinierte Werte. 

- `NULL` 
- `NA`

Weder `NULL` noch `NA` sind in R gleichwertig mit dem Wert `0`. Die beiden Werte sind ausserdem nicht gleich und haben eine leicht unterschiedliche Bedeutung.

`NULL` bedeutet, dass kein Wert vorhanden ist *und* kein Datentyp bekannt ist. Dieses Symbol ist für die Programmierung von Bedeutung und zeigt für eine Variable (@sec-chapter-variablen) an, dass diese auf keinen Wert im Speicher des Computers verweist. Das Symbol `NULL` ist ein eigener Datentyp.

`NA` (für *not available*) bedeutet, dass ein Wert fehlt, obwohl ein Wert erwartet wird. In diesem Fall ist der Datentyp bekannt. Dieser Wert bezieht sich auf die Daten und zeigt fehlende Werte an. Der Wert `NA` hat einen beliebigen Datentyp ausser `NULL`. `NA` ist ein Platzhalter für fehlende Werte in Daten, der immer ausserhalb des gültigen Wertebereichs der Daten liegt. Damit müssen fehlende Werte in R nicht durch einen alternativen Wert ersetzt werden. 

Beim Zählen von Werten werden `NA`-Werte mitgezählt. Für mathematische Operationen, wie der Addition oder der Multiplikation, führt ein Operand mit Wert `NA` als Operand immer zum Ergebnis `NA`. Deshalb müssen `NA` Werte vor allen anderen Operationen behandelt werden. 

### Zahlen

R unterscheidet drei Arten von Zahlen:

- Gleitkomma (`numeric()`)
- Ganzzahlen (`integer()`)
- Komplexe Zahlen (`complex()`)

Standardmässig werden alle Zahlenwerte als **Gleichkommazahlen** erstellt. Gleitkommazahlen können sowohl direkt oder in wissenschaftlicher Notation eingegeben werden. Bei der wissenschaftlichen Notation **muss** immer der Nachkommaanteil angegeben werden.

::: {#exm-zahlen-eingabe}
## Zahlenbeispiele in R 
```r
5
5.374
3.92e+2
1.0e-5
```
:::

Das kann mit der Funktion `is.numeric()` überprüft werden. Diese Funktion liefert Wahr zurück, wenn ein Wert eine Gleitkommazahl ist oder als solche behandelt werden kann. 

Die Werte anderer Datentypen lassen sich mit der Funktion `as.numeric()` in Zahlen umwandeln. Lässt sich ein Wert nicht eine Zahl umwandeln, dann wird der Wert `NA` mit einer entsprechenden Warnung erzeugt. 

Gelegentlich müssen **ganzzahlige Werte** sichergestellt werden. Diese Werte werden aus Gleitkommazahlen mit der Funktion `as.integer()` erzeugt. Wird eine Gleitkommazahl in eine Ganzzahl umgewandelt, dann wird nur der ganzzahlige Teil behalten. Der Nachkommateil wird gestrichen und nicht gerundet. Grundsätzlich sind alle Längen und alle Indizes in R automatisch Ganzzahlen und müssen nicht umgewandelt werden.

**Komplexe Zahlen** sind spezielle Zahlen, die über dem Zahlenraum der reellen bzw. Gleitkommazahlen hinausgehen. Eine Komplexe Zahl besteht aus einem reellen und einem *imaginären* Zahlenanteil. Der imaginäre Zahlenanteil ist als ein Vielfaches der Zahl $i = \sqrt{-1}$ definiert. In R wird diese Beziehung der beiden Teile als Summe dargestellt.

::: {#exm-komplexe-zahl-summe}
## Darstellung einer komplexen Zahl
```r
3+7i
```
:::

Alternativ können komplexe Zahlen mit der Funktion `complex()` erzeugt werden. Dazu werden bei beiden Zahlenanteile getrennt über die Parameter `real` und `imaginary` angegeben, wobei beim imaginären Teil das nachfolgende `i` durch die Funktion eingefügt wird.

::: {#exm-komplexe-zahl-summe}
## Erzeugen eines Vektors von komplexen Zahlen mit `complex()`
```r
complex(real = c(2,8), imaginary = c(3, 7))
```

```
[1] 2+3i 8+7i
```
:::

Für reelle Zahlen gilt, dass der imaginäre Anteil gleich 0 ist. Mit der Funktion `as.complex()` lässt sich jede Zahl in eine komplexe Zahl umwandeln. Wird eine so erstelle komplexe Zahl mit der ursprünglichen reellen Zahl verglichen, dann zeigt R korrekt an, dass die beiden Werte gleich sind. 

::: {.callout-tip}
## Praxis
In der Praxis wird die Umwandlung von reellen in komplexe Zahlen R überlassen. 
:::

::: {#exm-komplexe-zahl-summe}
## Umwandeln einer reelen Zahl in eine komplexe Zahl
```r
as.complex(2.2)
```

```
2.2+0i
```
:::

Neben den klassischen Zahlen verfügt R über das Konzept **Unendlich**, das in vielen Programmiersprachen fehlt. Ein unendlicher Wert wird durch das Symbol `Inf` dargestellt. Weil sowohl positive als auch negative unendliche Werte existieren steht `Inf` für *positiv unendlich* und `-Inf` für *negativ unendlich*.

### Zeichenketten

Zeichenketten heissen in R *Character-Strings* (`character()`). Zeichenketten müssen immer in Anführungszeichen eingefasst werden. Als Anführungszeichen dürfen das einfache Anführungszeichen (`'`) oder das doppelte Anführungszeichen (`"`) verwendet werden. 

::: {#exm-zeichenketten}
## Gleichwertige Zeichenketten mit einfachen und doppelten Anführungszeichen

```r
'Daten und Information'
"Daten und Information"
```
:::

::: {.callout-tip}
## Praxis
Die Lesbarkeit von R-Code wird dadurch verbessert, dass konsequent nur ein Symbol für die Kennzeichnung von Zeichenketten verwendet wird. Die Wahl welches der beiden Zeichen benutzt wird, wird von persönlichen Vorlieben geleitet. In diesem Buch wird konsequent das doppelte Anführungszeichen (`"`) eingesetzt, weil dadurch leere Zeichenketten direkt als solche erkennbar sind und dieses Symbol auch in anderen Programmiersprachen und Dateiformaten zur Kennzeichnung von Zeichenketten benutzt wird.
:::

::: {#exm-leere-zeichenketten}
## Leere Zeichenketten mit einfachen und doppelten Anführungszeichen

```r
''
""
```
:::

Um zu überprüfen, ob ein Wert eine Zeichenkette ist, wird die Funktion `is.character()` verwendet. Diese Funktion ergibt Wahr, wenn der Wert eine Zeichenkette ist und in allen anderen Fällen Falsch.

Um einen anderen Wert in eine Zeichenkette umzuwandeln bzw. zu *serialisieren*, wird die Funktion `as.character()` eingesetzt.

::: {#exm-zahlalszeichenkette}
## Eine Zahl in eine Zeichenkette serialisieren
```r
as.character(123.4)
# ergibt "123.4"
```
:::

### Wahrheitswerte

R kennt die beiden Wahrheitswerte *Wahr* und *Falsch* bzw. die englischen Begriffe **True** und **False**. In R müssen Wahrheitswerte ( `logical()`) immer in Grossbuchstaben geschrieben werden. Der Wert *Wahr* wird also `TRUE` und der Wert *Falsch* wird `FALSE` geschrieben. 

Beide Wahrheitswerte dürfen mit dem Anfangsbuchstaben abgekürzt werden. Auch dieser Buchstabe muss gross geschrieben werden. Die Werte `T` und `TRUE` sowie `F` und `FALSE` sind deshalb immer gleich.

Die Werte `TRUE` und `FALSE` dürfen nicht in Anführungszeichen eingefasst werden, denn sonst werden sie als Zeichenketten und nicht als Wahrheitswerte interpretiert. 

Normalerweise müssen Wahrheitswerte nicht mit `is.logical()` geprüft oder mit `as.logical()` aus anderen Datentypen erzeugt werden.

### Faktoren

Ein besonderer Datentyp von R sind **Faktoren** (`factor()`). 

::: {#def-faktor-r}
Ein **Faktor** ist ein *diskreter Datentyp* mit einem *festen Wertebereich* mit einer ganzzahligen Ordnung.
::: 

Die angezeigten Werte eines Faktors können als Zahlen, Zeichenketten oder Wahrheitswerte dargestellt werden. Jeder Faktor hat einen definierten Wertebereich, wobei die Werte dieses Wertebereichs diskret sind und eine ganzzahlige Ordnung haben. Mit Faktoren können nominal- und ordinalskalierte Datentypen abgebildet werden. Die Ordnung eines Faktor wird für die visuelle Darstellung der Werte verwendet und für Vergleiche von Werten berücksichtigt. 

Ein Faktor ist in R ein eigener fundamentaler Datentyp und kann nicht als Datentyp der dargestellten Werte verwendet werden.

Die *Ordnungswerte* eines Faktors lassen sich in Ganzzahlen ausgeben und entsprechend weiter verarbeiten. In diesem Fall muss der Faktor mit der Funktion `as.integer()` in Zahlen umgewandelt werden.

Der *Wertebereich* eines Faktors kann mit der Funktion `levels()` abgefragt werden. 

Der Wertebereich eines Faktors ist standardmässig Alphanumerisch geordnet. Für ordinalskalierte Datentypen kann diese Reihung angepasst werden (s. @sec-chapter-faktoren). 

::: {.callout-warning}
Der tatsächliche Wertebereich eines Faktors umfasst immer nur die vorhandenen Werte in den Daten. Die nicht vorkommenden Werte werden von R aus dem Wertebereich eines Faktors entfernt.
:::

## Datenstrukturen

Die zentralen Datenstrukturen von R sind Vektoren, Listen, Matrizen und Data-Frames. 

### Vektoren

Vektoren sind Datenstrukturen, bei denen alle Elemente vom gleichen Datentyp sind. Alle Werte mit fundamentalen Datentyp sind in R grundsätzlich Vektoren mit einem Element. Diese Eigenschaft mit der Funktion `is.vektor()` überprüft werden. Diese FUnktion hat als Ergebnis Wahr, wenn die Eingabe ein Vektor ist und in allen anderen Fällen Falsch. Wird der Funktion ein Wert übergeben, dann gibt die Funktion Wahr (bzw. `TRUE`) zurück. 

::: {#exm-isvector}
## Ein Wert ist ein Vektor
```r
is.vector(1)
# ergibt TRUE
```
:::

Um Werte zu längeren Vektoren zu Verknüpfen wird die Verbindenfunktion `c()` benutzt. Diese Funktion verbindet Vektoren so dass die Werte der Eingabevektoren im Ergebnisvektore nacheinander in der Reihenfolge der Eingabe vorliegen. 

::: {#exm-vektor-creation}
## Erzeugen eines Vektors aus einzelnen Werten
```r
c(1, 2, 3)
# ergibt {1, 2, 3}
```
:::

Die `c()`-Funktion hat immer einen Vektor als Ergebnis. Werden Vektoren als Eingabe verwendet, dann werden die Vektoren zu *einem* neuen Vektor zusammengefügt. 

::: {#exm-vektor-creation}
## Erzeugen eines Vektors aus mehreren Vektoren
```r
c(c(3, 4), c(1, 2), c(5, 6))
# ergibt {3, 4, 1, 2, 5, 6}
```
:::

Versucht man Vektoren aus Werten mit unterschiedlichen Datentypen zu erstellen, dann werden alle Werte in den allgemeinsten auftredenten Datentyp umgewandelt. Dabei gilt die Reihenfolge vom allgemeinsten Datentyp zum speziellsten Datentyp: Zeichenkette, Zahl, Wahrheitswert. 

::: {#exm-vektor-creation}
## Erzeugen eines Vektors mit unerschiedlichen Datentypen
```r
c(1, TRUE, "Daten und Information")
# ergibt {"1", "TRUE", "Daten und Information"}

c(1, TRUE, 2 FALSE)
# ergibt {1, 1, 2, 0}
```
:::

Alle Vektoren haben eine Länge, die mit der Funktion `length()` ermittelt wird.

::: {#exm-vector-length}
## Länge eines Vektor bestimmen
```r
length(c(1,3,7))
# ergibt 3
```
:::

Damit einzelne Werte eines Vektors angesprochen werden können, müssen eckige Klammern (`[]`) verwendet werden. 

::: {#exm-vektor-index}
## Ein Vektorelement über dessen Index ansprechen

```r
c(1, 7, 13) -> vektor123

vektor123[2] # ergibt 7
```
:::

::: {.callout-tip}
## Praxis

In der Regel werden die einzelnen Elemente von Vektoren nicht über den Index angesprochen. Im Gegensatz zu anderen Programmiersprachen hat die Verwendung des Vektorindex in R keine grosse Bedeutung.
:::

Vektoren sind eingeschränkt auf *eine* Dimension. Um komplexere Datenstrukturen zu erhalten, müssen weitere Datenstrukturen hinzugezogen werden.

### Listen

Listen ähneln Vektoren in vielen Punkten. Der zentrale Unterschied zwischen Listen und Vektoren ist, dass die Elemente von Listen einen *beliebigen Datentyp* haben dürfen. Listen werden in R mit der Funktion `list()` erzeugt.

::: {#exm-list-creation}
## Erzeugen einer Liste aus einzelnen Werten
```r
list(1, TRUE, "Daten und Information")
# erzeugt {1, TRUE, "Daten und Information"}
```
::: 

Die Funktion `is.list()` überprüft, ob ein Wert eine Liste ist. Die Funktion `length()` liefert die Anzahl der Elemente in einer Liste. 

Im Vergleich zur Funktion `c()` fügt die Funktion `list()` Listen nicht zusammen, sondern behandelt alle Eingabewerte als eigene Elemente. D.h. der Datentyp jedes Werts bleibt erhalten, wenn dieser einer Liste hinzugefügt wird. Diese Eigenschaft lässt sich ausnutzen, um geschachtelte Datenstrukturen zu erzeugen. @exm-geschachtelte-liste zeigt die Erstellung einer geschachtelten Liste mit zwei unterschiedlich langen Vektoren.

::: {#exm-geschachtelte-liste}
## Geschachtelte Liste mit zwei Vektoren
```r
list(c(1, 2), c(3, 4, 5))
```
:::

Der Listen verwenden zur Indizierung doppelte eckige Klammern (`[[]]`). Nur so lassen sich die Werte der Liste korrekt referenzieren. 

::: {#exm-listenindex}
```r
list(1, TRUE, "Daten und Information") -> gemischteListe 

gemischteListe[[3]]
# ergibt "Daten und Information"
```
::: 

::: {.callout-warning}
Der Vektorindex kann auch für Listeneinträge verwendet werden. In diesem Fall wird eine Liste zurückgegeben, die nur die ausgewählten Listenelemente enthält. Das ist normalerweise nicht gewünscht.
:::

#### Benannte Listen

Listeneinträge können *Namen* haben. Diese können später zum Referenzieren der Listeneinträge verwendet werden. Auf diese Weise lassen sich objektartige Strukturen erzeugen. Benannte Einträge lassen sich über die Position des Werts oder dem Namen als Index ansprechen. Wird ein Name als Index verwendet, dann **muss** dieser als Zeichenkette angegeben werden. Häufiger findet sich jedoch die Dollar-Referenzierung in R-Skripten. Die Dollar-Referenzierung verwendet das Dollar-Zeichen (`$`) um auf einen Namen zuzugreifen. Damit diese Referenzierung funktioniert, **muss** der Name ein gültiges R-Symbol sein, dass nicht markiert werden muss. Alle anderen Namen müssen als Listenindex verwendet werden. @exm-benannte-liste zeigt die verschiedenen Zugriffsarten für benannte Listen.

::: {#exm-benannte-liste}
## Verwendung benannter Listen
```r
list(modul = "Daten und Information", 
     semester = 1) -> 
        benannteListe

benannteListe[[2]] # ergibt 1
benannteListe[["semester"]] # ergibt 1
benannteListe$semester # ergibt 1
```
::: 

::: {.callout-note}
In R müssen nicht alle Listeneinträge benannt sein. Es ist normal, dass sowohl benannte als auch unbenannte Listenelemente vorhanden sind. Unbenannte Listeneinträge können nur über ihre Position angesprochen werden.
::: 

### Matrizen

Als vektororientierte Sprache ist die Matrix ein wichtiges Konstrukt der Datenstrukturierung. Alle Werte einer Matrix müssen vom Datentyp Zahl sein. Man kann sich eine Matrix als Vektor von gleichlangen Vektoren vorstellen. Wegen der besonderen Eigenschaften von R-Vektoren ist diese Art der Schachtelung jedoch nicht möglich. 

Eine Matrix wird in R immer aus Vektoren erzeugt. Dafür gibt es vier Wege.

Eine Matrix wird aus einem Vektor über die Zeilenzahl $m$ erstellt. Dabei werden immer $m$ aufeinanderfolgende Werte eines Vektors in eine Spalte geschrieben.

::: {#exm-matrix-make-rows}
## eine m-Matrix aus einem Vektor erstellen
```r
matrix(c(1,2,3,4,5,6), nrow = 2)
```

```
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
```
:::

Eine Matrix wird aus einem Vektor über die Spaltenzahl $n$ erstellt. Dazu wird der Vektor in $n$ grosse Blöcke aufeinanderfolgender Werte gegliedert, die jeweils in eine Spalte geschrieben werden. 


::: {#exm-matrix-make-cols}
## eine n-Matrix aus einem Vektor erstellen
```r
matrix(c(1,2,3,4,5,6), ncol = 2)
```

```
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
```
:::

Eine Matrix wird über das Kreuzprodukt (@sec-chapter-matrix-operationen) aus zwei Vektoren erstellt.

::: {#exm-matrix-dt-cross}
```r
c(1, 2, 3) %*% t(c(3, 4, 5))
```

```
     [,1] [,2] [,3]
[1,]    3    4    5
[2,]    6    8   10
[3,]    9   12   15
```
:::

::: {.callout-warning}
Wird lässt sich der Ausgangsvektor nicht in die angegebene Zeilen- oder Spaltenzahl gliedern, dann werden die Vektorwerte solange wiederholt, bis die Matrix aufgefüllt wurde und eine eine Warnrmeldung ausgegeben. Dadurch ist das Ergebnis nicht immer klar nachvollziehbar.
::: 

Eine Matrix wird über das äussere Matrixprodukt (@sec-chapter-matrix-operationen) aus zwei Vektoren erstetllt. 

::: {#exm-matrix-dt-outer}
## eine -Matrix aus Vektoren erstellen
```r
c(1, 2, 3) %o% c(3, 4, 5)
```

```
     [,1] [,2] [,3]
[1,]    3    4    5
[2,]    6    8   10
[3,]    9   12   15
```
:::

::: {.callout-tip}
## Praxis

Das Kreuzprodukt sollte nicht zum Erzeugen von Matrizen verwendet, sondern ausschliesslich als mathematische Operation behandelt werden. Das äussere Produkt ist flexibler und einfacher anzuwenden.
::: 

Mit der Funktion `is.matrix()` kann überprüft werden, ob eine Datenstruktur eine Matrix ist. Erfüllt eine tabellarische Struktur die Kriterien für eine Matrix, dann kann diese Struktur mit der Funktion `as.matrix()` in eine Matrix umgewandelt werden. 

Die R-Matrix ähnelt der Struktur von Vektoren.

- Eine Matrix hat eine Länge. Diese Länge entspricht der Gesamtzahl der Elemente der Matrix. 
- Wird eine Matrix in der Funktion `c()` als Wert übergeben, dann wird die Matrix zuerst in einen Vektor umgewandelt.

Weil die Anzahl der Spalten und Zeilen nicht über die Länge bestimmt werden kann, muss zu diesem Zweck die Funktion `dim()` verwendet werden. `dim()` gibt die Anzahl der Zeilen und der Spalten in dieser Reihenfolge aus.

Die Werte in einer R-Matrix können über den Zeilen- und Spaltenindex abgefragt werden. Dazu werden wie bei den Vektoren einfache eckige Klammern verwendet. Der Zeilen- und Spaltenindex werden dabei durch ein Komma voneinander getrennt. 

::: {#exm-matrix-select}
## Einen Wert über den Matrix-Index zugreifen
```r
c(1, 2, 3) %o% c(3, 4, 5) -> matrix123
matrix123[2,2] # ergibt 8
```
:::

Wird beim Matrix-Index der Zeilen- oder der Spaltenindex weggelassen, wird eine ganze Zeile bzw. Spalte als Vektor ausgewählt.

::: {.callout-tip}
## Praxis
Ähnlich wie bei Vektoren, ist es in der Praxis nur sehr selten notwendig, auf die Werte einer Matrix zuzugreifen.
::: 

### Data-Frames

Ein Data-Frame ist eine *benannte geschachtelte Liste* mit *Vektoren gleicher Länge*. Diese Datenstruktur ist die Basis für Datentabellen. Anders als eine normale geschachtelte Liste stellt ein Date-Frame zusätzlich sicher, dass alle Vektoren *immer* die gleiche Länge haben.

Data-Frames werden normalerweise mit den Funktionen `tibble()` oder `tribble()` erstellt. Diese Funktionen sind nur in **tidy R** verfügbar. Diese Funktionen erzeugen eine effizienteren und damit schnelle Version eines Data-Frames als Base R. 

::: {.callout-tip}
## Praxis
Data-Frames werden normalerweise beim Einlesen der Daten mit der korrekten Datenstruktur automatisch erzeugt. Händisches Erstellen von Date-Frames ist dann nicht mehr notwendig.
:::

Weil Data-Frames spezielle Listen sind, können sie genau gleich wie Listen behandelt werden. Es lassen sich damit die gleichen Zugriffe, wie bei Listen umsetzen.

::: {.callout-note}
Vektoren in Data-Frames können Listen als Datentyp haben, was bei normalen Vektore nicht möglich ist. Diese besondere Eigenschaft wird im @sec-chapter-daten-formen ausgenutzt.
::: 

::: {.callout-tip}
## Praxis
Die Behandlung von Data-Frames als geschachtelte Listen ist inzwischen unüblich. Stattdessen sollten die effizienteren und leichter zu merkenden Transformationsschritte eingesetzt werden.
:::
