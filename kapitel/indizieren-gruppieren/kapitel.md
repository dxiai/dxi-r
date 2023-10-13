# Indizieren und Gruppieren {#sec-chapter-indizieren-gruppieren}

::: {.callout-warning}
## Work in Progress
::: 

## Indizieren

Es werden drei Arten von Indizes unterschieden: 

1. Der **Primärindex**, mit dem ein einzelner Datensatz eindeutig *identifiziert* werden kann. 
2. **Fremdschlüssel** sind Sekundärindizes für *Querverweise* auf eine zweite Datenstruktur (eine sog. *Indextabelle* oder engl. *Lookup-Table*).
3. **Gruppenindizes** sind Sekundärindizes zur Identifikation von Datensätzen mit *gemeinsamen Eigenschaften*.

Weil ein Index Werte über einen Datensatz enthält, gehört ein Index zum jeweiligen Datensatz und wird über einen *Indexvektor* in einer Stichprobe abgebildet.

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Indexvektor** bezeichnet einen Vektor, mit dessen Werten Datensätze identifiziert werden können.
</p>

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Hash** bezeichnet einen Wert in einem Indexvektor.
</p>


### Existierende Indexvektoren. 

Häufig liegen Indexvektoren bereits in einer Stichprobe vor.

#### Beispiel bestehende Primär- und Sekundärindizes

```r
mtcars %>% 
    as_tibble(rownames = "modell")  -> mtcarStichprobe

mtcarStichprobe
```

|modell               |  mpg| cyl|  disp|  hp| drat|    wt|  qsec| vs| am| gear| carb|
|:-------------------|----:|---:|-----:|---:|----:|-----:|-----:|--:|--:|----:|----:|
|Mazda RX4           | 21.0|   6| 160.0| 110| 3.90| 2.620| 16.46|  0|  1|    4|    4|
|Mazda RX4 Wag       | 21.0|   6| 160.0| 110| 3.90| 2.875| 17.02|  0|  1|    4|    4|
|Datsun 710          | 22.8|   4| 108.0|  93| 3.85| 2.320| 18.61|  1|  1|    4|    1|
|Hornet 4 Drive      | 21.4|   6| 258.0| 110| 3.08| 3.215| 19.44|  1|  0|    3|    1|
|Hornet Sportabout   | 18.7|   8| 360.0| 175| 3.15| 3.440| 17.02|  0|  0|    3|    2|
|Valiant             | 18.1|   6| 225.0| 105| 2.76| 3.460| 20.22|  1|  0|    3|    1|
|Duster 360          | 14.3|   8| 360.0| 245| 3.21| 3.570| 15.84|  0|  0|    3|    4|
|Merc 240D           | 24.4|   4| 146.7|  62| 3.69| 3.190| 20.00|  1|  0|    4|    2|
|Merc 230            | 22.8|   4| 140.8|  95| 3.92| 3.150| 22.90|  1|  0|    4|    2|
|Merc 280            | 19.2|   6| 167.6| 123| 3.92| 3.440| 18.30|  1|  0|    4|    4|
|Merc 280C           | 17.8|   6| 167.6| 123| 3.92| 3.440| 18.90|  1|  0|    4|    4|
|Merc 450SE          | 16.4|   8| 275.8| 180| 3.07| 4.070| 17.40|  0|  0|    3|    3|
|Merc 450SL          | 17.3|   8| 275.8| 180| 3.07| 3.730| 17.60|  0|  0|    3|    3|
|Merc 450SLC         | 15.2|   8| 275.8| 180| 3.07| 3.780| 18.00|  0|  0|    3|    3|
|Cadillac Fleetwood  | 10.4|   8| 472.0| 205| 2.93| 5.250| 17.98|  0|  0|    3|    4|
|Lincoln Continental | 10.4|   8| 460.0| 215| 3.00| 5.424| 17.82|  0|  0|    3|    4|
|Chrysler Imperial   | 14.7|   8| 440.0| 230| 3.23| 5.345| 17.42|  0|  0|    3|    4|
|Fiat 128            | 32.4|   4|  78.7|  66| 4.08| 2.200| 19.47|  1|  1|    4|    1|
|Honda Civic         | 30.4|   4|  75.7|  52| 4.93| 1.615| 18.52|  1|  1|    4|    2|
|Toyota Corolla      | 33.9|   4|  71.1|  65| 4.22| 1.835| 19.90|  1|  1|    4|    1|
|Toyota Corona       | 21.5|   4| 120.1|  97| 3.70| 2.465| 20.01|  1|  0|    3|    1|
|Dodge Challenger    | 15.5|   8| 318.0| 150| 2.76| 3.520| 16.87|  0|  0|    3|    2|
|AMC Javelin         | 15.2|   8| 304.0| 150| 3.15| 3.435| 17.30|  0|  0|    3|    2|
|Camaro Z28          | 13.3|   8| 350.0| 245| 3.73| 3.840| 15.41|  0|  0|    3|    4|
|Pontiac Firebird    | 19.2|   8| 400.0| 175| 3.08| 3.845| 17.05|  0|  0|    3|    2|
|Fiat X1-9           | 27.3|   4|  79.0|  66| 4.08| 1.935| 18.90|  1|  1|    4|    1|
|Porsche 914-2       | 26.0|   4| 120.3|  91| 4.43| 2.140| 16.70|  0|  1|    5|    2|
|Lotus Europa        | 30.4|   4|  95.1| 113| 3.77| 1.513| 16.90|  1|  1|    5|    2|
|Ford Pantera L      | 15.8|   8| 351.0| 264| 4.22| 3.170| 14.50|  0|  1|    5|    4|
|Ferrari Dino        | 19.7|   6| 145.0| 175| 3.62| 2.770| 15.50|  0|  1|    5|    6|
|Maserati Bora       | 15.0|   8| 301.0| 335| 3.54| 3.570| 14.60|  0|  1|    5|    8|
|Volvo 142E          | 21.4|   4| 121.0| 109| 4.11| 2.780| 18.60|  1|  1|    4|    2|


Der Vektor `modell` ist der **Primärindex**, weil dieser Vektor nur Werte enthält, die einen Datensatz eindeutig identifizieren.

Die Vektoren `cyl` (Zylinder), `vs` (Motortyp), `am` (Automatikschaltung), `gear` (Anzahl der Gänge), `carb` (Anzahl der Vergaser) sind *Sekundärindizes* und *Gruppenindizes*, die Modelle nach verschiedenen Kriterien zusammenfassen. 

### Fehlende Indexvektoren

Gelegentlich liegen in einer Stichprobe keine Primär- oder Sekundärindizes vor oder die vorhandenen Indizes erlauben keine Zusammenfassungen für eine konkrete Fragestellung. In solchen Fällen muss ein entsprechender Index erzeugt werden.

<p class="alert alert-primary" markdown="1">
**Definition:** Eine Funktion, die *Hashes* für Indexvektoren erzeugt, heisst **Hashing-Funktion**.
</p>

<p class="alert alert-secondary" markdown="1">
Hashing-Funktionen werden in der Industrie als Unterstützung zur Suche von Datensätzen in Datenbanken eingesetzt. Durch die geschickte Berechnung von Hashes beschleunigen diese Funktionen die Suche einzelner Werte um ein Vielfaches, indem sie den Bereich für die Suche einschränken. Deshalb haben viele Hashing-Funktionen ein anderes **Anwendungsziel** als die hier beschriebenen Hashing-Funktionen. 
</p>

### Hashing zur Identifikation

Die einfachste Technik zur eindeutigen Indizierung ist das ***Durchnummerieren*** der Datensätze einer Stichprobe. Bei dieser Technik wird jedem Datensatz eine Nummer zugewiesen. In R verwenden wir dazu die Funktion `row_number()`. Diese Funktion ist einer *Sequenz* vorzuziehen, weil diese Funktion auch bei leeren Stichproben fehlerfrei arbeitet.

In Excel muss zum Durchnummerieren die `SEQUENZ()`-Funktion verwendet werden. Das erreichen wir mit der folgenden Operation: `=SEQUENZ(ZEILEN(StichprobenBereich))`, wobei `StichprobenBereich` eine Excel-Adresse sein muss. Weil mit Excel keine leeren Stichproben erzeugt werden dürfen, gibt es mit Excel nicht das gleiche Problem wie mit R. Wegen dieser Eigenschaft muss ein entsprechender Bereich mindestens einen Stichprobenumfang von eins haben. Diese Eigenschaft gilt auch für Tabellen, die nur aus Überschriften bestehen. 

<p class="alert alert-secondary" markdown="1">
**Fingerübung:** Nummerieren Sie die Stichprobe `mtcars` und speichern Sie die Nummern im Vektor `nr`.
</p>

### Hashing zum Gruppieren

Beim Hashing zum Gruppieren müssen wir Werte erzeugen, die eine Zuordnung zu einer Gruppe oder einen Wert in einer anderen Stichprobe ermöglichen. Die Hashing-Funktion orientiert sich dabei an den konkreten Analyseanforderungen. 

Vier gängige Techniken können dabei unterschieden werden: 

- Kodieren (alle Datentypen)
- Reihenfolgen bilden durch Ganzzahldivision (nur Zahlen)
- Reihenfolgen bilden durch Modulo-Operation (nur Zahlen)
- Reihenfolgen durch Anfangsbuchstaben (nur Zeichenketten)

#### Beispiel Hashing zum Gruppieren.

Das folgende Beispiel bildet einen Index, um die Motorisierung der Fahrzeugtypen in der Stichprobe `mtcars` zu bestimmen. Dabei sollen die Modelle in schwach-, mittel-, stark- und sehr starkmotorisierte Typen unterschieden werden. Die Motorisierung richtet sich dabei zum einen nach der Leistung (`hp`). Zum anderen richtet sich die Motorisierung nach dem Fahrzeuggewicht (`wt`), weil für ein schweres Fahrzeug mehr Leistung zum Bewegen benötigt wird als für ein leichtes. Um beide Werte zu berücksichtigen, wird das Verhältnis der beiden Werte bestimmt. Ein Verhältnis ist eine *Division*. In diesem Fall wird das Gewicht als Nenner verwendet und die Leistung als Zähler. So ergeben sich immer Werte grösser als 1, weil die Leistung immer viel grösser als das Gewicht ist.

In diesem Beispiel besteht die Hashing-Funktion aus zwei Teilen: 

1. Das Verhältnis zwischen Leistung und Gewicht wird bestimmt und im Vektor `verhaeltnis` abgelegt. 
2. Die Leistungsklassen werden durch *Kodieren* den oben festgelegten Klassen zugewiesen und im Vektor `klasse` gespeichert.

```r
mtcars %>% 
    as_tibble(rownames = "modell") %>% 
    mutate(
        verhaeltnis = hp/wt, 
        klasse = case_when( 
            verhaeltnis > 60 ~ "sehr stark",
            verhaeltnis > 50 ~ "stark", 
            verhaeltnis > 40 ~ "mittel", 
            TRUE ~ "schwach") 
    )
```


<p class="alert alert-secondary" markdown="1">
**Fingerübung:** Bestimmen Sie die Effizienz der Modelle in `mtcars` indem Sie den Verbrauch (`mpg`) und den Hubraum (`disp`) berücksichtigen. Legen Sie Effizienzklassen fest.
</p>

### Hashing für Querverweise

Beim Hashing für Querverweise gibt es zwei Stichproben. Die erste Stichprobe ist die Hauptstichprobe mit den eigentlichen Werten. Die zweite Stichprobe ist die Referenzstichprobe, die zusätzliche Informationen enthält. Ein Indexvektor für Querverweise in der ersten Stichprobe bezieht sich immer auf einen Primärindex aus der zweiten Stichprobe.

Die Hashing-Funktion muss deshalb einen Verweis zur zweiten Stichprobe herstellen. Diese Verbindung kann mit der gleichen Strategie erzeugt werden, wie beim Gruppieren. Dabei muss jedoch darauf geachtet werden, dass alle Zuordnungen des Primärvektors korrekt abgebildet sind. 

## Gruppieren
