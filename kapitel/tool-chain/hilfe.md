# Hilfe bekommen {#sec-chapter-hilfe-bekommen}

R-Funktionen sind in der Regel gut dokumentiert. Neben der eigentlichen Funktionsbeschreibung finden sich viele ausführliche Problemlösungsstrategien in Form sog. *Vignettes*. Zusätzlich finden sich für die `tidyverse`-Bibliotheken sog. *Cheat Sheets*, die einen schnellen Überblick über die Kernfunktionen erlauben. 

::: {.callout-tip}
## Praxis
Nutzen Sie die Dokumentation regelmässig, um die richtigen Funktionen für Ihre Problemstellungen auszusuchen. Die verschiedenen Teile der R-Dokumentation helfen Ihnen die Konzepte und Techniken für die Arbeit mit R zu vertiefen.
:::

## `help()`

Die `help()`-Funktion ist der erste Anlaufpunkt, um mehr über eine Funktion zu erfahren. 

R-Funktionen sind in der Regel sehr ausführlich dokumentiert. Falls Sie Details über die Arbeitsweise einer Funktion erfahren möchten, können Sie die Dokumentation einer Funktion mit der `help()`-Funktion abrufen. Dazu rufen Sie diese Funktion wie jede andere R-Funktion auf. 

Die `help()`-Funktion ist Teil von `Base R` und ist in jeder Umgebung verfügbar. 

Die Funktion erwartet den gewünschten Funktionsnamen. `help()` kann der Funktionsname direkt oder als Zeichenkette als Parameter übergeben werden. D.h. die beiden folgenden Operationen haben den gleichen Effekt und zeigen die Dokumentation der Funktion `read.csv` an.

::: {#exm-hilfe-anzeigen}
## Hilfe anzeigen
```r
help(read.csv)
help("read.csv")
```

Jede Funktionsdokumentation besteht aus den folgenden Teilen:

1. Beispielen für den Aufruf der Funktion
2. Beschreibung aller Funktionsparameter
3. Einer Detaillierten Funktionsbeschreibung
4. Beispielen

Die Beispiele zeigen typische Aufrufe der jeweiligen Funktion und finden sich **immer** *am Ende* der Dokumentation. Es lohnt sich häufig zuerst die Beispiele anzusehen und danach die Funktionsdetails zu lesen. 

## Vignettes

Viele R-Bibliotheken haben komplexe Anwendungen. Diese Anwendungen werden in sogenannten *Vignettes* beschrieben. 

Sie können sich die verfügbaren Vignettes für eine Bibliothek mit der Operation `vignette(package = bibliotheksname)` anzeigen lassen. Wenn Sie z.B. alle Vignettes für die dplyr Bibliothek anzeigen lassen möchten, dann geben Sie `vignette(package = "dplyr")` ein. Das Ergebnis ist die Liste der verfügbaren Vignettes für diese Bibliothek. 

Wenn Sie das gesuchte Thema gefunden haben, dann können Sie sich die Vignette mit dem folgenden Befehl anzeigen lassen: `vignette(thema, package = bibliotheksname)`
 

## Cheat Sheets

Die *tidyverse*-Bibliotheken bieten zusätzlich *Schummelzettel* für die wichtigsten Funktionen und Techniken für eine Bibliothek auf zwei Seiten. Diese Schummelzettel werden auch als *Cheat Sheets* bezeichnet. Sie können diese Cheat Sheets doppelseitig ausdrucken und als Schnellreferenz verwenden.
