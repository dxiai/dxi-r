---
# bibliography: references.bib

abstract: ""

execute: 
  echo: false
---

# Daten formen {#sec-chapter-daten-formen}

::: {.callout-warning}
## Work in Progress
:::

Die Funktionen zum Formen von Datenrahmen werden durch die `tidyverse`-Bibliothek `tidyr` bereitgestellt. 

## Transponieren



`pivot_longer()`

`pivot_wider()`

### Rezept: Operationen zusammenfassen



## Hierarchisieren 

Beim Hierarchisieren werden ausgewählte Vektoren eines Datenrahmens entlang eines Sekundärinzes zu separaten Datenrahmen organisiert und in den ursprünglichen Datenrahmen eingebettet. Über solche eingebettete Datenrahmen lassen sich hierarchische Datenstrukturen erzeugen, die beispielsweise in einem JSON- oder YAML-Dokument (s. @sec-chapter-daten-importieren) gespeichert werden können. In R übernimmt diese Aufgabe die Funktion `nest()`.

Weil R jedoch keine Vektoren mit Datenrahmen erlaubt, müssen diese Datenrahmen zusätzlich in eine Liste geschachtelt werden. Diese Liste dient nur zur Ablage in einem Vektor und besteht immer nur aus einem Element, nämlich dem eingebetteten Datenrahmen.

Mit der Funktion `unnest()` lassen sich eingebettete Datenrahmen wieder ausbetten. Diese Operation funktioniert jedoch nur dann zuverlässig, wenn alle eingebetteten Datenrahmen einheitliche Vektoren haben. Ist diese Bedingung nicht gegeben, dann erzeugt R zusätzliche Vektoren mit `NA` für alle Datensätze, die die Vektoren ursprünglich nicht enthielten. Die Funktion `unnest()` führt mehrere Spaltenkonkatenationen aus und ähnelt damit im Ergebnis der Funktion `bind_rows()`.

Die beiden Funktionen `nest()` und `unnest()` sind speziell zur Erzeugung bzw. zum Auflösen von eingebetteten Datenrahmen gedacht. Gelegentlich enthält ein Datenrahmen einen Vektor mit einfachen Listen oder benannten Listen. Solche Vektoren sind vom Typ `list` und sollten mit den Funktionn `unnest_longer()` für einfache Listen sowie `unnest_wider()` für einheitlich benannte Listen [@wickham_tidyr_2023]. 

## Transponieren mit Zeichenketten

Ein spezieller Fall ist gegeben, wenn die Daten in Zeichenketten kodiert sind. Streng genommen handelt es sich hierbei nicht um eine Transponierenoperation, sondern um eine normale Transformation. Die entsprechenden Funktionen für diese Operation wurden aber nach dem gleichen Prinzip wie beim Hierarchisieren bzw. beim Transponieren angepasst.

Wie beim Transponieren kann ein Vektor in die Lang- oder die Breitform getrennt werden. Für die meisten Anwendungen eignen sich zwei Funktionen: 

- `separate_longer_delim()`
- `separate_wider_delim()`

Gelegentlich sind die Daten in einer Zeichenkette nicht über ein eindeutiges Trennzeichen, sondern über ein Trennmuster kodiert. In diesem Fall kann das Trennmuster als *regulärer Ausdruck* der Funktion `separate_longer_regex()` bzw. `separate_wider_regex()` übergeben werden.

Für festkodierte Werte stehen die beiden Funktionen `separate_longer_position()` und `separate_wider_position()` zur Verfügung. Diese Funktionen erwarten einen Vektor mit den Feldbreiten, um die Werte trennen zu können. 

Die Umkehrfunktion für alle `separate_`-Funktionen ist die Funktion `unite()`, mit der sich die Werte aus einer Lang- oder Breitform zu einem Zeichenkettevektor verketten lassen.
