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

`nest()` 

`unnest()`

## Transponieren mit Zeichenketten

Ein spezieller Fall ist gegeben, wenn die Daten in Zeichenketten kodiert sind. Streng genommen handelt es sich hierbei nicht um eine Transponierenoperation, sondern um eine normale Transformation. Die entsprechenden Funktionen für diese Operation wurden aber nach dem gleichen Prinzip wie beim Hierarchisieren bzw. beim Transponieren angepasst.

`separate_longer_delim()`

`separate_wider_delim()`

Gelegentlich sind die Daten in einer Zeichenkette nicht über ein eindeutiges Trennzeichen, sondern über ein Trennmuster kodiert. In diesem Fall kann das Trennmuster als *regulärer Ausdruck* der Funktion `separate_longer_regex()` bzw. `separate_wider_regex()` übergeben werden.

Die Umkehrfunktion ist die Funktion `unite()`, mit der sich mehrere Vektoren zu einer Zeichenkette verketten lassen. 