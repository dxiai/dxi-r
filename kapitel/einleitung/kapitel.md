---
execute: 
  echo: false
---
# Einleitung {sec-chapter-einleitung}

::: {.callout-warning}
## Work in Progress
:::

## Motivation und Ausgangslage


## Base R und Tidy R

R ist eine Programmiersprache, die durch *Funktionsbibliotheken* erweitert wird. Beim Starten von R wird zuerst nur das Basissystem geladen. Das R-Basissystem besteht aus den eingebauten Sprachelementen und Funktionen sowie aus den Bibliotheken `base`, `compiler`, `datasets`, `grDevices`, `graphics`, `grid`, `methods`, `parallel`, `splines`, `stats`, `stats4`, `tcltk`, `tools`, `translations`, and `utils`. Alle Funktionen dieser Bibliotheken stehen damit nach dem Start von R sofort bereit. Diese Funktionen heissen im R-Jargon **Base R**. Dieses Basissystem stellt bereits alle Funktionen für das statistische Programmieren bereit. 

Base R verwendent sehr viele *Idiome* als Funktionsnamen, aus denen sich nicht intuitiv erschliesst, was eine Funktion leistet. Ausserdem wurden im Laufe der Entwicklung immer wieder Funktionen dem Basissystem hinzugefügt, die sich nicht konsistent in das bestehende System aus Funktionen integrieren. Als Beispiel sollen die Funktionen `eapply()`, `lapply()`, `sapply()`, `tapply()` und `vapply()` sowie `replicate()` und `rep()` dienen. Bis auf `replicate()` sehen die Funktionsnamen ähnlich aus, werden aber in unterschiedlichen Kontexten verwendet und auf unterschiedliche Weise aufgerufen. `replicate()` und `rep()` haben einen ähnlichen Funktionsnamen und in der Beschreibung dienen beide Funktionen der replikation. Die Funktion `replicate()` ist aber eine Variante der Funktion `lapply()` mit ähnlicher Syntax und `rep()` nicht. 

Beim Erlernen von Base R müssen die Kernsyntax der Programmiersprache, die Verwendung der Idiome mit ihren passenden Kontexten und Anwendungen sowie alle Widersprüche erlernt werden. Für Programmierneulinge erscheinen Programme in Base R sehr kyptisch und wenig intuitiv. Selbst erfahrenen R-Entwickler:innen erschliesst sich die Funktionsweise einiger Base R-Programme erst nach dem Studium der zugehörigen Bibliotheksdokumentation. 

Mit zunehmender Bedeutung der Datenwissenschaften, wurden die Inkonsistenzen von Base R zum Hindernis für komplexe Anwendungen und Analysen. Ausgehend von einer konsistenten Syntax für die Datenvisualisierung wurden nach und nach R-Bibliotheken für eine konsistente und koherente Datentransformation und -Auswertung bereitgestellt. Diese Bibliotheken stellen Daten und Datenströme in das Zentrum der Programmierung. Durch selbsterklärende Funktionsnamen, das zusammenfassen in Funktionsgruppen und einheitliche Logik für Funktionsaufrufe bilden diese Bibliotheken einen *R-Dialekt*, der als **tidy R** bezeichnet wird. R-Programme sind auch für unerfahrende R-Interessierte deutlich intuitiver zu verstehen, wenn sie mit den tidy R Konzepten entwickelt wurden, als vergleichbare Base R Varianten. Durch den datenzentrierten Zugang lässt sich tidy R wesentlich leichter erlernen als Base R. 

Den Kern von tidy R bildet die Bibliothek `tidyverse`, die die wichtigsten Funktionen für die Datentransformation und die Datenvisualisierung zusammenfasst. Sie besteht aus den Unterbibliotheken `dplyr`, `forcats`, `ggplot2`, `purrr`, `readr`, `tibble` und `tidyr`. 

Wichtige ergänzende Funktionen für die Statistik und das statistische Modellieren werden durch die Bibliotheken `rstatix` und `tidymodels` bereitgestellt. 

In diesem Buch werden alle Konzepte datenzentrisch mit dem tidy R Ansatz erarbeitet. Base R Konzepte, Operatoren und Funktionen werden nur verwendet, wenn diese nicht im Widerspruch zu tidy R stehen. In diesen Fällen werden diese nicht gesondert als Base R hervorgehoben.

## Organisation dieses Buchs
