---
# bibliography: references.bib

abstract: ""

execute: 
  echo: false
---

# Boole'sche Operationen {#sec-chapter-booleans}

::: {.callout-warning}
## Work in Progress
:::

In R stehen die logischen Operationen als *binäre* Operatoren zur Verfügung, bzw. als Funktionen mit genau zwei Parametern. Diese logischen Operatoren sind vektorisiert. Es ist deshalb unnötig, logische Ausdrücke durch die Boole'sche Arithmetik zu ersetzen. Lediglich die Reihenfolge der Ausführung dieser Opeartoren folgt der gleichen Regel wie die Arithmetik. 

::: {.callout-note}
## Merke
* Das logische Und entspricht der Multiplikation.
* Das logische Oder entspricht der Addition.
:::

Daraus folgt, dass immer zuerst das logische Und und erst danach das logische Oder ausgewertet wird. Dieser Regel folgt auch R. 

Die @tbl-logische-operatoren stellt die logischen Operationen und die verschiedenen Schreibweisen gegenüber.

| Operation | neutrales Element |  Mathematisch |  R |  arithmetische Operation |
| :--- | :--- | :--- | :---: | :---: |
| Nicht | - | $$ \lnot $$ |  `!`  | $1 - a$ | 
| Und | WAHR |  $$ \land $$ | `&` |  $a \cdot b$ | 
| Oder  | FALSCH | $$ \lor $$ |  `|`  | $a + b$ |
| Exklusiv-Oder/Antivalenz  |  - | $$ \oplus $$ | `xor() `  |  (a - b)^2 |

: Die wichtigsten logischen Operatoren und ihre Entsprechung in R {#tbl-logische-operatoren}

::: {.callout-warning}
## Achtung
Es gibt neben den beiden Operatoren `&` und `|` auch die gedoppelte Varianten `&&` und `||`. Diese Varianten arbeiten auf den Binärwerten von Ganzzahlen und werden normalerweise **nicht** im Zusammenhang mit logischen Ausdrücken verwendet. 
::: 

Das **Logisches Nicht** wird in R durch den Nicht-Operator (`!`) ausgedrückt. Dieser Operator wird auf jeden Wert eines Vektors einzeln angewandt. 

::: {#exm-logisches-nicht}
## Logisches Nicht

```R
logischer_vektor = c(TRUE, FALSE, FALSE, TRUE, TRUE)

! logischer_vektor 
# ergibt c(FALSE, TRUE, TRUE, FALSE, FALSE)
```
:::

R wandelt numerische Werte automatisch in Wahrheitswerte um, wenn sie mit logischen Operationen verwendet werden. Dabei gilt: 

* `FALSE` entspricht `0`
* `TRUE` entspricht *ungleich* `0`

::: {#exm-autoconvert-not}
```r
! c(1, 2, 0, 4, 0) 
# ergibt c(FALSE FALSE  TRUE FALSE  TRUE)
```
:::

Wenn Sie in R zwei Vektoren mit dem Und- (`&`), dem Oder-Operator (`|`) oder der  Antivalenz (`xor()`) verknüpfen, dann werden die Werte **immer** *paarweise* miteinander verglichen. Ein einzelner Vektor kann nicht an die Funktion des jeweiligen Operators übergeben werden. 

**Beispiel paarweise Verknüpfung**

```R
vektor_a = c(TRUE, FALSE, FALSE, TRUE, TRUE)
vektor_b = c(TRUE,  TRUE, FALSE, FALSE, TRUE)

vektor_a & vektor_b 
# ergibt c(TRUE, FALSE, FALSE, FALSE, TRUE)
```
## Logische Aggregationen mit `reduce()`

::: {.callout-note}
## Merke
Um logische Vektoren in **R** zu aggregieren, muss der Vektor **reduziert** (engl. *reduce*) werden. Das *Reduzieren* ist eine besondere *Aggregation* über eine Reihe von Werten, bei der jeder Wert gemeinsam mit dem Ergebnis der Vorgängerwerte an eine Funktion übergeben wird.
:::

::: {#exm-logical-reduce}
## Aggregation logischer Vektoren

```r
beispielWerte = c(TRUE, TRUE, FALSE, TRUE)

beispielWerte %>% reduce(`&`)   # Ergibt FALSE
beispielWerte %>% reduce(`|`)   # Ergibt TRUE
beispielWerte %>% reduce(`xor`) # Ergibt TRUE
```
:::

::: {.callout-important}
Beim Reduzieren muss beachtet werden, dass eine Funktion und nicht den Operator übergeben wird. Deshalb muss der jeweilige logische Operator in Backticks (`` ` ``) gesetzt und so als Funktionsbezeichner markiert werden. 
:::

## Vergleiche 

Neben den logischen Operationen sind Vergleiche ein wichtiges Konzept, das wir in logischen Ausdrücken regelmässig anwenden. 

Es gibt genau sechs (6) Vergleichsoperatoren:

* Gleich (`==`)
* Ungleich (`!=`)
* Grösser als (`>`)
* Grösser gleich (`>=`)
* Kleiner als (`<`)
* Kleiner gleich (`<=`)

::: {.callout-warning}
Vergleiche erfordern, dass beide Werte vom gleichen Datentyp sind.
:::

Die Vergleiche funktionieren für alle fundamentalen Datentypen.

Bei Zeichenketten wertet R die alphabetische Reihenfolge der Symbole vom Beginn einer Zeichenkette aus, um grösser oder kleiner Vergleiche durchzuführen.

### Die Existenz eines Werts in einem Vektor überprüfen

Häufig müssen Sie überprüfen, ob ein Wert in einer Liste vorkommt. Grundsätzlich können Sie das mit komplizierten logischen Verknüpfungen in der Art von @exm-check-without-in schreiben.

::: {#exm-check-without-in}
## Existstenzprüfung ohne `%in%`
```r
meinWert = 3
wertVektor = c(8, 2, 3)

meinWert == wertVektor[1] | meinWert == wertVektor[2] | meinWert == wertVektor[3]
```
:::

Einfacher ist aber ein sogenannter *Existenztest*. Dabei wird überprüft, ob ein Wert in einem Vektor vorkommt. Ein solcher Test lässt sich wie in @exm-check-with-in schreiben: 

::: {#exm-check-with-in}
## Existstenzprüfung mit `%in%`
```r
meinWert = 3
wertVektor = c(8, 2, 3)

meinWert %in% wertVektor
```
:::

Entsprechend der Definition des Existenzvergleichs $\in$ funktioniert R's `%in%`-Operator auch für Vektoren als linker Operand.
