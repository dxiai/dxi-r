---
abstract: ""

execute: 
  echo: false
---
# Variablen, Funktionen und Operatoren {#sec-chapter-variablen}

## Variablen

Variablen sind spezielle R Symbole (s. @sec-chapter-language) mit denen Werte für die spätere Verwendung markiert werden. Variablen sind also **Bezeichner**, welche die eigentlichen Werte **substituieren**. 

Damit eine Variable einen Wert substituieren kann, muss der Wert der Variablen *zugewiesen* werden. Ein Wert kann dabei ein einzelner Wert eines fundamentalen Datentyps oder eine komplexe Datenstruktur sein. 

Bei der ersten Zuweisung wird eine Variable *deklariert* (@def-deklaration).

::: {#exm-var-zuweisen}
## Den Wert 1 der Variable `var1` zuweisen
```r
var1 = 1
```
:::

Variablen müssen in einem Geltungsbereich *eindeutig* sein. Wird nämlich einer Variable mehrfach zugewiesen, dann ist der Wert einer Variablen der Wert der letzten Zuweisung.

Der **Geltungsbereich** (engl. Scope) einer Variablen wird durch Funktionskörper definiert. R kennt dabei drei Arten von Geltungsbereichen. In diesem Zusammenhang spricht man von äusseren (engl. *outer scope*) und inneren Geltungsbereichen (engl. *inner scope*).

Grundsätzlich können alle Variablen in einem Geltungsbereich verwendet werden, die in einem der äusseren Geltungsbereiche deklariert und zugewiesen wurden. Variablen der inneren Geltungsbereiche sind in den äusseren Geltungsbereichen *nicht verfügbar*. 

Der **globale Geltungsbereich** gilt für alle Variablen, die ausserhalb einer Funktion oder einer Bibliothek erzeugt werden.

Der **Funktionsgeltungsbereich** ist auf den Funktionsköper einer Funktion beschränkt. 

Der **Modulgeltungsbereich** ist der globale Geltungsbereich einer Funktionsbibliothek. Variablen dieses Geltungsbereichs sind im globalen Geltungsbereich eines R-Scripts nicht erreichbar. In der Praxis spielt dieser Geltungsbereich eine untergeordnete Rolle

::: {.callout-warning}
Die letzte Zuweisung ist nicht zwingend die Zuweisung, die als letztes im Code erscheint.
:::

## Funktionen

In R bilden Funktionen die Grundlage für die Datenverarbeitung. 

::: {.callout-note}
## Merke
Eine Funktion ist für R ein Wert wie eine Zahl oder eine Zeichenkette. 
:::

Im Fall von Funktionen ist der Wert einer Funktion die Funktionsdeklaration. Entsprechend ist es möglich Funktionen zu überschreiben. 

Wird nur der Bezeichner einer R gibt eine direkte Funktionsdefinition wie jeden anderen Wert direkt aus. 

### Operatoren

Alle R-Operatoren sind Funktionen. R kennt 29 vordefinierte Operatoren, die zwei Werte verknüpfen. Zu diesen Operatoren gehören die auch die arithmetischen Operatoren für die Grundrechenarten. 

| Operator | Beschreibung | Art | 
| :---: | :------- | :--- |
| `+`	| Plus, sowohl unär als auch binär | arithmetisch |
|  `-` |	Minus, sowohl unär als auch binär | arithmetisch |
| `*`	| Multiplikation, binär | arithmetisch |
| `/`	| Division, binär | arithmetisch |
| `^`	| Potenz, binär | arithmetisch |
| `%%`	| Modulo, binär | arithmetisch |
| `%/%`	| Ganzzahldivision, binär | arithmetisch |
| `%*%`	| Matrixprodukt, binär | arithmetisch, Matrix |
| `%o%`	| äusseres Produkt, binär | arithmetisch, Matrix |
| `%x%`	| Kronecker-Produkt, binär | arithmetisch, Matrix |
| `<`	| Kleiner als, binär |  logisch |
| `>`	| Grösser als, binär | logisch |
| `==` | Gleich, binär | logisch |
| `!=` | Ungleich, binär | logisch |
| `>=` | Grösser oder gleich, binär | logisch |
| `<=` | Kleiner oder gleich, binär | logisch |
| `%in%` | Existenzoperator, binär | logisch |
| `!` |	unäres Nicht | logisch |
| `&`	| Und, binär, vektorisiert | logisch |
| `&&`	| Und, binär, nicht vektorisiert | logisch |
| `|` |	Oder, binär, vektorisiert | logisch |
| `||` | Oder, binär, nicht vektorisiert | logisch |
| `<-`, `<<-`, `=` |	linksgerichtete Zuweisung, binär | Zuweisung |
| `->`, `->>` |	rechtsgerichtete Zuweisung, binär | Zuweisung |
| `[` | Indexzugriff (Vektoren), binär |  Index |
| `$`, `[[`	| Listenzugriff, binär | Index |
| `~`	| funktionale Abhängigkeit, sowohl unär als auch binär | Funktionen |
| `:`	| Sequenz (in Modellen: Interaktion), binär | Funktionen |
| `?`	| Hilfe | spezial |

: Liste der Base R Operatoren {#tbl-r-operatorem}

::: {.callout-note}
Im R-Umfeld wird oft von **Modellen** geschrieben und gesprochen. *Modelle* sind *spezielle Funktionen*, die *Beziehungen zwischen Daten* beschreiben, ohne eine mathematisch exakte Beziehung vorzugeben. Modelle werden in der *Statistik* und *Stochastik* eingesetzt, wenn die exakten Beziehungen zwischen Daten unbekannt sind.
:::

::: {#exm-exakte-beziehung}
## Exakte *lineare* Beziehung zwischen Daten
```r
f = function (x) 2 * x + 3
```
:::

::: {#exm-beziehung-als-modell}
## Beziehung zwischen Daten mit Interaktion als Modell
```r
f = y ~ x : c
```
:::

Hinter jedem Operator steht eine Funktion, die mit den beiden Operanden als Parameter ausgeführt wird, um das Ergebnis des Operators zu bestimmen. Daraus folgt, dass jeder Operator auch als Funktionsbezeichner verwendet werden kann. In diesem Fall muss R mitgeteilt werden, dass der Operator nun als Funktionsbezeichner verwendet werden soll. Der Operator muss also  mit Backticks als Bezeichner markiert werden.

::: {#exm-plus-als-fkt}
## `+`-Operator als Funktionsbezeichner
```r
`+`(1, 2)
```
```
3
```
:::

#### Zuweisungsoperatoren

R kennt zwei Zuweisungsoperatoren: `<-` und `->`. Die Zuweisung erfolgt in Richtung des Pfeils. Daneben wird der `=`-Operator ebenfalls als (inoffizieller) Zuweisungsoperator unterstützt. 

Ein Zuweisungsoperator erwartet immer einen Bezeichner und eine Operation als Parameter. Das Ergebnis der Operation wird als Wert dem Bezeichner zugewiesen. 

Weil nicht immer klar ist, ob `<-` oder `=` verwendet werden soll, lautet die offizielle Kommunikation, dass für Variablenzuweisungen der `<-`-Operator verwendet werden sollte. Das einfache Gleich (`=`) weist einen Wert einem Funktionsparameter zu. Gerade in **tidy R** ist dieser Unterschied nur schwer nachvollziehbar, weil bestimmte Parameter wie Variablen behandelt werden.

::: {.callout-note}
In diesem Buch wird für die *linksgerichtete Zuweisung* immer das  Gleichzeichen (`=`) verwendet, so dass eine Zuweisung eines Werts an eine Variable und an einen Parameter gleichwertig behandelt wird. Dadurch wird die Lesart etwas vereinfacht. Zusätzlich wird die rechtsgerichtete Zuweisung konsequent als Abschluss für einen primären Datenstrom (s. @sec-fkt-ketten) eingesetzt.
:::

#### Ausführungsoperator

Der Ausführenoperator (`()`) gilt in R offiziell nicht als Operator, weil dieser nicht als Funktion umgesetzt werden kann. Es gibt zwar die Funktion `do.call()`, um eine Funktion auszuführen. Wenn diese Funktion als Ausführungsoperator eingesetzt wird,  müsste `do.call()` sich selbst aufrufen, um sich selbst auszuführen. Dieses Problem wird von R dadurch gelöst, dass `(` und  `)` als eigene *Symbole* erkannt werden und immer eine Funktionsausführung anzeigen.

#### Hilfeoperator

Der *Hilfeoperator* ist ein besonderer Operator, weil dieser die Interaktion mit der Dokumentation von Funktionen und Konzepten ermöglicht. Der Hilfeoperator wird normalerweise nicht in einem R-Script verwendet und hat keine Bedeutung für die Datenverarbeitung.

Der Hilfeoperator kann direkt mit einem Bezeichner aufgerufen werden. Existiert für den Bezeichner eine Dokumentation, dann wird diese angezeigt.

::: {#exm-hilfeop-funktionsname}
## Dokumentation der Funktion `is.character()`
```r 
?is.character
```
:::

Wird der Hilfeoperator mit sich selbst aufgerufen, wird der nächste Wert als Suchbegriff gewertete und eine Suche über alle Hilfedokumente auf dem System durchgeführt.

::: {#exm-hilfeop-hilfeop}
## Dokumentationssuche nach Operatoren
```r
??operator
```
::: 


### Funktionsketten {#sec-fkt-ketten}

R unterstützt die spezielle Funktionsverkettung mit dem `|>`- Operator. Dadurch lassen sich Funktionsfolgen direkt in R ausdrücken. In Kombination mit der rechtsgerichteten Zuweisung (`->`) ist es möglich, Datenströme durch eine Funktionskette von einem Ausgangswert zu einem Ergebnis in der natürlichen Reihenfolge aufzuschreiben.

::: {#exm-fktkette-datenstrom}
## Funktionskette mit abschliessender Zuweisung
```r
# library(tidyverse)
iris |>
    filter(Species == "setosa") |>
    arrange(desc(Petal.Length)) -> 
        sortierteSetosaWerte
```
:::

Neben der speziellen Funktionsverkettung (`|>`) gibt es einen sehr ähnlichen Verkettungsoperator: `%>%`. Dieser Verkettungsoperator ist Teil der `tidyverse`-Bibliothek und gleicht der speziellen Funktionsverkettung mit dem kleinen Unterschied, dass die Parameterzuweisung für die nachfolgende Funktion zusätzliche Kontrollmöglichkeiten bietet, die der speziellen Funktionsverkettung fehlen.  

## Eigene Funktionen erstellen

In R werden Funktionen mit dem `function`-Schlüsselwort erstellt. Eine R-Funktion besteht aus einer Parameterliste und einem Funktionskörper. Die Parameterliste wird in Klammern hinter dem Wort `function` angegeben. Der Funktionskörper kann eine einzelne Operation oder ein Block sein. Das Ergebnis einer Funktion ist das Ergebnis der letzten Operation des Funktionskörpers.

@exm-function-create zeigt eine *Funktionsdeklaration*, die einen `parameter` *akzeptiert*. Die Funktion quadriert diesen Wert und zieht vom Ergebnis `1` ab. An diesen Operationen wird erkannt, dass die Funktion nur Werte vom Datenyp Zahlen als `parameter` akzeptiert.

Parameter sind in R spezielle *Variablen*, mit denen Werte an eine Funktion übergeben werden. Parameter existieren nur *innerhalb* einer Funktion während der Ausführung des Funktionskörpers. Es kommt sehr häufig vor, dass ausserhalb einer Funktion Variablen mit gleichem Bezeichnern vorhanden sind. Ein Parameter überschreibt diese Variablen **nicht**.

::: {#exm-function-create}
## Eine Funktion deklarieren
```r
function (parameter) {
    parameter ^ 2 - 1
}
```
:::

Damit eine Funktion sinnvoll verwendet werden kann, muss sie zuerst einer Variablen zugewiesen werden. Der Bezeichner einer Funktion sollte möglichst die zentrale Bedeutung einer Funktion beschreiben.

::: {.callout-note}
Die Wahl eines guten Funktionsbezeichners hängt vom jeweiligen Geltungsbereich ab.

Mathematische Funktionen werden oft mit $f(x)$ oder $g(x)$ usw. geschrieben. In R sind solche Bezeichner ebenfalls zulässig, solange sie **eindeutig** sind. Solche sehr kurzen Funktionsbezeichnern sollten speziell gekennzeichnet und dokumentiert werden.
:::

@exm-function-named-create weist der Funktion aus @exm-function-create den Bezeichner `quadrat_minus_eins` zu. Dieser Bezeichner kann anschliessend als Funktion verwendet werden (s. @exm-funktion-aufrufen).

::: {#exm-function-named-create}
## Eine Funktion mit Bezeichner deklarieren
```r
quadrat_minus_eins = function (parameter) {
    parameter ^ 2 - 1
}
```
:::

::: {#exm-funktion-aufrufen}
## Eine selbstdeklarierte Funktion aufrufen
```r
quadrat_minus_eins(2)
```

```
3
```
:::

### Parameter und Variablen

Ein Parameter ist ein Platzhalter für einen Wert, der einer Funktion beim Funktionsaufruf übergeben wird. Parameter werden für eine spezielle Form der Variablenzuweisung eingesetzt. 

Im Funktionskörper verhält sich ein Parameter wie eine Variable. Einem Parameter können also in einem Funktionskörper neue Werte zugewiesen werden. Neben Parametern können Funktionskörper zusätzliche Variablen benötigen. Der Geltungsbereich dieser Variablen sind auf den Funktionskörper beschränkt.

### Datentypen überprüfen

Wird der neuen Funktion ein falscher Datentyp als Parameter übergeben, dann können die Rs Fehlermeldungen sehr verwirrend sein. Es ist daher ein guter Stil, Parameter die bestimmte Datentypen erfordern direkt zu Begin des Funktionskörpers zu prüfen (s. @exm-fkt-datentyp-prüfen).

::: {#exm-fkt-datentyp-prüfen}
## Eine Funktion mit Typenprüfung deklarieren
```r
quadrat_minus_eins = function (parameter) {
    stopifnot(is.numeric(parameter))
    parameter ^ 2 - 1
}
```
:::

### Nebeneffekte {#sec-nebeneffekte}

::: {.callout-important}
**Nebeneffekte** sind in (fast) immer unerwünscht. Die in diesem Abschnitt werden die beiden speziellen Zuweisungsoperatoren `<<-` und `->>` vorgestellt, die gezielt **Nebeneffekte** erzeugen.

Dieser Abschnitt beschreibt einen Sonderfall der Variablen- oder Funktionsdeklaration in **speziellen *Closures*** (s.u.), der in R **sehr selten** vorkommt. Die meisten Algorithmen lassen sich *nebeneffektsfrei* Programmieren, weshalb die beiden speziellen Zuweisungsoperatoren normalerweise nicht verwendet werden.
:::

Der Funktionskörper bildet einen abgegrenzten Geltungsbereich für Variablen. Alle normalen Zuweisungen gelten nur für den Funktionskörper, selbst wenn eine Variable oder ein Parameter ursprünglich in einem äusseren Geltungsbereich deklariert wurde.

::: {#exm-variablen-decl}
## Geltungsbereich von Variablen in Funktionen
```r
# Deklarationen
var1 = 1
f = function (x) {
    var1 = x + var1
    var1
}

# Anwendung
f(2)
var1
```

```
3 # Ergebnis von f(2)
1 # Ergebnis von var1
```
:::

In ***seltenen Fällen*** ist es notwendig, eine Variable eines äusseren Geltungsbereichs in einer Funktion einen neuen Wert zuzuweisen. Hier kommen die speziellen Zuweisungen `<<-` und `->>` zum Einsatz. Wird anstelle einer normalen Zuweisung die spezielle Zuweisung verwendet, dann wird einer Variablen oder einem Parameter eines äusseren Geltungsbereich ein neuer Wert zugewiesen. 

::: {#def-nebeneffekt}
Ändert eine Funktion eine Variable eines äusseren Geltungsbereichs, dann ist diese Änderung ein **Nebeneffekt** der Funktion. 
:::

::: {#exm-function-sideeffect}
## Funktion mit Nebeneffekt
```r
# Deklarationen
var1 = 1
f = function (x) {
    x + var1 ->> var1
    var1
}

# Anwendung
f(2)
var1
```
```
3 # Ergebnis von f(2)
3 # Ergebnis von var1
```
:::

::: {.callout-tip}
## Praxis
In R sollten ausschliesslich *Closures* Nebeneffekte haben, wenn eine Closure eine Variable einer generierenden Funktion ändern muss. ***Dieser Fall tritt sehr selten ein!***
:::


::: {.callout-important}
Variablen mit globalem Geltungsbereich sollten **nie** durch Nebeneffekte geändert werden. 
:::

::: {.callout-note}
**Objektorientierte Sprachen**, wie Python oder Java, verwenden Nebeneffekte als zentrales Programmierprinzip.

**Streng-funktionale Sprachen**, wie Excel, sind ***nebeneffektfrei***.
:::

## Bibliotheken

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

Ein Projekt wird mit `renv::init()` für die Verwendung des Packetmanagements vorbereitet. Beim ersten Aufruf von `renv` werden die internen Abhängigkeiten von `renv` kontrolliert und notfalls installiert. Das nimmt etwas Zeit in Anspruch. 

Das Packetmanagement erfasst automatisch alle Bibliotheken, die systemweit installiert wurden. Dadurch wird sichergestellt, dass alle Bibliotheken berücksichtigt wurden, die im eigenen System installiert sind und deshalb auch im Projekt verwendet werden können. Die Einzige Ausnahme davon ist `renv` selbst.

### Bibliotheken installieren

Nach der Initialisierung des Packetmanagements können projektspezifische Bibliotheken mit `renv::install()` installiert werden. War eine Installation erfolgreich, sollte die Bibliothek auf ihre Funktionstüchtigkeit mit einem einfachen Beispiel geprüft und danach mit `renv::snapshot()` als Abhängigkeit dokumentiert werden. Mit einem *Snapshot* wird eine Bibliotheksversion als Abhängigkeit registiert. Im Gegensatz zu `install.packages()` wird ab diesem Zeitpunkt nicht mehr eine beliebige Version der Bibliothek installiert, sondern nur die dokumentierte Version. Dadurch wird sichergestellt, dass der Code auch in anderen Umgebungen wie erwartet funktioniert. 

### Updates für Bibliotheken 

Eine Besonderheit von `renv` ist die Möglichkeit, kontrollierte Updates für einzelne oder alle Abhängigkeiten eines Projekts mit `renv::update()` durchzuführen. `renv::update()` installiert die neusten Versionen der Projektbibliotheken. 

::: {.callout-note}
## Merke
Updates sollten nie unklontrolliert akzeptiert werden!
:::

Bevor neue Bibliotheksversionen in das Packetmanagement aufgenommen werden, sollte immer geprüft werden, ob der bestehende Code mit den neuen Versionen immer noch funktioniert. Sollten bei dieser Prüfung Probleme auftreten, dann können die Updates mit `renv::revert()` wieder rückgängig gemacht werden. Gab es keine Probleme, dann können die Updates mit  `renv::snapshot()` übernommen werden. 
