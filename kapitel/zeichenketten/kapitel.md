---
abstract: ""

execute: 
  echo: false
---
# Zeichenketten {#sec-chapter-zeichenketten}

::: {.callout-warning}
## Work in Progress
:::


Bisher haben wir Zeichenketten als atomare Werte behandelt. In diesem Kapitel geht es die Operationen für Zeichenketten. 

Eine Zeichenkette hat eine Länge, die der Anzahl der Symbole in der Zeichenkette entspricht und jedes Symbol in einer Zeichenkette kann über dessen Position identifiziert werden.

Wenn Daten als Zeichenketten vorliegen, dann handelt es sich immer um **diskrete Daten**.

| Name | R |
|:---|:---|
| Länge | `str_length()` |
| Teilketten verbinden |  `str_c()` |
| Position einer Teilzeichenkette finden | `str_locate()`/`str_which()` |
| Teilkette finden (mit Wahrheitswert als Ergebnis) | `str_detect()` |
| In Grossbuchstaben umwandeln | `str_to_upper()` |
| In Kleinbuchstaben umwandeln   | `str_to_lower()` |
| Nur ersten Buchstaben als Grossbuchstabe  | `str_to_title()` |
| Leerzeichen bereinigen  | `str_squish()`/`str_trim()` |
| Nicht-druckbare Zeichen entfernen |  `str_replace_all(text, "[\u01-\u07\u0E-\u1f\u80-\u9F]+", "")` | 
| Teilkette extrahieren (linksseitig) |  `str_sub(text, 1, x)` |
| Teilkette extrahieren (rechtsseitig) | `str_sub(text, x, -1)` |
| Teilkette extrahieren (mittig) |  `str_split()`/`str_sub()` |
| Zeichenkette ersetzen | ``str_replace()``/``str_replace_all()`` |

: Die wichtigsten Zeichenketten-Operationen {#tbl-zeichenketten-opeartionen}

In R verwenden wir dazu die Funktion `str_replace_all()`. Diese Funktion ersetzt einen Teil einer Zeichenkette durch eine andere Zeichenkette. Wir müssen daher R mitteilen, dass wir alle Teilzeichenketten löschen möchten, die nicht-druckbare Zeichen enthalten. Das erreichen wir durch den ***regulären Ausdruck*** ``"[\u01-\u07\u0E-\u1f\u80-\u9F]+"``. Dieses Suchmuster teilt R mit, welche nicht-druckbaren Zeichen entfernt werden müssen. Das Löschen erreichen wir dadurch, dass wir eine Teilzeichenkette mit der leeren Zeichenkette (s.u.) ersetzen. 

## Einzelne Symbole aus einer Zeichenkette extrahieren

In **R** lassen sich die  einzelnen Symbole einer Zeichenkette mit der Operation ``zeichenkette |> str_extract("")`` extrahieren. 

::: {.callout-warning}
Wenn Sie die Zeichenketten in einem Zeichenkettenvektor in die einzelnen Symbole zerlegen möchten, dann erhalten Sie für jede Zeichenkette einen eigenen Vektor, der die Symbole der Zeichenkette enthält. R kann diese Vektoren nicht einfach zu einem grossen Vektor zusammensetzen. Daher werden die Ergebnisse als Listen geschützt und zu einem Ergebnisvektor zusammengefasst. 

In einem zweiten Schritt können die extrahierten Symbole mit `unnest()` in der Stichprobe erweitert werden.  
:::

::: {#exm-zeichenketten-zerlegen}
## Zeichenkettenvektor in die einzelnen Symbole zerlegen

```R
tibble(zeichenkette = c("Daten", "und", "Information")) |> 
    mutate(
        symbol = zeichenkette |> str_extract_all("") 
    ) |> 
    unnest(symbol)
```

| zeichenkette<br> &lt;chr&gt; | symbol<br> &lt;chr&gt; |
|---|---| 
| Daten | D |
| Daten | a |
| Daten | t |
| Daten | e |
| Daten | n |
| und | u |
| und | n |
| und | d |
| Information | I |
| Information | n |
| Information | f |
| Information | o |
| Information | r |
| Information | m |
| Information | a |
| Information | t |
| Information | i | 
| Information | o |
| Information | n |
:::


Bei solchen Operationen sollten Sie die Quelldaten nicht überschreiben. Erstellen Sie immer einen neuen Vektor für extrahierte Symbole und Zeichenketten. So bleibt der Bezug zu den ursprünglichen Werten erhalten. 

## Nicht-druckbare Zeichen

::: {#def-nicht-druckbare-zeichen}
Als **nicht-druckbare Zeichen** werden Symbole bezeichnet, die bei der Darstellung einer Zeichenkette nicht angezeigt werden. Die nicht-druckbaren Zeichen zählen zur Länge einer Zeichenkette und verändern den Inhalt einer Zeichenkette.
:::

Beispiel: Die Zeichenkette `Hallo` unterscheidet sich von der Zeichenkette `Hal<0x08>lo`. 

Excel und R behandeln nicht-druckbare Zeichen unterschiedlich. In Excel werden die nicht-druckbaren Zeichen für die  Darstellung und für Vergleiche entfernt, jedoch werden die nicht-druckbaren Zeichen bei der Länge und beim Extrahieren berücksichtigt. In R werden nicht-druckbare Zeichen bei der Darstellung und bei Vergleichen berücksichtigt. In Excel können wir mit der `IDENTISCH()`-Funktion zwei Zeichenketten nach den gleichen Regeln wie in R vergleichen.

Zu den nicht-druckbaren Zeichen gehören auch Leerzeichen, Tabulatoren und Zeilenumbrüche. Wir können diese speziellen nicht-druckbaren Zeichen nur erkennen, wenn sie von druckbaren Zeichen umgeben sind.

Deutlich wird das an den folgenden Zeichenketten: 

* `Hallo`
* `Hal<0x07>lo`, wobei das Symbol `0x07` für einen Piepton steht
* `Hal<0x08>lo`, wobei das Symbol `0x08` für einmal Rückwärtslöschen steht.

Diese drei Zeichenketten haben in Excel und R die Längen 5, 6 und 6. Excel stellt alle drei Zeichenketten als "Hallo" dar. Ausserdem werden die Zeichenketten als gleich ausgewertet. R wertet die Zeichenketten aus und stellt nicht-druckbare Zeichen prinzipiell als ein Leerzeichen dar. Das Symbol `0x08` wird von R ausgewertet und deshalb wird das vorangehende Symbol gelöscht. Entsprechend wird in unserem Fall `Halo` angezeigt. Ebenfalls werden alle drei Zeichenketten in R als ungleich ausgewertet.

## Die leere Zeichenkette

Ein besonderer Fall ist die *leere Zeichenkette*. Die leere Zeichenkette wird oft als Platzhalter genutzt. Die leere Zeichenkette ist das *neutrale Element* für die Verknüpfung von Zeichenketten.

Die leere Zeichenkette wird in R immer durch doppelte Anführungszeichen eingerahmt. Soll eine leere Zeichenkette als  Wert in einer Zelle eingegeben werden, dann ist ein einfacher Apostroph (`) einzugeben. 

::: {.callout-note}
In R dürfen Sie *optional* auch einfache Anführungszeichen als alternative Zeichenkettenmarkierungen verwenden. Weil das einfache Anführungszeichen (') und der Backtick (`` ` ``) sehr ähnlich aussehen aber eine andere Bedeutung haben, sollte nur das doppelte Anführungszeichen (`"`) in R verwendet werden. 
::: 


::: {#exm-leere-zeichenkette}
## Die leere Zeichenkette
```r
leereZeichenkette = ""
```
:::


### Schreibweise ändern

## Zeichenketten trennen

In R gibt es verschiedene Funktionen, die aus einem Wert mehrere Werte erzeugen. Das gilt insbesondere für die Zeichenkettenfunktionen. In diesem Zusammenhang nimmt die Funktion ``str_split()`` eine besondere Position ein, weil sie relativ oft gebraucht wird. Diese Funktion trennt eine Zeichenkette entlang eines Trennzeichens bzw. eines Trennmusters und gibt die Ergebniswerte zurück.

::: {.callout-note}
Wir können uns die Funktion `str_split()` als eine flexiblere Variante von Excels *"Text in Spalten"*-Befehl vorstellen. Die Parameter für diese Funktion sind eine Zeichenkette sowie das Trennmuster. Das Ergebnis ist ein Vektor aus Zeichenketten.
:::

Natürlich wäre es toll, wenn wir `str_split()` zum Umformen einer Stichprobe verwenden könnten. Allerdings führt das Ergebnis zu  *Listenvektoren*, mit denen wir nicht leicht arbeiten können. Das illustriert das folgende Beispiel. In diesem Beispiel trennen wir die Zeichenketten im Vektor `text` an den Leerzeichen, sodass wir einzelne Worte erhalten. 

::: {#exm-str_split}
## Zeichenketten mit `str_split()` trennen
```R
library(tidyverse)

texte = tibble(text = c("Daten und Information", "Klimatologie Informatik"))

texte |> 
    mutate(
        getrennter_text = text |> str_split(" ")
    )
```

| text <br> `<chr>` | getrennter_text <br>`<list>` |
|---|---|
| Daten und Information   | Daten, und, Information |
| Klimatologie Informatik | Klimatologie, Informatik   |
:::

Im Vektor `getrennter_text` stehen nun Listen mit unterschiedlicher Länge. Wären diese Listen Vektoren, dann könnten wir mit der Funktion `pivot_longer()` die Werte transponieren. Das funktioniert mit eingebetteten Listen leider nicht, weil die Werte nicht über mehrere Vektoren verteilt sind, sondern alle im gleichen Vektor stehen.

::: {#def-eingebettete-werte}
Enthält ein Vektor Listen mit Werten, dann werden die Listenwerte als **eingebettete** (engl. *nested*) **Werte** bezeichnet. 
::: 

Um an eingebettete Werte zu gelangen, müssen wir sie zuerst "ausbetten". Dazu verwenden wir die Funktion `unnest()`. Mit dieser Funktion werden eingebette Listen in einen Vektor entpackt. 

::: {#exm-trennung-entpacken}
## Entpacken von getrennten Zeichenketten mit `unnest()`
```R
texte |> 
    mutate(
        getrennter_text = text |> str_split(" ")
    ) |> 
    unnest(getrennter_text) -> texte_getrennt

texte_getrennt
```

| text <br> `<chr>` | getrennter_text <br>`<chr>` |
|---|---|
| Daten und Information   | Daten |
| Daten und Information   | und |
| Daten und Information   | Information |
| Klimatologie Informatik | Klimatologie   |
| Klimatologie Informatik |  Informatik   |
:::

Beachten Sie hier, dass alle nicht aufgelösten Vektoren für jeden Listeneintrag erweitert werden. 

Jetzt können wir mit diesen Werten wie gewohnt weiterarbeiten. 

::: {.callout-note}
Die Umkehrfunktion von `unnest()` ist die Funktion `nest()`. Beide Funktionen werden ausführlich in @sec-chapter-daten-formen behandelt.
:::

Das @exm-trennung-entpacken können wir mit der folgenden Operation zurück in die Listenform bringen. Dabei beachten wir, dass wir den neuen Vektor benennen und diesem die Werte aus dem Ursprungsvektor übergeben müssen.

::: {#exm-renesting-zeichenketten}
## Zeichenketten einbetten
```R
texte_getrennt |> 
    nest(getrennter_text = getrennter_text)
```

| text <br> `<chr>` | getrennter_text <br>`<list>` |
|---|---|
| Daten und Information   | Daten, und, Information |
| Klimatologie Informatik | Klimatologie, Informatik   |
:::

## Teilzeichenketten extrahieren


## Suchen und Ersetzen

### Position einer Teilzeichenkette finden

### Teilzeichenketten austauschen

## Mustererkennung
::: {#def-reguaerer-ausdruck}
**Reguläre Ausdrücke** sind Zeichenketten, mit denen *komplexe Suchmuster* für Zeichenketten beschrieben werden.
::: 

Ein regulärer Ausdruck beschreibt die Struktur einer gesuchten Zeichenkette. Reguläre Ausdrücke sind eine Standardtechnik zum Suchen-und-Ersetzen.

R verfügt mit regulären Ausdrücken über eine leistungsfähige Mustererkennung für Zeichenketten. Diese Mustererkennung steuern wir über *reguläre Ausdrücke*([Vignette regex](https://stringr.tidyverse.org/articles/regular-expressions.html)) oder *Regulärausdruck* (Sauer, 2019). Reguläre Ausdrücke erlauben es, Muster in Zeichenketten zu finden und diese Muster durch etwas anderes auszutauschen. In R schreiben wir reguläre Ausdrücke als Zeichenketten mit einer besonderen Musterbeschreibungssprache. Wir können an viele Zeichenkettenfunktionen solche regulären Ausdrücke als Parameter übergeben. 

Die wichtigsten Symbole zur Musterbeschreibung mit regulären Ausdrücken sind die folgenden Symbole und Symbolkombinationen: 

* `.` - beschreibt das Vorkommen eines beliebigen Symbols
* `\s` - beschreibt alle Symbole die als Leerzeichen gelten
* `\d` - beschreibt alle Ziffern
* `\w` - beschreibt alle Buchstaben unabhängig von der Gross- und Kleinschreibung
* `*` - beschreibt das Auftreten von Sequenzen von 0 oder mehreren der voranstehenden Symbole
* `+` - beschreibt das Auftreten von Sequenzen von 1 oder mehreren der voranstehenden Symbole
* `?` - beschreibt das Auftreten von Sequenzen  von 0 oder 1 des voranstehenden Symbols
* `{}` - beschreibt das Auftreten von Sequenzen der angegebenen Länge des voranstehenden Symbols
* `^` - steht für den Anfang der Zeichenkette
* `$` - steht für das Ende der Zeichenkette
* `[]` - "Symbolbereich": Die Symbole zwischen den beiden Klammern beschreiben die möglichen Symbole an der Position in der Zeichenkette
* `()` - Gruppiert eine Teilzeichenkette

### Normale Zeichen in Mustern

Normale Buchstaben oder Ziffern haben keine besondere Bedeutung und bedeuten, dass an der entsprechenden Stelle das jeweilige Symbol vorkommen muss.

```R
zeichenkettenVektor = c( "Daten und Information", "Datenverarbeitung", "Informatik", "Daten Information", "Computation Daten Informatik" )

# der reguläre Ausdruck wäre eigentlich "\w\s\w" die zusätzlichen Backslashs 
# zeigen R an, dass wir den Backslash in unserem Muster haben möchten.

regulaererAusdruck = "Daten In"

zeichenkettenVektor |> str_detect(regulaererAusdruck) 
# erzeugt c(FALSE FALSE FALSE TRUE TRUE)
```

### Mustersymbole in R verwenden

Wenn wir ein Symbol in unserem Muster aufnehmen wollen, das normalerweise ein besonderes Symbol für reguläre Ausdrücke ist, dann müssen wir diesem Symbol einen *Backslash* voranstellen. Dieses Voranstellen wird als **"Escaping"** bezeichnet. Weil R reguläre Ausdrücke als Zeichenketten behandelt, müssen wir aufpassen, denn der Backslash ist auch ein reserviertes Symbol in Zeichenketten. Deshalb ist der zweite Backslash notwendig, um den ersten Backslash vor der Zeichenketteninterpretation zu schützen. 

```R
zeichenkette = "Daten und Information"

# der reguläre Ausdruck wäre eigentlich "\w\s\w" die zusätzlichen Backslashs 
# zeigen R an, dass wir den Backslash in unserem Muster haben möchten.

regulaererAusdruck = "\\w\\s\\w"

zeichenkette |> str_replace(regulaererAusdruck, "p x") # erzeugt "Datep xnd Information"

# Hinweis, um alle Vorkommnisse des Musters auszutauschen, müssen wir 
# str_replace_all() verwenden!
```

### Multiplikatoren

Die Symbole `*`, `+`, `?` und `{}` werden als *Multiplikatoren* bezeichnet. So können Wiederholungen in Mustern abgebildet werden. 

Mit diesen Elementen können wir Zeichenketten beschreiben, ohne die genaue Abfolge der Symbole zu kennen.

Beispiele: 

```
"ab"       # erkennt ab
"a?b"      # erkennt b und ab
"a*b"      # erkennt b, ab, aab, aaab, aaaab usw. 
"a+b"      # erkennt ab, aab, aaab, aaaab usw. 
"a{2}b"    # erkennt aab
"a{2,4}b"  # erkennt aab, aaab und aaaab
"a.b"      # erkennt aab, acb, adb, a3b, a-b usw. 
"a.*b"     # erkennt ab, acb, acdb, a-!%b usw. 

"a\\sb"    # erkennt "a b" oder "a     b" (Achtung doppelter Backslash!)
"\\w\\d"   # erkennt einen Buchstaben, der von einer Ziffer gefolgt wird  (Achtung doppelter Backslash!)
"a[cd]?b"  # erkennt ab, acb und adb

"ab$"      # erkennt ab nur am Ende der Zeichenkette
"^ab"      # erkennt ab nur am Anfang der Zeichenkette
```

Gelegentlich wollen wir ein Muster bis zu einem bestimmten Symbol in unserer Zeichenkette finden. In diesem Fall können wir einen negierten Symbolbereich angeben.

> ::: {#exm-regex-finden}
> Gegeben ist die folgende Zeichenkette: 
> 
> ```r
> aquaponics = "
>    The term aquaponics [7] is coined by combining 
>     two words: aquaculture, which refers to fish 
>     farming, and hydroponics—the technique of growing 
>     plants without soil.[16]"
> ```
>
> Wir möchten nun die Zeichenkette ab dem Wort `term` und der öffnenden eckigen Klammer der Referenz markieren. D.h. wir wollen nicht ein beliebiges Zeichen und wollen nicht alle Symbole bis auf die öffnende Klammer explizit ausschliessen. Stattdessen können wir einen *negierten* Symbolbereich angeben. In unserem Fall erlauben wir jedes Zeichen, ausser die öffnende eckige Klammer. Weil die eckige Klammer eine besondere Bedeutung für reguläre Ausdrücke hat, müssen wir sie entsprechend mit Backslash "escapen". Unser regulärer Ausdruck muss entsprechend `"termn [^\\[]+\\["`. Der Teil `[^\\[]+` bedeutet dabei, "*alle Symbole ausser der öffnenden eckigen Klammer `[`*". Die beiden Backslashes sind dabei die notwendige Escape-Sequenz, um die Klammer vom Symbolbereich zu unterscheiden. 
> 
> Der folgende Code demonstriert diesen regulären Ausdruck.
> 
> ```r
> aquaponics |>
>     str_match("term [^\\[]+\\[")
> ```
> :::

Das Ergebnis eines regulären Ausdrucks ist normalerweise immer nur der erste Treffer. 

```r
"term aquaponics ["
```

Um alle Treffer eines Musters zu erhalten, muss für die entsprechende `_all`-Variante der Funktion verwendet werden. Die folgenden Funktionen haben eine solche Funktionsvariante:

| Bedeutung | Erster Treffer | Alle Treffer |
| :-------- | :--- | :--- |
| Finde und extrahiere ein Suchmuster | `str_extract()` | `str_extract_all()` | 
| Finde ein Suchmuster und gebe die Zeichenkette zurück | `str_match()` |`str_match_all()` |
| Finde ein Suchmuster und gebe die Position des Treffers zurück | `str_locate()` | `str_locate_all()` |
| Finde und lösche ein Suchmuster  | `str_remove()` | `str_remove_all()` |
| Finde ein Suchmuster und ersetze den Treffer durch eine andere Zeichenkette  | `str_replace()` | `str_replace_all()` |
| Zeige Suchtreffer für ein Suchmuster an | `str_view()` | `str_view_all()` |

## Tokens

Die Bibliothek `tidytext` stellt viele hilfreiche Funktionen zur quantitativen Textanalyse bereit. Beim Tokenisieren müssen verschiedene Regeln beachtet werden, damit das richtige Ergebnis erzeugt wird. Diese Regeln müssen wir zum Glück nicht im Detail kennen.   Die Bibliothek `tidytext` stellt uns die Funktion `unnest_tokens()` sowie ein paar Hilfsfunktionen bereit. Mit diesen Funktionen können wir Texte leicht in Tokens zerlegen. 

<!-- Wir laden für die folgenden Beispiele die notwendigen Funktionen und Daten aus der Datei `text/marketing_4.txt` aus den [Beispieltexten](https://moodle.zhaw.ch/mod/resource/view.php?id=703515): -->

```r
library(tidyverse)
library(tidytext)

tibble(
    rohtext = read_file("text/marketing_4.txt")
) -> Rohdaten 
```

Hier fällt uns eine neue Funktion auf: `read_file()`. Mit dieser Funktion können beliebige Textdateien eingelesen werden. R versucht bei dieser Funktion nicht, strukturierte Daten zu finden. Stattdessen wird der gesamte Inhalt der Datei als Zeichenkette zurückgegeben. 

Nun können wir die Daten mit Hilfe von `unnest_tokens()` in Tokens zerlegen. Die Funktion `unnest_tokens()` funktioniert analog zu `str_split()` gefolgt von `unnest()`. Sie erleichtert diese Funktionsfolge, indem sie nicht nur Leerzeichen, sondern auch alle anderen Symbole und Satzzeichen, die keine Worte darstellen aus der ursprünglichen Zeichenkette entfernt. 

```r
Rohdaten |> 
    unnest_tokens(worte, rohtext)  |> 
    head()
```

| worte<br>&lt;chr&gt; | 
| --- |
| neues |
| lehrbuch |
| für |
| modernes |
| marketing |
| marketing |

In diesem Beispiel sehen wir die grundsätzliche Arbeitsweise der Funktion. Wir übergeben ein Stichprobenobjekt der Funktion über die Funktionsverkettung. Anschliessend übergeben wir der Funktion als ersten Parameter den Namen des Zielvektors (hier: `worte`) und als zweiten Parameter den Namen des Quellvektors (hier `rohtext`). Alle Texte im Vektor `rohtext` werden durch die Funktion in Worte zerlegt und anschliessend konsequent in die Kleinschreibung überführt. Abschliessend wird der ursprüngliche Vektor `rohtext` aus der Ergebnisstichprobe entfernt. 

### Deutsche Gross- und Kleinschreibung

Während die Gross- und Kleinschreibung im Englischen (mit Ausnahme von Eigennamen) nicht signifikant ist, haben deutsche Worte in Gross- und Kleinschreibung eine andere inhaltliche Bedeutung. Falls diese Bedeutung erhalten bleiben soll, kann der optionale Parameter `to_lower = FALSE` übergeben werden. Der Code ändert sich dann wie folgt: 

```r
Rohdaten |> 
    unnest_tokens(worte, rohtext, to_lower = FALSE)  |> 
    head()
```

| worte<br>&lt;chr&gt; | 
| --- |
| Neues |
| Lehrbuch |
| für |
| modernes |
| Marketing |
| Marketing |

### Texte in Sätze zerlegen

Wenn wir Texte in Sätze zerlegen wollen, dann verwenden wir die Funktion `unnest_sentences()`.

```r
Rohdaten |> 
    unnest_sentences(saetze, rohtext, to_lower = FALSE)
```

| saetze <br>&lt;chr&gt; |
| --- | 
| Neues Lehrbuch für modernes Marketing. | 
| Marketing wandelt sich im rasanten Tempo. | 
| Auf Kundenwünsche oder neue Technologien muss nicht nur auf der operativen, sondern auch auf der strategischen Ebene in nahezu Echtzeit reagiert werden. | 
| Welche Instrumente und Frameworks dem Marketing dabei zur Verfügung stehen, zeigt das gerade erschienene Lehrbuch des Instituts für Marketing Management: "Marketingmanagement. | 
| Building and Running the Business. | 
| Mit Marketing Unternehmen transformieren" Marketing hat in den vergangenen Jahren einen Paradigmenwechsel durchlaufen. |

Dieser Code ist übrigens identisch mit dem folgenden Code: 

```r
Rohdaten |> 
    unnest_tokens(saetze, rohtext, to_lower = FALSE, token = "sentences")
```

In diesem Beispiel fällt auf, dass `unnest_sentences()` streng entlang den Satztrennzeichen (`. ! ?`) trennt und in Anführungszeichen eingebettete Sätze ebenfalls trennt. Dieses Verhalten kann nur dadurch beeinflusst werden, dass die Texte im Vorfeld entsprechend vorbereitet werden. In solchen Fällen empfiehlt es sich, eingebettete Satzenden durch ein Semikolon (`;`) zu ersetzen. 

In anderen Fällen wollen wir nach dem Trennen die Satzzeichen aus den Sätzen vollständig entfernen. Das erreichen wir mit dem Parameter `strip_punct = TRUE`. Dieser Parameter veranlasst, dass alle Satzzeichen aus den Sätzen entfernt werden. Dazu gehört auch das Zeichen für das Satzende. 

### Absätze trennen

Damit wir Absätze trennen können, müssen die Rohtexte entsprechend vorbereitet sein. Absätze werden in Textformaten durch eine zusätzliche Leerzeile markiert. Das weicht von der üblichen Vorgehensweise bei der Arbeit mit Word ab. Dort markiert der einfache Zeilenumbruch einen Absatz.

::: {.callout-tip}
## Praxis 
Trennen Sie beim Transkribieren mit MS Word Absätze **immer** mit einer zusätzlichen Leerzeile. In dieser Leerzeile dürfen **keine** anderen Symbole stehen (auch keine Leerschläge). Diese Zeile erzeugen Sie durch zwei Zeilenumbrüche mit der Eingabetaste. Sie halten sich so alle Optionen für die nachfolgende Analyse offen.
::: 

Sind die Textdaten entsprechend vorbereitet, dann können wir unsere Texte mit der Funktion `unnest_paragraphs()` in  Absätze gliedern. 

```r
Rohdaten |> 
    unnest_paragraphs(
        saetze, 
        rohtext, 
        to_lower = FALSE, 
        paragraph_break = "\r\n\r\n"
   )  
```

::: {.callout-warning}
Der zusätzliche Parameter `paragraph_break = "\r\n\r\n"` ist hier notwendig, weil die Daten aus Wort heraus als `Nur Text (.txt)` gespeichert wurden.
:::

### n-Gramme extrahieren

n-Gramme sind ein wichtiges Werkzeug für einen besseren inhaltlichen Überblick. Worte sind spezielle n-Gramme mit $n = 1$. Bei dieser Länge haben bestimmte häufig vorkommende Worte (die Stoppworte) die unerwünschte Eigenschaft, das Ergebnis inhaltlich zu verzerren. Durch das Entfernen dieser Worte wird aber immer auch ein Teil der inhaltlichen Bedeutung entfernt. Dieser besondere Effekt von Stoppworten tritt bei n-Grammen mit $n > 1$ nicht auf. 

n-Gramme extrahieren wir mit der Funktion `unnest_ngrams()`. Dabei wird der Text in Wortsequenzen mit der Länge `n` gegliedert. 

```r
Rohdaten |> 
    unnest_ngrams(ngram, rohtext, to_lower = FALSE) |> 
    head()
``` 

| ngram<br>&lt;chr&gt; |
| --- |
| Neues Lehrbuch für |
| Lehrbuch für modernes |
| für modernes Marketing |
| modernes Marketing Marketing |
| Marketing Marketing wandelt |
| Marketing wandelt sich |


Dieser Code ist identisch mit dem folgenden Code: 

```r
Rohdaten |> 
    unnest_ngrams(ngram, rohtext, to_lower = FALSE, n = 3) |> 
    head()
``` 

::: {.callout-tip}
## Praxis
Typische n-Gram-Längen sind `3`, `5` oder `7`.

Wobei die n-Gram-Länge von `3` am üblichsten ist. 
:::


Am Beispiel ist die Arbeitsweise von n-Gram-Tokenisierung erkennbar: Beginnend vom ersten Wort wird eine Sequenz von `n` Worten extrahiert und so lange ein Wort weiter gegangen bis keine Wortsequenz der Länge `n` mehr möglich ist. Dabei ist zu beachten, dass Satz- und Zeilengrenzen nicht automatisch berücksichtigt werden. Um nur inhaltlich zusammenhängende n-Gramme zu erhalten, müssen zwei Tokenisierungen nacheinander vorgenommen werden. Das folgende Beispiel zeigt eine 3-Gram-Zerlegung auf Satzebene. 

```r
Rohtext |> 
    unnest_sentences(saetze, rohtext, to_lower = FALSE)  |> 
    unnest_ngrams(ngram, saetze, to_lower = FALSE)  |> 
    head()
```

| ngram<br>&lt;chr&gt; |
| --- |
| Neues Lehrbuch für |
| Lehrbuch für modernes |
| für modernes Marketing |
| Marketing wandelt sich |
| wandelt sich im |
| sich im rasanten |



## Rezepte

### Word als Datenquelle

Kodierte Texte sind keine Tabellen, sondern liegen als Word-Dokumente auf unserem Rechner. In diesen Word-Dokumenten sind unsere Daten Paare von markierten Textstellen und Kommentaren . Diese Paare wollen wir extrahieren.

Dieses Beispiel hat vier Beiträge vom offiziellen Blog des Studienschwerpunkts Marketing der ZHAW kopiert und kodiert. Dabei wurden Bilder entfernt. Mit diesem Beispiel untersuchen wir, ob diese Beiträge eine genderneutrale Sprache verwenden. Dazu wurden Substantive jeweils kategorisiert und mit der jeweiligen Geschlechtlichkeit (`feminin`, `maskulin`, `neutral`) kodiert. 

#### Schritt 1: Datei einlesen und bereinigen

Wenn wir unsere Texte mit Word kodiert haben, können wir sie mit Hilfe der `docxtractr` Bibliothek einlesen. 

```
library(tidyverse)
library(docxtractr)
```

Nun können wir kodierte Dokumente in unsere R-Umgebung importieren. Dazu verwenden wir die besondere Funktion `read_docx()`. Diese Funktion liest das ganze Word-Dokument ein. Mit Hilfe der Funktion `docx_extract_all_cmnts()` sammeln wir unsere markierten Textstellen ein. 

```r
read_docx("kodiert/marketing_1.docx") |> 
    docx_extract_all_cmnts(include_text = TRUE) -> documentCodes
```

Drei Vektoren sind für uns von besonderer Bedeutung: 

- Der Vektor `comment_text` enthält nun unsere Kodierungen. 
- Der Vektor `id` zeigt uns die Reihenfolge der Kommentare im Dokument.
- Der Vektor `word_src` enthält den beim Kodieren markierten Text. 

Oft müssen wir unsere Codes und Texte noch bereinigen. In diesem Fall, sind die Codes nicht durchgehend einheitlich geschrieben und in jedem Kommentar stehen zwei Codes. Wir wollen deshalb die Codes trennen und vereinheitlichen. In unserem Fall sind enthalten die Kommentartexte *immer* zwei Kodierungen. Weil immer die gleiche Kode-Anzahl in den Kommentaren vorliegt, können wir die Funktion `separate()` verwenden. 

::: {.callout-tip}
Die Funktion `separate()` trennt einen Zeichenketten Vektor in mehrere Zeichenkettenvektoren auf.
:::

::: {.callout-important}
Die `separate()`-Funktion darf nur verwenden werden, wenn alle Kommentare die gleiche Anzahl von Codes enthalten!
::: 

```r
documentCodes |>
    select(id, comment_text, word_src) |> 
    separate(comment_text, into =c("kategorie", "gender"), sep =",") |>
    mutate(
        gender = gender |> str_trim() |> str_to_lower(),
        kategorie = kategorie |> str_trim() |> str_to_lower()
    ) -> kodierteDaten
```

Nach diesem Schritt sind unsere Codes vereinheitlicht und für jede Textstelle können nun die Codes bearbeitet werden. 

Falls wir unterschiedlich viele Kodierungen in den Kommentaren vorliegen, müssen wir die Funktion `str_split()`  und anschliessend `unnest()` verwenden. 

```r
data |> 
    select(id, comment_text, word_src) |> 
    mutate(
        code = comment_text |> str_split(",")
    ) |> 
    unnest(code) -> allgemeinereExtraktion
```

#### Schritt 2: Word Hyperlinks entfernen

In diesem Beispiel enthalten die markierten Texte Hyperlinks zu externen Seiten. Diese Links stören uns bei  der Analyse und deshalb entfernen wir sie aus den Textstellen.

```r
kodierteDaten |>
    mutate(
        word_src = word_src |> str_remove("HY\\s?\\S+ \"[^\"]+\"[^\"]+\"[^\"]+\" "),
    ) -> kodierteDaten2
```

#### Schritt 3: Kategorien organisieren

```r
kodierteDaten2 |> 
    count(kategorie)
```

| kategorie | count | 
| --- | --- |
| aktivität | 7 |
| aufgabe |	1 |
| cas |	10 |
| eigenschaft |	4 |
| fähigkeit |	1 |
| funktion |	5 |
| gruppe |	12 |
| information |	1 |
| kontext |	1 |
| konzept |	1 |
| organisation |	1 |
| person |	6 |
| produkt |	3 |
| technik |	1 |
| zeit |	7 | 


Beachten Sie, dass der vorherige Teilschritt in der Regel **nicht** berichtet wird, weil das Ergebnis nur dazu dient, die Codes den richtigen Variablen zuzuordnen. 

Die Codes im Vektor `Kategorie` gehören zu verschiedenen Variablen. Diese Zuordnung muss explizit erfasst  werden.

```r
Akteure = c("person", "organisation", "gruppe")

Studiengang = c("cas", "bsc", "mas", "msc")

Funktion = c("aktivität", "eigenschaft", "fähigkeit",
            "möglichkeit", "funktion", "produkt", "anwendung")

Kontext = c("kontext", "konzept", "technik", "zeit", "information")
```

Wir können so die Kodierungen als Merkmalsausprägungen nominalskalierter Variablen verwenden, beschreiben und auswerten. 


#### Weiterführende Textanalyse

Wir können nun weiter analysieren und besonders häufige Worte für unsere Codes auswerten.

Wir extrahieren die einzelnen Worte aus der jeweiligen Markierung und entfernen Artikel und andere oft benutzte Worte aus unseren Daten. 

Anschliessend zählen wir die verbleibenden Worte nach Kategorien und Gender.

Damit das Ergebnis einfacher zu lesen ist, wird das Ergebnis mit `arrange()` sortiert. Weil in unserem Fall sehr viele Worte nur einmal vorkommen, tragen diese nicht viel zum Gesamtinhalt bei. Daher werden diese Worte mit dem abschliessenden Filter für die Darstellung entfernt. 

```r
library(tidytext)

stopwords_DE = tibble(
    word = stopwords::stopwords("de", source = "stopwords-iso")
)

kodierteDaten2 |> 
    unnest_tokens(word, word_src) |>
    anti_join(stopwords_DE) |> 
    count(word, kategorie, gender) |> 
    arrange(desc(n)) |> 
    filter(n > 1)
```

| word | kategorie | gender | n |
| --- |  --- |  --- |  --- | 
| cas | cas | maskulin | 9 |
| marketing | funktion | neutral | 3 |
| mitstudierenden | gruppe | neutral | 3 |
| austausch | aktivität | maskulin | 2 |
| digital | cas | maskulin | 2 |
| marketing | cas | maskulin | 2 |
| netzwerk | gruppe | neutral | 2 |
| npo | cas | maskulin | 2 |
| referenten | gruppe | maskulin | 2 |

Solche Auswertungen geben zusätzliche Einblicke in die Inhalte und helfen bei der Interpretation der Daten. 

### Word als Datenquelle

Kodierte Texte sind keine Tabellen, sondern liegen als Word-Dokumente auf unserem Rechner. In diesen Word-Dokumenten sind unsere Daten Paare von markierten Textstellen und Kommentaren . Diese Paare wollen wir extrahieren.

Dieses Beispiel hat vier Beiträge vom offiziellen Blog des Studienschwerpunkts Marketing der ZHAW kopiert und in MS Word kodiert. Dabei wurden Bilder entfernt. Mit diesem Beispiel untersuchen wir, ob diese Beiträge eine genderneutrale Sprache verwenden. Dazu wurden Substantive jeweils kategorisiert und mit der jeweiligen Geschlechtlichkeit (`feminin`, `maskulin`, `neutral`) kodiert. 

#### Schritt 1: Datei einlesen und bereinigen

Wenn wir unsere Texte mit Word kodiert haben, können wir sie mit Hilfe der `docxtractr` Bibliothek einlesen. 

```r
library(tidyverse)
library(docxtractr)
```

Nun können wir kodierte Dokumente in unsere R-Umgebung importieren. Dazu verwenden wir die besondere Funktion `read_docx()`. Diese Funktion liest das ganze Word-Dokument ein. Mit Hilfe der Funktion `docx_extract_all_cmnts()` sammeln wir unsere markierten Textstellen ein. 

```r
read_docx("kodiert/marketing_1.docx") |> 
    docx_extract_all_cmnts(include_text = TRUE) -> documentCodes
```

Drei Vektoren sind für uns von besonderer Bedeutung: 

- Der Vektor `comment_text` enthält nun unsere Kodierungen. 
- Der Vektor `id` zeigt uns die Reihenfolge der Kommentare im Dokument.
- Der Vektor `word_src` enthält den beim Kodieren markierten Text. 

Oft müssen wir unsere Codes und Texte noch bereinigen. In diesem Fall, sind die Codes nicht durchgehend einheitlich geschrieben und in jedem Kommentar stehen zwei Codes. Wir wollen deshalb die Codes trennen und vereinheitlichen. In unserem Fall sind enthalten die Kommentartexte *immer* zwei Kodierungen. Weil immer die gleiche Kode-Anzahl in den Kommentaren vorliegt, können wir die Funktion `separate()` verwenden. 

::: {.callout-tip}
Die Funktion `separate()` trennt einen Zeichenketten Vektor in mehrere Zeichenkettenvektoren auf.
:::

::: {.callout-warning}
Die `separate()`-Funktion darf nur verwenden werden, wenn alle Kommentare die gleiche Anzahl von Codes enthalten!
:::

```r
documentCodes |>
    select(id, comment_text, word_src) |> 
    separate(comment_text, into =c("kategorie", "gender"), sep =",") |>
    mutate(
        gender = gender |> str_trim() |> str_to_lower(),
        kategorie = kategorie |> str_trim() |> str_to_lower()
    ) -> kodierteDaten
```

Nach diesem Schritt sind unsere Codes vereinheitlicht und für jede Textstelle können nun die Codes bearbeitet werden. 

Falls wir unterschiedlich viele Kodierungen in den Kommentaren vorliegen, müssen wir die Funktion `str_split()`  und anschliessend `unnest()` verwenden. 

```r
data |> 
    select(id, comment_text, word_src) |> 
    mutate(
        code = comment_text |> str_split(",")
    ) |> 
    unnest(code) -> allgemeinereExtraktion
```

#### Schritt 2: Word Hyperlinks entfernen

In diesem Beispiel enthalten die markierten Texte Hyperlinks zu externen Seiten. Diese Links stören uns bei  der Analyse und deshalb entfernen wir sie aus den Textstellen.

```r
kodierteDaten |>
    mutate(
        word_src = word_src |> str_remove("HY\\s?\\S+ \"[^\"]+\"[^\"]+\"[^\"]+\" "),
    ) -> kodierteDaten2
```

#### Schritt 3: Kategorien organisieren

```r
kodierteDaten2 |> 
    count(kategorie)
```

| kategorie | count | 
| --- | --- |
| aktivität | 7 |
| aufgabe |	1 |
| cas |	10 |
| eigenschaft |	4 |
| fähigkeit |	1 |
| funktion |	5 |
| gruppe |	12 |
| information |	1 |
| kontext |	1 |
| konzept |	1 |
| organisation |	1 |
| person |	6 |
| produkt |	3 |
| technik |	1 |
| zeit |	7 | 


Beachten Sie, dass der vorherige Teilschritt in der Regel **nicht** berichtet wird, weil das Ergebnis nur dazu dient, die Codes den richtigen Variablen zuzuordnen. 
Die Codes im Vektor `Kategorie` gehören zu verschiedenen Variablen. Diese Zuordnung muss explizit erfasst  werden.

```r
Akteure = c("person", "organisation", "gruppe")

Studiengang = c("cas", "bsc", "mas", "msc")

Funktion = c("aktivität", "eigenschaft", "fähigkeit",
            "möglichkeit", "funktion", "produkt", "anwendung")

Kontext = c("kontext", "konzept", "technik", "zeit", "information")
```

Wir können so die Kodierungen als Merkmalsausprägungen nominalskalierter Variablen verwenden, beschreiben und auswerten. 


#### Weiterführende Textanalyse

Wir können nun weiter analysieren und besonders häufige Worte für unsere Codes auswerten.

Wir extrahieren die einzelnen Worte aus der jeweiligen Markierung und entfernen Artikel und andere oft benutzte Worte aus unseren Daten. 

Anschliessend zählen wir die verbleibenden Worte nach Kategorien und Gender.

Damit das Ergebnis einfacher zu lesen ist, wird das Ergebnis mit `arrange()` sortiert. Weil in unserem Fall sehr viele Worte nur einmal vorkommen, tragen diese nicht viel zum Gesamtinhalt bei. Daher werden diese Worte mit dem abschliessenden Filter für die Darstellung entfernt. 

```r
library(tidytext)

stopwords_DE = tibble(
    word = stopwords::stopwords("de", source = "stopwords-iso")
)

kodierteDaten2 |> 
    unnest_tokens(word, word_src) |>
    anti_join(stopwords_DE) |> 
    count(word, kategorie, gender) |> 
    arrange(desc(n)) |> 
    filter(n > 1)
```

| word | kategorie | gender | n |
| --- |  --- |  --- |  --- | 
| cas | cas | maskulin | 9 |
| marketing | funktion | neutral | 3 |
| mitstudierenden | gruppe | neutral | 3 |
| austausch | aktivität | maskulin | 2 |
| digital | cas | maskulin | 2 |
| marketing | cas | maskulin | 2 |
| netzwerk | gruppe | neutral | 2 |
| npo | cas | maskulin | 2 |
| referenten | gruppe | maskulin | 2 |

Solche Auswertungen geben zusätzliche Einblicke in die Inhalte und helfen bei der Interpretation der Daten. 

### Kodierte Daten aus mehreren Word-Dateien einlesen. 

Kodierte Texte sind keine Tabellen, sondern liegen in mehrere Dateien auf unserem Rechner. Diese Dateien sollen in einem Schritt eingelesen werden und in ein Stichprobenobjekt umgewandelt werden. 

#### Lösung

```r
library(tidyverse)
library(docxtractr)

datenordner = "kodiert"

tibble(
    datei = list.files(
                    path = datenordner, 
                    pattern = "^[^~]+.docx$"  
               )
) |> 
    group_by(datei) |> 
    mutate(
        pfad = str_c(datenordner, "/", datei),
        codes = read_docx(pfad) |> 
                    docx_extract_all_cmnts(include_text = TRUE) |> list()
    ) |> 
    ungroup() |> 
    unnest(codes) -> alleCodes
```

#### Erklärung

Die Code-Beispiele basieren auf Dateien aus dem [Beispieldaten](https://moodle.zhaw.ch/mod/resource/view.php?id=703515)

Wenn wir unsere Texte mit Word kodiert haben, können wir sie mit Hilfe der `docxtractr` Bibliothek einlesen. 

```r
library(tidyverse)
library(docxtractr)
```

Dazu erstellen wir uns ein Stichprobenobjekt zur Unterstützung, in das wir die Namen der Dateien einlesen.

```r
datenordner = "kodiert"

tibble(
    datei = list.files(
                    path = datenordner, 
                    pattern = "^[^~]+.docx$"  # nur reguläre Word Dokumente auswählen
               )
)  -> dateinamen
```

Die Funktion `list.files()` gibt einen Vektor mit allen Datennamen im angegebenen Verzeichnis `path` zurück. Mit dem Parameter `pattern` können Dateien nach ihrem Namen noch gezielter ausgewählt werden. Der hier gezeigte *reguläre Ausdruck* wird als Teil eines *logischen Ausdrucks* verwendet, um reguläre Word-Dokumente zu erhalten. Hier müssen wir aufpassen, denn die Dateiendung reicht nicht aus. Word erzeugt beim Bearbeiten einer Datei Hilfsdokumente, die ebenfalls auf `docx` enden, aber im vorderen Teil des Dateinamens eine Tilde (`~`) haben. Diese Dateien können wir nicht verwenden und sie dürfen deshalb nicht in unserer Dateiliste vorkommen.

Anschliessend können wir die kodierten Dokumente einzeln einlesen. Dabei müssen wir beachten, dass die Funktion `read_docx()` nur eine Datei gleichzeitig einlesen kann. Wir müssen deshalb über die Dateinamen mit `group_by()` gruppieren. Dadurch erhalten wir Teilstichproben mit genau einen Dateinamen. 

```r
dateinamen |> 
    group_by(datei) |> 
    mutate(
        pfad = str_c(datenordner, "/", datei),
        codes = read_docx(pfad) |> 
                    docx_extract_all_cmnts(include_text = TRUE) |> list()
    ) |> 
    ungroup() |> 
    unnest(codes) -> alleCodes
```

::: {.callout-warning}
Beachten Sie, dass Sie mit dem Parameter ``include_text = TRUE`` nicht nur die Kodierung einlesen, sondern auch den Text, der beim Kodieren markiert wurde. 
:::

Mit dieser Operation lesen wir jede einzelne Datei ein. In der Variablen `alleCodes` liegen nun alle vorgenommenen Kodierungen mit den relevanten Zusatzinformationen. Weil die Dateinamen Teil der Stichprobe ist, kann jeder Code und jeder markierte Text der Ursprungsdatei zugeordnet werden. 

#### Lösung für normale Textdateien

Das gleiche Prinzip funktioniert auch für beliebige Textdateien.

```r
datenordnet = "texte"
dateiendung = "txt"

tibble(
    datei = list.files(
                    path = datenordner, 
                    pattern = str_c("\\.", dateiendung, "$")
               )
) |> 
    group_by(datei) |> 
    mutate(
        pfad = str_c(datenordner, "/", datei),
        codes = read_file(pfad)
    ) |> 
    ungroup() -> eingeleseneTexte
```
