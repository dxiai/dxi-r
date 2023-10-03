---
abstract: ""

execute: 
  echo: false
---
# Variablen, Funktionen und Operatoren {#sec-chapter-variablen}

::: {.callout-warning}
## Work in Progress
:::

## Variablen

Variablen sind spezielle R Symbole (s. @sec-chapter-language) mit denen Werte für die spätere Verwendung gespeichert werden. Variablen erzeugen Referenzen, über die Werte zugegriffen werden können. Es sind also **Namen**, die die eigentlichen Werte *substituieren*. 

::: {#exm-var-zuweisen}
## Den Wert 1 der Variable `var1` zuweisen
```r
var1 = 1
```
:::

Variablen sollten in einem Kontext *eindeutig* sein. Wird nämlich einer Variable mehrfach zugewiesen, dann ist der Wert der Variable der Wert der letzten Zuweisung.

::: {.callout-warning}
Die letzte Zuweisung ist nicht zwingend die Zuweisung die als letztes im Code erfolgt. Deshalb sollte immer geprüft werden, ob ein Variablenname bereits verwendet wird.
:::

## Funktionen


### Operatoren


### Funktionsketten

Datenströme

### Eigene Funktionen erstellen

In R werden Funktionen mit dem `function`-Schlüsselwort erstellt. Eine R-Funktion besteht aus einer Parameterliste und einem Funktionskörper. Die Parameterliste wird in Klammern hinter dem Wort `function` angegeben. Der Funktionskörper kann eine einzelne Operation oder ein Block sein. Das Ergebnis einer Funktion ist das Ergebnis der letzten Operation des Funktionskörpers.

@exm-function-create zeigt eine *Funktionsdeklaration*, die einen `parameter` *akzeptiert*. Die Funktion quadriert diesen Wert und zieht vom Ergebnis `1` ab. An diesen Operationen wird erkannt, dass die Funktion nur Werte vom Datenyp Zahlen als `parameter` akzeptiert.

Parameter sind in R spezielle *Variablen*, mit denen Werte an eine Funktion übergeben werden. Parameter existieren nur *innerhalb* einer Funktion während der Ausführung des Funktionskörpers. Es kommt sehr häufig vor, dass ausserhalb einer Funktion Variablen mit gleichem Namen vorhanden sind. Ein Parameter überschreibt diese Variablen **nicht**.

::: {#exm-function-create}
## Eine Funktion deklarieren
```r
function (parameter) {
    parameter ^ 2 - 1
}
```
:::

Eine Funktion ist in R ein Wert wie eine Zahl oder eine Zeichenkette. R gibt eine direkte Funktionsdefinition wie jeden Wert direkt aus. Im Fall von Funktionen ist das die Funktionsdeklaration. Damit eine Funktion sinnvoll verwendet werden kann, muss sie zuerst einer Variablen zugewiesen werden. Der Name einer Funktion sollte möglichst die zentrale Bedeutung einer Funktion beschreiben.

::: {.callout-note}
Die Wahl eines guten Funktionsnamen hängt vom jeweiligen Kontext ab.

Mathematische Funktionen werden oft mit $f(x)$ oder $g(x)$ usw. geschrieben. In R sind solche Namen ebenfalls zulässig, solange sie **eindeutig** sind. Solche sehr kurzen Funktionsnamen sollten speziell gekennzeichnet und dokumentiert werden.
:::

@exm-function-named-create weist der Funktion aus @exm-function-create den Namen `quadrat_minus_eins` zu. Dieser Name kann anschliessend als Funktion verwendet werden (s. @exm-funktion-aufrufen).

::: {#exm-function-named-create}
## Eine Funktion mit Namen deklarieren
```r
quadrat_minus_eins = function (parameter) {
    parameter ^ 2 - 1
}
```
:::

::: {#exm-funktion-aufrufen}
## Eine Funktion aufrufem
```r
quadrat_minus_eins(2)
```

```
3
```
:::

::: {.callout-tip}
## Praxis
Wird der neuen Funktion ein falscher Datentyp als Parameter übergeben, dann können die Rs Fehlermeldungen sehr verwirrend sein. Es ist daher ein guter Stil, Parameter die bestimmte Datentypen erfordern direkt zu Begin des Funktionskörpers zu prüfen (s. @exm-fkt-datentyp-prüfen).
:::

::: {#exm-fkt-datentyp-prüfen}
## Eine Funktion mit Namen deklarieren
```r
quadrat_minus_eins = function (parameter) {
    stopifnot(is.numeric(parameter))
    parameter ^ 2 - 1
}
```
:::


### Bibliotheken

Oft ist es nicht notwendig eigene Funktionen zu erstellen. Stattdessen kann in vielen Fällen auf Funktionsbibliotheken zurückgegriffen werden, die bereits entsprechende Funktionen bereitstellen. 

R wird durch Funktionsbibliotheken erweitert. Eine Funktionsbibliothek stellt hauptsächlich Funktionen und Operationen für bestimme Algorithmen oder Analysemethoden bereit. Eine Funktionsbibliothek wird mit der Funktion `install.packages()` auf einem Rechner installiert. 

In einem R-Script lassen sich die Funktionen einer Bibliothek auf zwei Arten nutzen:

1. Die Bibliothek wird mithilfe der Funktion `library()` in den Code eingebunden. 
2. Eine Funktion einer Bibliothek wird direkt angesprochen. 

Die erste Option bietet sich an, wenn ein Script viele Funktionen einer Bibliothek aufrufen wird. R läd in diesem Fall *alle Funktionen* der Bibliothek, so dass diese direkt verwendet werden können.

::: {#exm-fkt-library-load}
## Funktionen mit der `library()` Funktion einbinden
```r
library(ggplot2)

mtcars |> 
    ggplot(aes(mpg, hp)) +
        geom_point()
```
:::

Die zweite Option ist sinnvoll, wenn nur eine oder zwei Funktionen einer Bibliothek verwendet werden sollen. In diesem Fall muss R nicht die gesamte Bibliothek bereitstellen, sondern läd gezielt nur die gewünschten Funktionen. 

::: {#exm-fkt-direct-access}
## Eine Funktion direkt ansprechen
```r
mtcars |> 
    dplyr::filter(hp > 200)
```
:::

::: {.callout-note}
R bietet sog. Meta-Bibliotheken an, mit denen mehrere Bibliotheken gemeinsam verwendet werden können. Funktionen können nur nicht über den Namen einer Meta-Bibliothek, sondern immer nur über die Bibliothek, die einer Funktion definiert. 
:::

Die `tidyverse`-Bibliothek ist eine solche Meta-Bibliothek. @exm-fkt-dir-ansprechen-meta zeigt wie die Funktion `read_delim()` direkt angesprochen werden kann, wenn die `tidyverse`-Bibliotheken nicht mit `library(tidyverse)` eingebunden wurden. `read_delim()` wird in der Bibliothek `readr` definiert. Entsprechend kann die Funktion nur über `readr::read_delim()` aufgerufen werden.

::: {#exm-fkt-dir-ansprechen-meta}
## Funktion aus Unterbibliothek direkt ansprechen
```r
readr::read_delim("meine_daten.csv")
# entspricht:
#   library(tidyverse)
#   read_delim("meine_daten.csv")
```
:::

::: {.callout-note}
Die Syntax von R kann durch Module erweitert werden. Diese Form nutzt die Konzepte zur **Metaprogrammierung** von R. Dadurch können neue Programmierkonzepte in die Sprache einfliessen. Die `tidyverse`-Bibliotheken nutzen diese Möglichkeit intensiv. Solche Bibliotheken **müssen** mit der Funktion `library()` eingebunden werden, damit die zusätzliche Syntax bereitgestellt wird.
::: 

## Bibliotheksmanagement

Verwendet ein R-Script Funktionsbibliotheken, dann ist dieses Script nur auf Rechnern lauffähig, auf denen die benutzten Bibliotheken auch installiert sind. Solche notewendigen Bibliotheken heissen die **Abhängigkeiten** (engl. *dependencies*) eines Scripts. Alle Abhängigkeiten müssen dokumentiert werden. 

::: {.callout-tip}
## Praxis

Im Internet gibt es sehr viele Beispiele, die die Funktion `install.packages()` als Teil des Programmcodes darstellen. In konkreten R-Projekten sollte die Funktion `install.packages()` **nie** in einem normalen R-Script aufgerufen werden, weil bei jedem Start des Script geprüft wird, ob eine neue Version der Bibliothek existiert. Diese Technik stellt ein Sicherheitsrisiko dar, weil bei jeder Ausführung des Scripts Installationen unkontrolliert vorgenommen werden können und Schadcode auf die Systeme geschleust werden kann. 

Das Risiko unkontrollierter Installationen wird verringert, indem Installationen von der Programmlogik getrennt und nur kontrolliert durchgeführt werden. Dadurch wird die Installation von Funktionsbibliotheken von ihrer Anwendung getrennt.
:::

Die Dokumentation von Abhängigkeiten wird normalerweise von einem sog. Packetmanagement übernommen. R verfügt über *kein integriertes* Packetmanagement. Dieses wird von der Bibliothek `renv` übernommen. Bevor dieses genutzt werden kann muss `renv` mit `install.packages("renv")` installiert werden. 

::: {.callout-tip}
## Praxis
`renv` sollte bei der Installation von R gleich mitinstalliert werden.
:::

`renv` ist ein Packetmanagementsystem für R. Anders als die Funktion `install.packages()` installiert `renv` nicht nur Bibliotheken, sondern dokumentiert auch die Abhängigkeiten eines Projekts in einer Form, dass alle Abhängigkeiten einfach auf dem System installiert werden können. Mit `renv::restore()` lässt sich ein Projekt in einer anderen Umgebung mit allen Abhängigkeiten konfigurieren und ausführen. 

Wird eine Bibliothek mit `renv` installiert, dann steht diese Bibliothek nur dem jeweiligen Projekt zur Verfügung. Was auf dem ersten Blick als Nachteil klingt, ist ein grosser Vorteil, wenn unterschiedliche Projekte besonderen Anforderungen an die Versionen einer Bibliothek haben. Auf diese Weise kann jedes Projekt die richtige Version einer Bibliothek verwenden und beeinflusst keine anderen Projekte.

### Projektvorbereitung

Ein Projekt wird mit `renv::init()` für die Verwendung des Packetmanagements vorbereitet. Beim ersten Aufruf von `renv` werden die internen Abhängigkeiten von `renv` kontrolliert und notfalls installiert. Das nimmt etwas Zeit in Anspruch. Das Packetmanagement erfasst automatisch alle Bibliotheken, die systemweit installiert wurden. Dadurch wird sichergestellt, dass alle Bibliotheken berücksichtigt wurden, die im eigenen System installiert sind und deshalb im Projekt verwendet werden können.

### Bibliotheken installieren

Nach der Initialisierung des Packetmanagements können projektspezifische Bibliotheken mit `renv::install()` installiert werden. War eine Installation erfolgreich, sollte die Bibliothek auf ihre Funktionstüchtigkeit mit einem einfachen Beispiel geprüft und danach mit `renv::snapshot()` als Abhängigkeit dokumentiert werden. Mit einem *Snapshot* wird eine Bibliotheksversion als Abhängigkeit registiert. Im Gegensatz zu `install.packages()` wird ab diesem Zeitpunkt nicht mehr eine beliebige Version der Bibliothek installiert, sondern nur die dokumentierte Version. Dadurch wird sichergestellt, dass der Code auch in anderen Umgebungen wie erwartet funktioniert. 

### Updates für Bibliotheken 

Eine Besonderheit von `renv` ist die Möglichkeit, kontrollierte Updates für einzelne oder alle Abhängigkeiten eines Projekts mit `renv::update()` durchzuführen. `renv::update()` installiert die neusten Versionen der Projektbibliotheken. 

::: {.callout-note}
## Merke
Updates sollten nie unklontrolliert akzeptiert werden!
:::

Bevor neue Bibliotheksversionen in das Packetmanagement aufgenommen werden, sollte immer geprüft werden, ob der bestehende Code mit den neuen Versionen immer noch funktioniert. Sollten bei dieser Prüfung Probleme auftreten, dann können die Updates mit `renv::revert()` wieder rückgängig gemacht werden. Gab es keine Probleme, dann können die Updates mit  `renv::snapshot()` übernommen werden. 
