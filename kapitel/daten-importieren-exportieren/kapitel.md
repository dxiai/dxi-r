---
# bibliography: references.bib


abstract: ""

execute: 
  echo: false
---
# Importieren und Exportieren {#sec-chapter-daten-importieren}


## Daten importieren

Das Einlesen von Datendateien ist ein zentraler Bestandteil von R, weil es die Voraussetzung für die statistische Programmierung bildet. Diese Funktionen gehen jedoch nicht sehr sparsam mit dem Arbeitsspeicher unseres Computers um, sodass sehr grosse Datenmengen immer wieder zu Problemen führen. 

Die `readr`-Bibliothek ersetzt die Base R-Funktionen zum Einlesen von Dateien durch flexiblere und effizientere Funktionen. Diese Funktionen können mit grösseren Datenmengen umgehen und schonen den verfügbaren Arbeitsspeicher. Deshalb sind die `readr`-Funktionen den jeweiligen Gegenstücken von Base R vorzuziehen.


### Dateitypen

Für den Austausch von Stichproben stehen verschiedene Dateiformate zur Verfügung. Diese Dateiformate unterscheiden sich durch die Strategie, mit der die Werte in den einzelnen Tabellenzellen unterschieden werden. 

Die wichtigsten Formate sind: 

* Tabulator getrennte Werte (TSV, tabulator-separated values)
* Komma getrennte Werte (CSV, comma-separated values)
* Excel Tabellen (via `readxl`-Bibliothek)
* Fixformat Tabellen (FWF, fixed-width format)
* R-Datendateien (RDS, R-data structure)

Diese Dateien können wir mit den folgenden Funktionen einlesen.

| Format | tidy R | Base R |
| --- | --- | --- |
| txt (ganze Datei) | `read_file()` | `readChar()` + `file.info()` |  
| txt (zeilenweise) | `read_lines()` | `readLines()` + `file()` |
| csv (mit `,` als Trennzeichen) | `read_csv()` | `read.csv()` |
| csv (mit `;` als Trennzeichen) | `read_delim()` oder `read_csv2()` | `read.csv2()` |
| tsv | `read_tsv()` | `read.table()` |
| xls (Excel Arbeitsmappen mit `readxl`) | `read_excel()` | - |
| FWF | `read_fwf()` | - |
| RDS | `read_rds()` | `readRDS()` |

Die Base R Funktionen `read.table()`, `read.csv` und `read.csv2()` importieren Zeichenketten als Faktoren (s. @sec-chapter-datentypen). Damit können diese Werte nicht direkt als Zeichenketten behandelt werden. Diese automatische Behandlung entfällt bei den jeweiligen tidy R Varianten. Dadurch lassen sich Daten intuitiver bearbeiten.

::: {.callout-warning}
## Achtung

In der Schweiz kann das CSV-Format zu Verwirrung führen, weil sehr häufig das Semikolon als Spaltentrennzeichen und der Punkt als Dezimaltrennzeichen verwendet werden. Die Ursache für diese Situation sind CSV-Dateien, die aus Excel exportiert wurden. 

Die normalerweise für dieses Format empfohlene Funktion `read_csv2()` behandelt Dezimalzahlen fälschlich als Ganzzahlen. Um dieses Problem zu beheben, sollte das Dezimaltrennzeichen laut Dokumentation wie folgt angepasst werden:

```r
read_csv2(datei_name, locale = local(decimal_mark = "."))
```

**Dieser Aufruf funktioniert jedoch nicht!**

Hier greift die Funktion `read_delim()`. Wird dieser Funktion nur ein Dateiname übergeben, dann prüft die Funktion auf die verschiedenen Trennzeichen. Dieser (undokumentierte) Algorithmus erkennt Schweizer CSV-Dateien korrekt.

```r
read_delim(datei_name)
```

**Dieser Aufruf importiert die Werte wie erwartet.**

:::

Bei der modernen `read_` Variante können wir uns leicht an der Dateiendung orientieren, um die richtige `read_`-Funktion auszuwählen. 

Wenn eine Datei eingelesen wird, dann gibt die jeweilige `read_`-Funktion neben den Daten auch zurück, wie die Datei eingelesen wurde. Enthält die eingelesene Datei die erwarteten Spaltenüberschriften, dann wurde das richtige Dateiformat ausgewählt. 


### Dateien mit einer Spalte 

**CSV**-Dateien können mit Komma oder Semikolon als Trennzeichen erstellt werden. Die Funktion `read_delim()` liest diese Dateien meistens korrekt ein. Falls eine Datei mit nur **einem** *Datenvektor* importiert werden muss, dann kann R das Spaltentrennzeichen nicht finden. In solchen Fälle **muss** die Datei mit der `read_csv()` oder `read_csv2()`-Funktion noch einmal eingelesen werden.

Für Spalten mit Zeichenketten oder Ganzzahlen wird immer die Funktion `read_csv()` verwendet.

Für Gleitkommazahlen erfolgt die Auswahl auf Grundlage des verwendeten Dezimaltrennzeichens. Wird der Dezimalpunkt verwendet, dann **muss** die Funktion `read_csv()` benutzt werden. Wird das Dezimalkomma verwendet, dann **muss** die Funktion `read_csv2()` eingesetzt werden.

> ::: {#exm-import-single-col}
> ## Datei mit einer Spalte importieren
>
> Mit dem Aufruf `read_csv("beispieldaten.csv")` werden Daten mit einem Komma als Trennzeichen und mit Dezimalpunkt eingelesen. 
> 
> Mit dem Aufruf `read_csv2("beispieldaten.csv")` werden Daten mit einem Semikolon als Trennzeichen und mit Dezimalkomma eingelesen. 
>
> In beiden Fällen nutzen wir dieses Verhalten aus, um eine Stichprobe mit nur einer Spalte einzulesen. 
> :::

### Excel Arbeitsmappen

::: {.callout-tip}
## Praxis

Liegen Daten in einer Excel Arbeitsmappe vor, dann muss diese Arbeitsmappe **nicht** in ein anderes Dateiformat umgewandelt werden, damit die Daten in R importiert werden können.
:::

In Excel werden Daten in *Arbeitsmappen* organisiert. Es ist also möglich, mehr als eine Tabelle und darauf basierende Operationen in einer Datei zu speichern. Damit  Daten aus Arbeitsmappen in R importiert werden können, müssen die Struktur der  Arbeitsmappe bekannt sein.

Eine Excel Arbeitsmappe ist eine Datei, die üblicherweise auf `.xlsx` endet. Die Dateiendung signalisiert uns *meistens* die interne Organisation einer Datei. *Interne Organisation einer Datei* bedeutet, in welcher Folge die Daten in einer Datei auf der Festplatte abgelegt sind.

Nur das Dateiformat von `.xlsx`-Dateien unterstützt alle Funktionen von Excel und kann von R korrekt eingelesen werden

Das Dateiformat wird in Excel im `Speichern-Unter`-Dialog festgelegt. Dieser Dialog erscheint in der Regel, wenn eine neue Arbeitsmappe das erste Mal gespeichert wird. Wenn im Start-Dialog von Excel einfach eine neue Arbeitsmappe erstellt wird, dann erzeugt Excel *automatisch* eine Arbeitsmappe im Excel-Format. 

::: {.callout-note}
## Merke
Excel-Dateien sind Dateien mit der Endung `.xlsx` oder `.xls`und werden als **Excel Arbeitsmappen** bezeichnet. Nur Dateien mit dieser Endung können in R als Excel-Datei importiert werden.
:::

Excel Arbeitsmappen haben vier zentrale Strukturelemente: 

1. Arbeitsblätter 
2. Adressbereiche
3. Zellenwerte 
4. Zellenformeln

Jedes Arbeitsblatt einer Arbeitsmappe hat einen *eindeutigen* Namen. 

Die Adressbereiche sind in Zeilen und Spalten gegliedert. Wir finden Daten daher immer auf einem bestimmten Arbeitsblatt in einem bestimmten Adressbereich. Die konkrete Position der Daten in der Arbeitsmappe legen die Autoren willkürlich fest. 

Jede Zelle eines Arbeitsblatts hat *immer* zwei *gleichzeitige* Zustände, die immer in einer Excel Arbeitsmappe gespeichert werden: 

1. Jede Zelle hat einen Wert. 
2. Jede Zelle hat eine Operation.

Aus diesen Strukturelementen ergeben sich zwei Konsequenzen: 

1. Ein Arbeitsblatt kann mehr als eine Tabelle mit Daten enthalten.
2. Die Daten müssen nicht am Anfang (d.h. in der ersten Zeile und ersten Spalte) eines Arbeitsblatts beginnen. 

Um mit den Daten in Excel Arbeitsmappen arbeiten zu können, müssen bekannt sein, auf welchem Arbeitsblatt und in welchem Adressbereich die Daten stehen.

::: {.callout-note}
## Merke

Tabellen sind **keine** Strukturelemente von Excel Arbeitsmappen, die in R zugänglich sind.
:::

::: {.callout-warning}
## Achtung

Wenn Excel Arbeitsmappen mit Excel geöffnet werden, dann berechnet Excel alle Operationen auf *allen* Arbeitsblättern neu. Damit werden die Werte in der Arbeitsmappe verändert. 

Es kommt also vor, dass sich eine Arbeitsmappe ändert, ohne dass eine Interaktion vorgenommen wurde. In diesen Fällen fragt Excel beim Schliessen der Arbeitsmappe, ob die Änderungen gespeichert werden sollen. 

Wird eine Excel Arbeitsmappe in R (oder in einer anderen Programmiersprache) geöffnet, dann wird nur die Arbeitsmappe geöffnet ***ohne*** die Operationen neu zu berechnen. 
:::

Mit den Funktionen der `readxl`-Bibliothek können wir Excel Arbeitsmappen nach R importieren. Dabei sind zwei Funktionen von besonderer Bedeutung:

* `excel_sheets(dateiname)` und 
* `read_excel(dateiname, sheet)`

Mit der Funktion `excel_sheets()` können die vorhandenen Arbeitsblätter erkannt werden. Das Ergebnis dieser Funktion ist die Liste der Arbeitsblattnamen in einer Arbeitsmappe. Diese Funktion sollte vor dem Import von Daten zur Kontrolle der Arbeitsblattnamen verwendet werden. 

Die Funktion `read_excel()` erlaubt es einzelne Arbeitsblätter zu importieren. Wenn kein Arbeitsblattname für den Parameter `sheet` übergeben wird, dann nimmt die Funktion das aktive oder das erste Arbeitsblatt in der Arbeitsmappe.

Mit den `readxl`-Funktionen können keine Formeln aus den Zellen ausgelesen werden.

::: {#exm-read-arbeitsmappe}
## Excel-Arbeitsmappe importieren
```r
library(readxl)

Arbeitsblaetter = excel_sheets("Bestellungen_2.xlsx")
# Das Arbeitsblatt "Daten" sollte vorhanden sein.

Daten = read_excel("Bestellungen_2.xlsx", "Daten")
```
:::

Die Funktion `read_excel()` importiert alle Daten auf einem Arbeitsblatt. Enthält nur ein bestimmter Bereich auf einem Arbeitsblatt die Daten von Interesse, dann muss dieser Bereich als Excel-Bereichsadresse angegeben werden. 

::: {.callout-warning}
`read_excel()` kann nur mit Excels Arbeitsblattadressen umgehen. Tabellenadressen oder die Gatter-Notation beherrscht die Funktion nicht. 
:::

## Daten exportieren

R unterstützt den Export strukturierter Daten in Textdateien. Beim Exportieren kommen für die Formate TSV und CSV werden die entsprechenden Funktionen `write_tsv()`, `write_csv()` und `write_csv2()` benutzt. Für speziellformatierte Dateien kann die Funktion `write_delim()` eingesetzt werden. 

Alle Export-Funktionen erwarten eine Datenstruktur als ersten Parameter und einen Dateinamen als zweiten Parameter. Der Dateiname legt fest, wohin das Ergebnis der Funktion auf dem Computer geschrieben werden soll. 

Die Import- und Export-Funktionen lassen sich zu einfachen Konvertierungsprogrammen verknüpfen. @exm-korrect-swiss-csv korrigert von Excel exportierte CSV-Dateien in ein gültiges CSV-Format. 

::: {#exm-korrect-swiss-csv}
## "Schweizer" CSV-Format korrigieren

```r
library(readr)

write_csv(
    read_delim("Bestellungen_Excel.csv"), 
    "Bestellungen_korrigiert.csv" 
)
```
:::

::: {.callout-warning}
R kann Excel Arbeitsmappen **nicht exportieren**. Die `readr`-Funktionen `write_excel_csv()` und `write_excel_csv2()` exportieren CSV-Dateien mit einer zusätzlichen Markierung am Dateianfang. **Diese Funktionen sollten nur verwendet werden, wenn eine CSV-Datei nur mit Excel importiert werden soll und nicht für die Archivierung oder Weiterverarbeitung gedacht ist.**

Die zusätzliche Markierung wird als **Byte Order Mark** (BOM) bezeichnet und muss das UTF8-Symbol `FEFF` sein. Dieses Symbol ist ein Leerzeichen ohne Länge und wird deshalb nie dargestellt. Excel bzw. Power Query verwenden das BOM, um UTF8-kodierte Dateien zu identifizieren.
:::

## JSON-Daten

JSON ist ein Datenformat, dass von vielen sog. *Web-Diensten* zum Austausch von Datenstrukturen eingesetzt wird. R kann dieses Datenformat mit der `tidyverse`-Bibliothek `jsonlite` importieren und auch exportieren. `jsonlite` stellt zwei Funktionen für den regelmässigen Einsatz bereit:

- `fromJSON()`
- `toJSON()`

Die beiden Funktionen `fromJSON()` und `toJSON()` unterstützen das Parsen von und Serialisieren zu Zeichenketten im JSON-Format.

Um Daten aus einer Textdatei im JSON-Format zu importieren, muss die gesamte Datei zuerst eingelesen werden und dann an den JSON-Parser `fromJSON()` übergeben werden. 

::: {#exm-parse-json}
## JSON Daten aus einer Datei importieren

```r
library(jsonlite)

Daten = fromJSON(read_file("beispiele/daten.json"))
```
::: 

Mit der Funktion `toJSON()` werden Daten in eine JSON-formatierte Zeichenkette umgewandelt. Diese Zeichenkette kann anschliessend mit `write_file()` in eine Datei geschrieben werden.


::: {#exm-parse-json}
## Daten im JSON-Format exportieren

```r
write_file(toJSON(Daten),"neue_daten.json"))
```
::: 

::: {.callout-note}
Die beiden Funktionen `read_json()` und `write_json()` erlauben das Lesen und Schreiben von Textdateien im JSON-Format. Die Standardeinstellungen sind jedoch nicht identisch mit denen von `fromJSON()` und `toJSON()`, so dass der Import und Export mit diesen Funktionen komplexer ist, als mit der oben beschrieben Technik.
:::

## YAML-Daten

YAML ist eine Verallgemeinerung des JSON-Formats. Mit dem Ziel, dass Menschen komplexe Datenstrukturen leichter eingeben und lesen können. In R wird das Format von der Bibliothek `yaml` unterstützt. Diese Bibliothek gehört nicht zum `tidyverse` und muss separat installiert werden. 

Die `yaml`-Bibliothek stellt vier Funktionen bereit: 

- `yaml.load()` zum *Parsen* einer YAML-formatierte Zeichenkette in einer Datenstruktur.
- `as.yaml()` zum *Serialisieren* einer Datenstruktur in eine YAML-Zeichenkette.
- `read_yaml()` zum Importieren von YAML-Daten aus einer Datei.
- `write_yaml()` zum Schreiben einer Datenstruktur in eine YAML-formatierte Datei.

Der YAML-Parser `yaml.load()` erzeugt immer eine geschachtelte Datenstruktur aus benannten Listen, unbenannten Listen und Vektoren erzeugt wird. Der Parser erkennt automatisch, ob eine YAML-Liste ein Vektor oder eine Liste ist. YAML-Objekte werden immer in benannte Listen umgewandelt.

::: {#exm-read_yaml}
## YAML-Daten mit `read_yaml()` importieren
```r
library(yaml)

yamlDaten = read_yaml("daten.yml")
```
:::

::: {#exm-write_yaml}
## YAML-Daten mit `read_yaml()` exportieren
```r
yamlDaten |> write_yaml("kopie_der_daten.yml")
```
:::

## Festkodierte Daten

R unterstützt den Import von **festkodierten Daten** nicht direkt. Festkodierte Daten benötigen einen eigenen Parser, der die Datenfelder extrahiert. Die prinzipielle Vorgehensweise ähnelt dem Import und Export von JSON-Daten. Dazu werden die Daten als unstrukturierte Textdaten mit der Funktion `read_file()` eingelesen. Anschliessend werden die Datenfelder mit Zeichenketten-Operationen ([@sec-chapter-zeichenketten]) einzeln extrahiert. Beim Exportieren müssen die Daten zuerst serialisiert werden und anschliessend mit der Funktion `write_file()` in die entsprechende Datei geschrieben werden.
