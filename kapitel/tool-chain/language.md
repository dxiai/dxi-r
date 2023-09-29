# Sprachelemente von R

::: {.callout-warning}
## Work in Progress
:::

- Symbole
- Operatoren
- Operationen
- Blöcke

## Symbole

### Werte

### Schlüsselworte

R kennt verschiedene Schlüsselworte, die als eigene Symbole festgelegt sind und nicht verändert werden können. Jedes der folgenden Schlüsselworte hat eine Bedeutung für R und kann nur in dieser Bedeutung verwendet werden. 

Funktionen (@sec-chapter-variablen)

- `function`

Entscheidungen (@sec-chapter-booleans)

- `if`
- `else`

Schleifen

- `repeat`
- `while`
- `for`
- `next`
- `break`

::: {.callout-important}
## Keine Schleifen in der Paxis

R-Schleifen sind im Vergleich zu vergleichbaren Funktionen ***äusserst ineffizient***. Deshalb haben Schleifen in der praktischen Anwendung von R in den Datenwissenschaften **keine Bedeutung** mehr. Stattdessen werden ausschliesslich spezielle Funktionen über Datenstrukturen verwendet. Entsprechend finden sich diese Schlüsselworte sehr selten in R-Skripten.
:::


### Namen

Ist ein Symbol kein Schlüsselwort und kein Wert, dann wird das Symbol als Name behandelt. Ein Name steht für einen Wert (@sec-chapter-datentypen) und kann wie ein Wert in Operationen verwendet werden. 

Namen können beliebige Zeichen enthalten. R unterscheidet Namen von Werten indem die folgenden Regeln für Namen verwendet werden:

Weicht ein Name von diesen Regeln ab, dann muss dieser als solcher durch Backticks gekennzeichnet werden (`).

::: {#exm-gekennzeichneter-name}
## markierter Name

```r
`merkwürdiger Name`
```
::: 

## Operatoren

## Operationen

## Blöcke

Mehrere Operationen können in R zu Blöcken zusammengefasst werden. In diesem Fall müssen die zusammengehörenden Opeartionen durch geschweifte Klammern (`{` und `}`) eingerahmt werden.

Ein Block wird von R als eine Operation behandelt. Damit die Operationen in einem Block von R ausgeführt werden können, müssen alle übergeordneten Blöcke geschlossen werden. 