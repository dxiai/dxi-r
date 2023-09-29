---
# bibliography: references.bib

abstract: ""

execute: 
  echo: false
---
# Datentypen {#sec-chapter-datentypen}

::: {.callout-warning}
## Work in Progress
:::

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

- Ganzzahlen (`integer()`)
- nummeric / Gleitkomma (`numeric()`)
- Komplexe Zahlen (`complex()`)

Neben den klassischen Zahlen verfügt R über das Konzept *Unendlich*, das in vielen Programmiersprachen fehlt. Ein unendlicher Wert wird durch das Symbol `INF` dargestellt. Weil sowohl positive als auch negative unendliche Werte existieren steht `Inf` für *positiv unendlich* und `-Inf` für *negativ unendlich* 

### Zeichenketten (character)

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
## leere Zeichenketten mit einfachen und doppelten Anführungszeichen

```r
''
""
```
:::



### Wahrheitswerte

R kennt die beiden Wahrheitswerte *Wahr* und *Falsch* bzw. die englischen Begriffe **True** und **False**. In R müssen Wahrheitswerte ( `logical()`) immer in Grossbuchstaben geschrieben werden. Der Wert *Wahr* wird also `TRUE` und der Wert *Falsch* wird `FALSE` geschrieben. 

Beide Wahrheitswerte dürfen mit dem Anfangsbuchstaben abgekürzt werden. Auch dieser Buchstabe muss gross geschrieben werden. Die Werte `T` und `TRUE` sowie `F` und `FALSE` sind deshalb immer gleich.

Die Werte `TRUE` und `FALSE` dürfen nicht in Anführungszeichen eingefasst werden, denn sonst werden sie als Zeichenketten und nicht als Wahrheitswerte interpretiert. 

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

### Vektoren

### Listen

### Matrizen

### Data Frames
