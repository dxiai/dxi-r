# R-Sprachelemente {#sec-chapter-language}

Jede Programmiersprache besteht aus festgelegten *Symbolen* und *Operatoren*. Diese beiden Elemente bilden zusammen die **Syntax** einer Programmiersprache mit der *Operationen* erstellt werden können. Syntaktische Kern von R ist vergleichsweise kompakt und übersichtlich. 

## Syntaktische Symbole

Syntaktische **Symbole** sind alle Sprachelemente, die für sich allein stehen. Symbole können Werte, Schlüsselworte oder Bezeichner sein. Symbole werden durch Operatoren oder durch Leerzeichen voneinander getrennt.

### Werte

Werte können direkt angegeben werden. Dabei legt der Datentyp eines Werts (@sec-chapter-datentypen) fest, wie dieser eingegeben werden muss. 

- Zahlen werden als Ziffernfolge direkt eingegeben (z.B. `123.45`).
- Wahrheitswerte werden in Grossbuchstaben eingegeben (z.B. `WAHR`).
- Zeichenketten werden in Anführungszeichen eingegeben (z.B. `"Daten und Information"`)

### Schlüsselworte

R kennt verschiedene Schlüsselworte, die als eigene Symbole festgelegt sind und nicht verändert werden können. Jedes der folgenden Schlüsselworte hat eine Bedeutung für R und kann nur in dieser Bedeutung verwendet und nicht umdefiniert werden.

Funktionen (@sec-chapter-variablen)

- `function`

Entscheidungen (@sec-chapter-booleans) zur bedingten Ausführung von Operationen.

- `if`
- `else`

Schleifen zur wiederholten Ausführung von Operationen.

- `repeat`
- `while`
- `for`
- `next`
- `break`

::: {.callout-important}
## Keine Schleifen in der Paxis

R-Schleifen sind im Vergleich zu vergleichbaren Funktionen ***äusserst ineffizient***. Deshalb haben Schleifen in der praktischen Anwendung von R **keine Bedeutung** mehr. Stattdessen werden ausschliesslich spezielle Funktionen über Datenstrukturen verwendet. Entsprechend finden sich diese Schlüsselworte sehr selten in R-Skripten.
:::

### Bezeichner

Ist ein Symbol kein Schlüsselwort und kein Wert, dann wird das Symbol als **Bezeichner** behandelt. Ein Bezeichner ist ein Platzhalter für einen Wert (@sec-chapter-datentypen) und kann wie ein Wert in Operationen verwendet werden. 

Grundsätzlich können Bezeichner beliebige Zeichen enthalten. R erkennt einfache Bezeichner mit zwei Regeln. 

- Beginnt mit einem ASCII-Buchstaben (A-Z, a-z) oder einem Unterstrich.
- Enthält nur ASCII-Buchstaben (A-Z, a-z), arabische Ziffern (0-9) und Unterstriche.

Weicht ein Bezeichner von diesen Regeln ab, dann muss dieser als solcher durch Backticks (`` ` ``) gekennzeichnet werden. @exm-gekennzeichneter-name zeigt einen Bezeichner, der den deutschen Umlaut (`ü`) und ein Leerzeichen enthält. Weil `ü` kein ASCII-Buchstabe ist und das Leerzeichen normalerweise Symbole trennt, muss der ganze Bezeichner als Symbol markiert werden.

::: {#exm-gekennzeichneter-name}
## markierter Bezeichner

```r
`merkwürdiger Bezeichner`
```
::: 

Wird der Bezeichner aus @exm-gekennzeichneter-name nicht markiert, erzeugt R die Fehlermeldung `Fehler: unerwartetes Symbol in "merkwürdiger Bezeichner"`.

## Operationen

::: {#def-operation}
Eine **Operation** ist ein syntaktisches Konstrukt, das von einer Programmiersprache als ausgeführbar erkannt wird.
::: 

Eine Operation kann sich als ein Satz in einer Sprache vorgestellt werden, wobei für Programmiersprachen nur **ganze (vollständige) Sätze** gültig sind und ausgeführt werden. 

R fügt Symbole und Operatoren solange zusammen, bis eine **syntaktisch gültige** Operation gefunden wird. Syntaktisch gültig heisst in diesem Zusammenhang, dass alle syntaktischen Elemente für eine Operation gefunden wurden. Erst dann versucht R diese Operation auszuführen. Bei der Ausführung kann festgestellt werden, dass eine Operation nicht ausführbar ist. In diesem Fall erzeugt R eine **Fehlermeldung**.

Die einfachste R-Operation ist die Angabe eines einzelnen Symbols. Wird nur ein Symbol als Operation eingegeben, dann bedeutet das für R, dass die zum Symbol gehörenden Daten **serialisiert** werden sollen. Das bedeutet, dass die Daten des Symbols angezeigt werden. Ist das Symbol ein Wert, dann wird der Wert wiederholt.

::: {#exm-wertwiederholen}
## Wert direkt serialisieren
```r
"Daten und Information"
```
:::

@exm-wertwiederholen zeigt die Operation, die die Zeichenkette *Daten und Information* als Ergebnis serialisiert.

### Operatoren

::: {#def-operator}
Ein **Operator** ist ein syntaktisches Element, das Symbole zu einer Operation *verknüpft*. 
:::

Die bekanntesten Operatoren sind die arithmetischen Operatoren und die Vergleichsoperatoren.

In R sind vier spezielle Operatoren wichtig: 

- Die drei Zuweisungsoperatoren (`<-`, `=` und `->`).
- Der Funktionsaufrufoperator (`()`).

Mit den *Zuweisungsoperatoren* können Werte Namen zugewiesen werden. Bei den Pfeil-Operatoren wird der Wert in Richtung des Pfeils zugewiesen. Beim Gleich-Operator erfolgt die Zuweisung von rechts nach links. (s. @exm-zuweisungsoperatoren)

::: {#def-deklaration}
Eine **Deklaration** heisst die (erste) Zuweisung eines Werts an eine Variable.
:::

::: {#exm-zuweisungsoperatoren}
## Zuweisung einer Zeichenkette 
```r
buchtitel1 = "Daten und Information 1"
buchtitel2 <- "Daten und Information 2"
"Daten und Information 3" -> buchtitel3 
```
:::

Es können auch Namen anderen Namen zugewiesen werden. In diesem Fall wird der zugehörige Wert für einen Namen ermittelt und dem neuen Namen zugewiesen (@exm-zuweisung-namen).

::: {#exm-zuweisung-namen}
## Zuweisung von Namen an einen anderen Namen.
```r
buchtitel4 = buchttitel1 
# buchtitel4 enthält "Daten und Information 1"
buchtitel5 <- buchtitel2 
# buchtitel5 enthält "Daten und Information 2"
buchtitel3 -> buchtitel6 
# buchtitel6 enthält "Daten und Information 3"
```
:::

Der *Funktionsaufrufoperator* prüft ob der vorangehende Name eine Funktion ist und ruft diese auf. Zwischen den Klammern können Werte oder Namen als Parameter der Funktion übergeben werden. Die Parameter werden durch Kommas getrennt (@exm-funktionsaufruf).

::: {#exm-funktionsaufruf}
## Funktionsaufruf der Summefunktion mit Parametern
```r
sum(1,2,3) # 6
```
:::

Der Funktionsaufrufoperator darf nicht mit den aus der Mathematik bekannten Klammern verwechselt werden. Klammern fassen auch in R Teiloperationen zusammen und reihen diese gegenüber einer anderen Teiloperation vor. Der Funktionsaufrufoperator kommt nur zur Anwendung, wenn ein Name auf eine Funktion verweist.

Wird der Funktionsaufrufoperator zusammen mit dem Schlüsselwort `function` verwendet, dann wird eine Funktion mit den angegebenen Parametern erstellt (s. @exm-funktionsdeklaration). Diese Operation heisst **Funktionsdeklaration**. Eine deklarierte Funktion muss in R einem Namen zugewiesen werden.

::: {#exm-funktionsdeklaration}
## Funktionsdeklaration
```r
add3 <- function (eins, zwei, drei) 
    sum(eins, zwei, drei)
```
:::

### Blöcke

Mehrere Operationen lassen sich in R zu *Blöcken* zusammenfassen. Ein Block wird durch geschweifte Klammern (`{` und `}`) markiert. Geschweifte Klammern bilden also den **Blockoperator**.

Ein Block wird von R als eine Operation behandelt. Damit die Operationen in einem Block von R ausgeführt werden können, müssen alle übergeordneten Blöcke geschlossen werden. 

@exm-funktionsblock zeigt die Deklaration einer Funktion, die aus zwei Operationen besteht. Damit beide Operationen zu einer Funktion zusammengefasst werden können, müssen diese in einen Block gefasst werden.

::: {#exm-funktionsblock}
## Funktionsdeklaration mit Block
```r
add3mod6 <- function (eins, zwei, drei) {
    sum(eins, zwei, drei) -> sechs
    sechs %% 6
}
```
:::
