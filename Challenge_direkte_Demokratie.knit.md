
<!-- rnb-text-begin -->

---
title: "Challenge: Direkte Demokratie"
output: html_notebook
---

***Gruppe: Chantal und Julia***
                                        
                                        
# Fragestellung: <b>
### Wieso gestalltet sich die Finanzierung der AHV so schwierig?

# Daten: <b>
### Wir verwendeten die Daten von swissvotes.ch (https://swissvotes.ch/page/dataset)
                                       
#### Packete importieren

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeShlc3F1aXNzZSlcbmxpYnJhcnkoZ2dwbG90MilcbmxpYnJhcnkodGlkeXZlcnNlKVxubGlicmFyeShtYWdyaXR0cilcbmxpYnJhcnkocm1hcmtkb3duKVxuYGBgIn0= -->

```r
library(esquisse)
library(ggplot2)
library(tidyverse)
library(magrittr)
library(rmarkdown)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Daten einlesen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGF0YV9jc3YgPC0gcmVhZC5jc3YyKFwiRGF0ZW5zYXR6QWJzdGltbXVuZ2VuLmNzdlwiLCBuYSA9IFwiTkFcIilcbmBgYCJ9 -->

```r
data_csv <- read.csv2("DatensatzAbstimmungen.csv", na = "NA")
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Datum Spalte zu Datentyp "date" ändern

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGF0YV9jc3YgPC0gZGF0YV9jc3YlPiVcbiAgbXV0YXRlKGRhdHVtPWFzLkRhdGUoZGF0dW0sIFwiJWQuJW0uJVlcIikpXG5cbmNsYXNzKGRhdGFfY3N2JGRhdHVtKVxuYGBgIn0= -->

```r
data_csv <- data_csv%>%
  mutate(datum=as.Date(datum, "%d.%m.%Y"))

class(data_csv$datum)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

#### Datensatz nur mit AHV Abstimmungen erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWh2IDwtIHN1YnNldChkYXRhX2NzdiwgYW5yID09IDk5IHxhbnIgPT0gMTAxIHwgYW5yID09IDExNSB8IGFuciA9PSAxNDQgfCBhbnIgPT0gMjgwIHwgYW5yID09IDI4MSB8YW5yID09IDM1MiB8YW5yID09IDQwMSB8IGFuciA9PSA0MjMgfCBhbnIgPT0gNDQ0IHwgYW5yID09IDQ2OSB8YW5yID09IDQ4MSB8YW5yID09IDQ4OS4xICAgfCBhbnIgPT0gNDg5LjIgIHwgYW5yID09IDUwNyB8IGFuciA9PSA1MDggfCBhbnIgPT0gNTIzIHxhbnIgPT0gNTM2IHxhbnIgPT0gNTk0IHwgYW5yID09IDYwNiB8IGFuciA9PSA2MTQgfGFuciA9PSA2MjcgfGFuciA9PSA0MDEgfGFuciA9PSA0MjIpXG5gYGAifQ== -->

```r
ahv <- subset(data_csv, anr == 99 |anr == 101 | anr == 115 | anr == 144 | anr == 280 | anr == 281 |anr == 352 |anr == 401 | anr == 423 | anr == 444 | anr == 469 |anr == 481 |anr == 489.1   | anr == 489.2  | anr == 507 | anr == 508 | anr == 523 |anr == 536 |anr == 594 | anr == 606 | anr == 614 |anr == 627 |anr == 401 |anr == 422)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Bei AHV Datensatz Spalte annahme 1,0 durch "Worte"Abglehnt" und "Angenommen" ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWh2JGFubmFobWVbYWh2JGFubmFobWUgPT0gXCIwXCJdIDwtIFwiQWJnZWxlaG50XCJcbmFodiRhbm5haG1lW2FodiRhbm5haG1lID09IFwiMVwiXSA8LSBcIkFuZ2Vub21tZW5cIlxuYGBgIn0= -->

```r
ahv$annahme[ahv$annahme == "0"] <- "Abgelehnt"
ahv$annahme[ahv$annahme == "1"] <- "Angenommen"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>

# Grafik 1 (Blog): Aufteilung der AHV-Abstimmungen in Teilgebiete 

#### Data Frame mit benötigten Kateogiren erstellen (im Vorfeld entschieden, was zu welcher Karegorie gehört. Noch aufschreiben)

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxua2F0ZWdvcmllbiA8LSBjKFwiRmluYW56aWVydW5nXCIsXCJSZXZpc2lvblwiLFwiUmVudGVuYWx0ZXJcIiwgXCJBbmRlcmVzXCIpXG5BbnphaGwgPC0gYygxMSw0LDQsNSlcblxuZGZLYXRlZ29yaWVuIDwtIGRhdGEuZnJhbWUoa2F0ZWdvcmllbixBbnphaGwpXG5gYGAifQ== -->

```r
kategorien <- c("Finanzierung","Revision","Rentenalter", "Anderes")
Anzahl <- c(11,4,4,5)

dfKategorien <- data.frame(kategorien,Anzahl)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGRmS2F0ZWdvcmllbikgK1xuIGFlcyh4ID0ga2F0ZWdvcmllbiwgd2VpZ2h0ID0gQW56YWhsKSArXG4gZ2VvbV9iYXIoZmlsbCA9IFwiIzBDNEM4QVwiKSArXG4gIHRoZW1lX21pbmltYWwoKStcbiBsYWJzKHggPSBcIlRoZW1lbmJlcmVpY2hlXCIsIHkgPSBcIkFuemFobCBJbml0aWF0aXZlblwiKSArXG4gIGxhYnMoXG4gICAgdGl0bGUgPSBcIkF1ZnRlaWx1bmcgZGVyIFRoZW1lbiBpbiBkZW4gQUhWLUFic3RpbW11bmdlblwiKStcbiAgdGhlbWUocGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLGhqdXN0ID0gMC41KSlcbmBgYCJ9 -->

```r
ggplot(dfKategorien) +
 aes(x = kategorien, weight = Anzahl) +
 geom_bar(fill = "#0C4C8A") +
  theme_minimal()+
 labs(x = "Themenbereiche", y = "Anzahl Initiativen") +
  labs(
    title = "Aufteilung der Themen in den AHV-Abstimmungen")+
  theme(plot.title = element_text(size = 15L,hjust = 0.5))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>

# Grafik 2: Alle Abstimmungen über die AHV seit 1848 und ob diese angenommen oder abgelehnt wurden

#### DataFrame nur mit Datum, Abstimmung und Ergebnis

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWh2X0VyZ2Vibmlzc2UgPC0gc3Vic2V0KGFodiwgc2VsZWN0PWMoZGF0dW0sdGl0ZWxfa3Vyel9kLGFubmFobWUsdm9sa2phLnByb3opKVxuYGBgIn0= -->

```r
ahv_Ergebnisse <- subset(ahv, select=c(datum,titel_kurz_d,annahme,volkja.proz))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGFodl9FcmdlYm5pc3NlKSArXG4gYWVzKHggPSBkYXR1bSwgZmlsbCA9IGFubmFobWUpICtcbiBnZW9tX2hpc3RvZ3JhbShiaW5zID0gMzFMKSArXG4gc2NhbGVfZmlsbF9odWUoZGlyZWN0aW9uID0gMSkgK1xuIGxhYnMoeCA9IFwiSmFocmVcIiwgeSA9IFwiQW56YWhsIEFic3RpbW11bmdlblwiKSArXG4gdGhlbWVfbWluaW1hbCgpK1xuIHNjYWxlX2ZpbGxfbWFudWFsKCdFcmdlYm5pcycsIHZhbHVlcz1jKCcjMEM0QzhBJywgJ3RhbjInKSkrXG4gICBsYWJzKFxuICAgIHRpdGxlID0gXCJBbGxlIEFic3RpbW11bmdlbiDDvGJlciBkaWUgQUhWIHNlaXQgMTg0OFwiKStcbiAgdGhlbWUocGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLCBoanVzdCA9IDAuNSkpXG5gYGAifQ== -->

```r
ggplot(ahv_Ergebnisse) +
 aes(x = datum, fill = annahme) +
 geom_histogram(bins = 31L) +
 scale_fill_hue(direction = 1) +
 labs(x = "Jahre", y = "Anzahl Abstimmungen") +
 theme_minimal()+
 scale_fill_manual('Ergebnis', values=c('#0C4C8A', 'tan2'))+
   labs(
    title = "Alle Abstimmungen über die AHV seit 1848")+
  theme(plot.title = element_text(size = 15L, hjust = 0.5))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->







# Abstimmungen mit Militär oder Medien in Verbindung mit Finanzierung suchen
                               
#### Zeilen suchen wo d1e1 == 3 oder d1e1 == 12 (3= Sicherheitspolitik, 12 = Kultur, Bidung, Medien)

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZDFlMV8zX29kZXJfMTIgPC0gc3Vic2V0KGRhdGFfY3N2LCBkMWUxID09IDMgfCBkMWUxID09IDEyKVxuYGBgIn0= -->

```r
d1e1_3_oder_12 <- subset(data_csv, d1e1 == 3 | d1e1 == 12)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Zeilen suchen wo d1e2 == 3.2(Militär) oder d1e2 == 12.5(Medien)

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZDFlMl8zLjJfb2Rlcl8xMi41IDwtIHN1YnNldChkMWUxXzNfb2Rlcl8xMiwgZDFlMiA9PSAzLjIgfCBkMWUyID09IDEyLjUpXG5gYGAifQ== -->

```r
d1e2_3.2_oder_12.5 <- subset(d1e1_3_oder_12, d1e2 == 3.2 | d1e2 == 12.5)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Was hat vom Data Frame d1e2_3.2_oder_12.5 mit Finanzen zu tun?

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWJzdGltbXVuZ2VuX21pbGl0w6RyX2ZpbmFuemllcnVuZyA8LSBzdWJzZXQoZDFlMl8zLjJfb2Rlcl8xMi41LCBkMmUxID09IDYpXG5cbiMgSGllciBhbHMgRXJnZWJuaXMgbnVyIEFic3RpbW11bmdlbiBtaXQgVmVyYmluZHVuZyB6dW0gTWlsaXTDpHIgcmF1c2dla29tbWVuLCBkZXNoYWxiIE5hbWUgZGFyYXVmIGdlw6RuZGVydC5cbmBgYCJ9 -->

```r
abstimmungen_militär_finanzierung <- subset(d1e2_3.2_oder_12.5, d2e1 == 6)

# Hier als Ergebnis nur Abstimmungen mit Verbindung zum Militär rausgekommen, deshalb Name darauf geändert.
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Filtern nach Abstimmungen mit Medien

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZDFlMl8xMi41IDwtIHN1YnNldChkMWUxXzNfb2Rlcl8xMiwgZDFlMiA9PSAxMi41KVxuYGBgIn0= -->

```r
d1e2_12.5 <- subset(d1e1_3_oder_12, d1e2 == 12.5)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>

# Grafik 4: Abstimmungsergebnisse im Bereich der AHV-Finanzierung
                                                    
#### Data Frame mit AHV-Abstimmungen im Bereich Finanzierung erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWh2X0ZpbmFuemllcnVuZyA8LSBzdWJzZXQoYWh2LCBhbnI9PTQwMSB8YW5yID09IDQ4MSB8IGFuciA9PSA0ODkuMSB8IGFuciA9PSA0ODkuMiB8IGFuciA9PSA1MDggfCBhbnIgPT0gNTIzIHwgYW5yID09IDU5NCB8IGFuciA9PSA2MDYgfCBhbnIgPT0gNjE0IHwgYW5yID09IDYyNylcbmBgYCJ9 -->

```r
ahv_Finanzierung <- subset(ahv, anr==401 |anr == 481 | anr == 489.1 | anr == 489.2 | anr == 508 | anr == 523 | anr == 594 | anr == 606 | anr == 614 | anr == 627)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Wie oft abgelehnt / angenommen?

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuemFobGVuX2FuZ19hYmcgPC0gYWh2X0ZpbmFuemllcnVuZyAlPiVcbiAgZ3JvdXBfYnkoYW5uYWhtZSkgJT4lXG4gIGNvdW50KClcbmBgYCJ9 -->

```r
zahlen_ang_abg <- ahv_Finanzierung %>%
  group_by(annahme) %>%
  count()
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KHphaGxlbl9hbmdfYWJnLCBhZXMoeD1cIlwiLCB5PW4sIGZpbGw9YW5uYWhtZSkpK1xuICBnZW9tX2JhcihzdGF0PVwiaWRlbnRpdHlcIiwgd2lkdGggPSAxKStcbiAgY29vcmRfcG9sYXIoXCJ5XCIsc3RhcnQ9MCkrXG4gIGdlb21fdGV4dChhZXMobGFiZWwgPSBwYXN0ZTAoKHJvdW5kKDEwMC8xMCkqbiksIFwiJVwiKSksIHBvc2l0aW9uID0gcG9zaXRpb25fc3RhY2sodmp1c3Q9MC41KSxjb2xvcj1cIndoaXRlXCIpICtcbiAgbGFicyh4ID0gTlVMTCwgeSA9IE5VTEwsIGZpbGwgPSBOVUxMKSArXG4gIHRoZW1lX2NsYXNzaWMoKSArXG4gIHRoZW1lKGF4aXMubGluZSA9IGVsZW1lbnRfYmxhbmsoKSxcbiAgICAgICAgICBheGlzLnRleHQgPSBlbGVtZW50X2JsYW5rKCksXG4gICAgICAgICAgYXhpcy50aWNrcyA9IGVsZW1lbnRfYmxhbmsoKSkgK1xuICBzY2FsZV9maWxsX21hbnVhbCh2YWx1ZXM9YyhcIiMwQzRDOEFcIixcInRhbjJcIikpK1xuICAgIGxhYnMoXG4gICAgdGl0bGUgPSBcIkFic3RpbW11bmdzZXJnZWJuaXNzZSBpbSBCZXJlaWNoIGRlciBBSFYtRmluYW56aWVydW5nXCIpK1xuICB0aGVtZShwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsIGhqdXN0ID0gMC41KSlcbmBgYCJ9 -->

```r
ggplot(zahlen_ang_abg, aes(x="", y=n, fill=annahme))+
  geom_bar(stat="identity", width = 1)+
  coord_polar("y",start=0)+
  geom_text(aes(label = paste0((round(100/10)*n), "%")), position = position_stack(vjust=0.5),color="white") +
  labs(x = NULL, y = NULL, fill = NULL) +
  theme_classic() +
  theme(axis.line = element_blank(),
          axis.text = element_blank(),
          axis.ticks = element_blank()) +
  scale_fill_manual(values=c("#0C4C8A","tan2"))+
    labs(
    title = "Abstimmungsergebnisse im Bereich der AHV-Finanzierung")+
  theme(plot.title = element_text(size = 15L, hjust = 0.5))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>

# Grafik 5: Abstimmungsergebnisse im Bereich Medien in Verbindung mit Finanzierung

#### Pie Chart Medien in Verbindung mit Finanzierungen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuIyBEYXRhRnJhbWUgZXJzdGVsbGVuIG1pdCBBYmdlbGVobnQ6MiB1bmQgQW5nZW5vbW1lbjoyIChpbSBWb3JmZWxkIGRpZSB2aWVyIEFic3RpbW11bmdlbiB1bnRlcnN1Y2h0KVxuYW5uYWhtZSA8LSBjKFwiQW5nZW5vbW1lblwiLCBcIkFiZ2VsZWhudFwiKVxubiA8LSBjKDIsMilcbmRmTWVkaWVuIDwtIGRhdGEuZnJhbWUoYW5uYWhtZSxuKVxuXG5gYGAifQ== -->

```r
# DataFrame erstellen mit Abgelehnt:2 und Angenommen:2 (im Vorfeld die vier Abstimmungen untersucht)
annahme <- c("Angenommen", "Abgelehnt")
n <- c(2,2)
dfMedien <- data.frame(annahme,n)

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGRmTWVkaWVuLCBhZXMoeD1cIlwiLCB5PW4sIGZpbGw9YW5uYWhtZSkpK1xuICBnZW9tX2JhcihzdGF0PVwiaWRlbnRpdHlcIiwgd2lkdGggPSAxKStcbiAgY29vcmRfcG9sYXIoXCJ5XCIsc3RhcnQ9MCkrXG4gIGdlb21fdGV4dChhZXMobGFiZWwgPSBwYXN0ZTAocm91bmQoKDEwMC80KSpuKSwgXCIlXCIpKSwgcG9zaXRpb24gPSBwb3NpdGlvbl9zdGFjayh2anVzdD0wLjUpLGNvbG9yID0gXCJ3aGl0ZVwiKSArXG4gIGxhYnMoeCA9IE5VTEwsIHkgPSBOVUxMLCBmaWxsID0gTlVMTCkgK1xuICB0aGVtZV9jbGFzc2ljKCkgK1xuICB0aGVtZShheGlzLmxpbmUgPSBlbGVtZW50X2JsYW5rKCksXG4gICAgICAgICAgYXhpcy50ZXh0ID0gZWxlbWVudF9ibGFuaygpLFxuICAgICAgICAgIGF4aXMudGlja3MgPSBlbGVtZW50X2JsYW5rKCkpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwodmFsdWVzPWMoXCIjMEM0QzhBXCIsXCJ0YW4yXCIpKStcbiAgICBsYWJzKFxuICAgIHRpdGxlID0gXCJBYnN0aW1tdW5nc2VyZ2Vibmlzc2UgaW0gQmVyZWljaCBNZWRpZW4gaW4gVmVyYmluZHVuZyBtaXQgRmluYW56aWVydW5nXCIpK1xuICB0aGVtZShwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsIGhqdXN0ID0gMC4yKSlcbmBgYCJ9 -->

```r
ggplot(dfMedien, aes(x="", y=n, fill=annahme))+
  geom_bar(stat="identity", width = 1)+
  coord_polar("y",start=0)+
  geom_text(aes(label = paste0(round((100/4)*n), "%")), position = position_stack(vjust=0.5),color = "white") +
  labs(x = NULL, y = NULL, fill = NULL) +
  theme_classic() +
  theme(axis.line = element_blank(),
          axis.text = element_blank(),
          axis.ticks = element_blank()) +
  scale_fill_manual(values=c("#0C4C8A","tan2"))+
    labs(
    title = "Abstimmungsergebnisse im Bereich Medien in Verbindung mit Finanzierung")+
  theme(plot.title = element_text(size = 15L, hjust = 0.2))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

                  
<br>
<br>

# Grafik 6: Abstimmungsergebnisse im Bereich Militär in Verbindung mit Finanzierung
                  
#### Pie Chart Militär in Verbindung mit Finanzierungen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuIyBEYXRhRnJhbWUgZXJzdGVsbGVuIG1pdCBBYmdlbGVobnQ6NCB1bmQgQW5nZW5vbW1lbjo3IChpbSBWb3JmZWxkIGRpZSBlbGYgQWJzdGltbXVuZ2VuIHVudGVyc3VjaHQpXG5hbm5haG1lIDwtIGMoXCJBbmdlbm9tbWVuXCIsIFwiQWJnZWxlaG50XCIpXG5uIDwtIGMoNyw0KVxuZGZNaWxpdMOkciA8LSBkYXRhLmZyYW1lKGFubmFobWUsbilcblxuYGBgIn0= -->

```r
# DataFrame erstellen mit Abgelehnt:4 und Angenommen:7 (im Vorfeld die elf Abstimmungen untersucht)
annahme <- c("Angenommen", "Abgelehnt")
n <- c(7,4)
dfMilitär <- data.frame(annahme,n)

```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGRmTWlsaXTDpHIsIGFlcyh4PVwiXCIsIHk9biwgZmlsbD1hbm5haG1lKSkrXG4gIGdlb21fYmFyKHN0YXQ9XCJpZGVudGl0eVwiLCB3aWR0aCA9IDEpK1xuICBjb29yZF9wb2xhcihcInlcIixzdGFydD0wKStcbiAgZ2VvbV90ZXh0KGFlcyhsYWJlbCA9IHBhc3RlMChyb3VuZCgoMTAwLzExKSpuKSwgXCIlXCIpKSwgcG9zaXRpb24gPSBwb3NpdGlvbl9zdGFjayh2anVzdD0wLjUpLGNvbG9yPVwid2hpdGVcIikgK1xuICBsYWJzKHggPSBOVUxMLCB5ID0gTlVMTCwgZmlsbCA9IE5VTEwpICtcbiAgdGhlbWVfY2xhc3NpYygpICtcbiAgdGhlbWUoYXhpcy5saW5lID0gZWxlbWVudF9ibGFuaygpLFxuICAgICAgICAgIGF4aXMudGV4dCA9IGVsZW1lbnRfYmxhbmsoKSxcbiAgICAgICAgICBheGlzLnRpY2tzID0gZWxlbWVudF9ibGFuaygpKSArXG4gIHNjYWxlX2ZpbGxfbWFudWFsKHZhbHVlcz1jKFwiIzBDNEM4QVwiLFwidGFuMlwiKSkrXG4gICAgbGFicyhcbiAgICB0aXRsZSA9IFwiQWJzdGltbXVuZ3NlcmdlYm5pc3NlIGltIEJlcmVpY2ggTWlsaXTDpHIgaW4gVmVyYmluZHVuZyBtaXQgRmluYW56aWVydW5nXCIpK1xuICB0aGVtZShwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsIGhqdXN0ID0gMC4yKSlcbmBgYCJ9 -->

```r
ggplot(dfMilitär, aes(x="", y=n, fill=annahme))+
  geom_bar(stat="identity", width = 1)+
  coord_polar("y",start=0)+
  geom_text(aes(label = paste0(round((100/11)*n), "%")), position = position_stack(vjust=0.5),color="white") +
  labs(x = NULL, y = NULL, fill = NULL) +
  theme_classic() +
  theme(axis.line = element_blank(),
          axis.text = element_blank(),
          axis.ticks = element_blank()) +
  scale_fill_manual(values=c("#0C4C8A","tan2"))+
    labs(
    title = "Abstimmungsergebnisse im Bereich Militär in Verbindung mit Finanzierung")+
  theme(plot.title = element_text(size = 15L, hjust = 0.2))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

                  
<br>
<br>                  


# Grafik 7: Prozentualer Ja-Anteil bei abgelehnten AHV-Initiativen
#### Benötigter Data Frame erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYWJsZWhudW5nIDwtIHN1YnNldChhaHZfRXJnZWJuaXNzZSwgYW5uYWhtZSA9PSBcIkFiZ2VsZWhudFwiKSBcbmFibGVobnVuZyR2b2xramEucHJvel9uIDwtYXMubnVtZXJpYyhhYmxlaG51bmckdm9sa2phLnByb3opXG5gYGAifQ== -->

```r
ablehnung <- subset(ahv_Ergebnisse, annahme == "Abgelehnt") 
ablehnung$volkja.proz_n <-as.numeric(ablehnung$volkja.proz)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGFibGVobnVuZykgK1xuICBhZXMoXG4gICAgeCA9IHZvbGtqYS5wcm96X24sXG4gICAgeSA9IHRpdGVsX2t1cnpfZCxcbiAgICBjb2xvdXIgPSB2b2xramEucHJvel9uXG4gICkgK1xuICBnZW9tX3BvaW50KHNoYXBlID0gXCJjaXJjbGVcIiwgc2l6ZSA9IDRMKSArXG4gIHNjYWxlX2NvbG9yX2dyYWRpZW50KGxvdyA9IFwiIzBDNEM4QVwiLCBoaWdoID0gXCJ0YW4xXCIpICtcbiAgbGFicyhcbiAgICB4ID0gXCJQcm96ZW50dWFsZXIgQW50ZWlsIGRlciBKYS1TdGltbWVuXCIsXG4gICAgeSA9IFwiIFwiLFxuICAgIHRpdGxlID0gXCJQcm96ZW50dWFsZXIgQW50ZWlsIEphLVN0aW1tZW4gYmVpIGFiZ2VsZWhudGVuIEFIVi1Jbml0aWF0aXZlblwiLFxuICAgIGNvbG9yID0gXCJQcm96ZW50YW50ZWlsXCJcbiAgKSArXG4gIHRoZW1lX21pbmltYWwoKSArXG4gIHRoZW1lKFxuICAgIHBsb3QudGl0bGUgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDE1TCxcbiAgICBmYWNlID0gXCJib2xkXCIsXG4gICAgaGp1c3QgPSAwLjkpLFxuICAgIGF4aXMudGl0bGUueSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTJMLFxuICAgIGZhY2UgPSBcImJvbGRcIiksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMkwsXG4gICAgZmFjZSA9IFwiYm9sZFwiKVxuICApICsgXG4gIHRoZW1lKHRleHQgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDE1KSkgXG5gYGAifQ== -->

```r
ggplot(ablehnung) +
  aes(
    x = volkja.proz_n,
    y = titel_kurz_d,
    colour = volkja.proz_n
  ) +
  geom_point(shape = "circle", size = 4L) +
  scale_color_gradient(low = "#0C4C8A", high = "tan1") +
  labs(
    x = "Prozentualer Anteil der Ja-Stimmen",
    y = " ",
    title = "Prozentualer Anteil Ja-Stimmen bei abgelehnten AHV-Initiativen",
    color = "Prozentanteil"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.9),
    axis.title.y = element_text(size = 12L,
    face = "bold"),
    axis.title.x = element_text(size = 12L,
    face = "bold")
  ) + 
  theme(text = element_text(size = 15)) 
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->





# Überprüfung der Theorien
                                                             
#### Data Frame mit Medien- Militär- und AHV- Finanzierungsabstimmungen erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW4gPC0gc3Vic2V0KGRhdGFfY3N2LCBhbnIgPT0gMTYgfCBhbnIgPT0gMTggfCBhbnIgPT0gODYgfGFuciA9PSAxMzEgfCBhbnIgPT0gMTYxIHwgYW5yID09IDE2MiB8YW5yID09IDE4MSB8ICBhbnIgPT0gMzkzIHwgYW5yID09IDQwMSB8YW5yID09IDQ4MSB8YW5yID09IDQ4OS4xICB8IGFuciA9PSA0ODkuMiB8IGFuciA9PSA1MDggfCBhbnIgPT0gNTIzIHxhbnIgPT0gNTk0IHwgYW5yID09IDYwNiB8IGFuciA9PSA2MTQgfGFuciA9PSA2MjcgfCAgYW5yID09IDQyNyB8IGFuciA9PSA0NzEgfCBhbnIgPT0gNTg0IHwgYW5yID09IDYzNXwgIGFuciA9PSA1OTUgfCBhbnIgPT0gNjE3IHwgYW5yID09IDY1NClcbmBgYCJ9 -->

```r
finanzierungen <- subset(data_csv, anr == 16 | anr == 18 | anr == 86 |anr == 131 | anr == 161 | anr == 162 |anr == 181 |  anr == 393 | anr == 401 |anr == 481 |anr == 489.1  | anr == 489.2 | anr == 508 | anr == 523 |anr == 594 | anr == 606 | anr == 614 |anr == 627 |  anr == 427 | anr == 471 | anr == 584 | anr == 635|  anr == 595 | anr == 617 | anr == 654)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Data Frame kürzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQgPC0gc3Vic2V0KGZpbmFuemllcnVuZ2VuLCBzZWxlY3Q9YyhhbnIsZGF0dW0sdGl0ZWxfa3Vyel9kLGQxZTEsYW5uYWhtZSxici5wb3MsYnYucG9zLG5yLnBvcyxzci5wb3MpKVxuYGBgIn0= -->

```r
finanzierungen_gekuerzt <- subset(finanzierungen, select=c(anr,datum,titel_kurz_d,d1e1,annahme,br.pos,bv.pos,nr.pos,sr.pos))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Zuordung der Abstimmungen zu den bestimmten Kategorien

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkZDFlMVtmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRkMWUxID09IFwiM1wiXSA8LSBcIk1pbGl0w6RyXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGQxZTFbZmluYW56aWVydW5nZW5fZ2VrdWVyenQkZDFlMSA9PSBcIjEyXCJdIDwtIFwiTWVkaWVuXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGQxZTFbZmluYW56aWVydW5nZW5fZ2VrdWVyenQkZDFlMSA9PSBcIjEwXCJdIDwtIFwiQUhWXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGQxZTFbZmluYW56aWVydW5nZW5fZ2VrdWVyenQkZDFlMSA9PSBcIjZcIl0gPC0gXCJBSFZcIlxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkZDFlMVtmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRkMWUxID09IFwiN1wiXSA8LSBcIkFIVlwiXG5maW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRkMWUxW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGQxZTEgPT0gXCI0XCJdIDwtIFwiQUhWXCJcbmBgYCJ9 -->

```r
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "3"] <- "Militär"
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "12"] <- "Medien"
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "10"] <- "AHV"
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "6"] <- "AHV"
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "7"] <- "AHV"
finanzierungen_gekuerzt$d1e1[finanzierungen_gekuerzt$d1e1 == "4"] <- "AHV"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Bei Position Bundesrat Zahlen mit Wörtern ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkYnIucG9zW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGJyLnBvcyA9PSBcIjFcIl0gPC0gXCJCZWbDvHJ3b3J0ZW5kXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGJyLnBvc1tmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRici5wb3MgPT0gXCIyXCJdIDwtIFwiQWJsZWhuZW5kXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGJyLnBvc1tmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRici5wb3MgPT0gXCIuXCJdIDwtIFwiRW50aGFsdGVuZFwiXG5gYGAifQ== -->

```r
finanzierungen_gekuerzt$br.pos[finanzierungen_gekuerzt$br.pos == "1"] <- "Befürwortend"
finanzierungen_gekuerzt$br.pos[finanzierungen_gekuerzt$br.pos == "2"] <- "Ablehnend"
finanzierungen_gekuerzt$br.pos[finanzierungen_gekuerzt$br.pos == "."] <- "Enthaltend"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Bei Position Parlament Zahlen mit Wörtern ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkYnYucG9zW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGJ2LnBvcyA9PSBcIjFcIl0gPC0gXCJCZWbDvHJ3b3J0ZW5kXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGJ2LnBvc1tmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRidi5wb3MgPT0gXCIyXCJdIDwtIFwiQWJsZWhuZW5kXCJcbmBgYCJ9 -->

```r
finanzierungen_gekuerzt$bv.pos[finanzierungen_gekuerzt$bv.pos == "1"] <- "Befürwortend"
finanzierungen_gekuerzt$bv.pos[finanzierungen_gekuerzt$bv.pos == "2"] <- "Ablehnend"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Bei Position Nationalrat Zahlen mit Wörtern ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkbnIucG9zW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JG5yLnBvcyA9PSBcIjFcIl0gPC0gXCJCZWbDvHJ3b3J0ZW5kXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JG5yLnBvc1tmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRuci5wb3MgPT0gXCIyXCJdIDwtIFwiQWJsZWhuZW5kXCJcbmBgYCJ9 -->

```r
finanzierungen_gekuerzt$nr.pos[finanzierungen_gekuerzt$nr.pos == "1"] <- "Befürwortend"
finanzierungen_gekuerzt$nr.pos[finanzierungen_gekuerzt$nr.pos == "2"] <- "Ablehnend"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Bei Position Ständerat Zahlen mit Wörtern ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkc3IucG9zW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JHNyLnBvcyA9PSBcIjFcIl0gPC0gXCJCZWbDvHJ3b3J0ZW5kXCJcbmZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JHNyLnBvc1tmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRzci5wb3MgPT0gXCIyXCJdIDwtIFwiQWJsZWhuZW5kXCJcbmBgYCJ9 -->

```r
finanzierungen_gekuerzt$sr.pos[finanzierungen_gekuerzt$sr.pos == "1"] <- "Befürwortend"
finanzierungen_gekuerzt$sr.pos[finanzierungen_gekuerzt$sr.pos == "2"] <- "Ablehnend"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### 0 und 1 durch "Abgelehnt" und "Angenommen" ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZmluYW56aWVydW5nZW5fZ2VrdWVyenQkYW5uYWhtZVtmaW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRhbm5haG1lID09IFwiMFwiXSA8LSBcIkFiZ2VsZWhudFwiXG5maW5hbnppZXJ1bmdlbl9nZWt1ZXJ6dCRhbm5haG1lW2ZpbmFuemllcnVuZ2VuX2dla3Vlcnp0JGFubmFobWUgPT0gXCIxXCJdIDwtIFwiQW5nZW5vbW1lblwiXG5gYGAifQ== -->

```r
finanzierungen_gekuerzt$annahme[finanzierungen_gekuerzt$annahme == "0"] <- "Abgelehnt"
finanzierungen_gekuerzt$annahme[finanzierungen_gekuerzt$annahme == "1"] <- "Angenommen"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


# Grafik 8: Analyse der Haltung des Bundesrates
#### Plot: Abstimmungen im Bereich AHV, Medien, Militär in Verbindung mit Finanzierung und ob diese angenommen oder abgelehnt wurden und die Position des Bundesrates

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGZpbmFuemllcnVuZ2VuX2dla3Vlcnp0KSArXG4gIGFlcyh4ID0gYW5uYWhtZSwgZmlsbCA9IGJyLnBvcykgK1xuICBnZW9tX2JhcigpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmxlaG5lbmQgPSBcIiMwQzRDOEFcIixcbiAgICBCZWbDvHJ3b3J0ZW5kID0gXCJ0YW4yXCIsXG4gICAgYEVudGhhbHRlbmRgID0gXCJncmV5XCIpXG4gICkgK1xuICBsYWJzKFxuICAgIHggPSBcIkVudHNjaGVpZHVuZyBkZXMgVm9sa2VzXCIsXG4gICAgeSA9IFwiQW56YWhsIEFic3RpbW11bmdlblwiLFxuICAgIHRpdGxlID0gXCJBbmFseXNlIGRlciBIYWx0dW5nIGRlcyBCdW5kZXNyYXRlc1wiLFxuICAgIGZpbGwgPSBcIlBvc2l0aW9uIEJ1bmRlc3JhdFwiXG4gICkgK1xuICB0aGVtZV9taW5pbWFsKCkgK1xuICB0aGVtZShcbiAgICBwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsXG4gICAgZmFjZSA9IFwiYm9sZFwiLFxuICAgIGhqdXN0ID0gMC41KSxcbiAgICBheGlzLnRpdGxlLnkgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTCksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpXG4gICkgK1xuICBmYWNldF93cmFwKHZhcnMoZDFlMSkpXG5gYGAifQ== -->

```r
ggplot(finanzierungen_gekuerzt) +
  aes(x = annahme, fill = br.pos) +
  geom_bar() +
  scale_fill_manual(
    values = c(Ablehnend = "#0C4C8A",
    Befürwortend = "tan2",
    `Enthaltend` = "grey")
  ) +
  labs(
    x = "Entscheidung des Volkes",
    y = "Anzahl Abstimmungen",
    title = "Analyse der Haltung des Bundesrates",
    fill = "Position Bundesrat"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  ) +
  facet_wrap(vars(d1e1))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



# Grafik 9: Analyse der Haltung des Parlaments
#### Plot: Abstimmungen im Bereich AHV, Medien, Militär in Verbindung mit Finanzierung und ob diese angenommen oder abgelehnt wurden und die Position des Parlaments

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGZpbmFuemllcnVuZ2VuX2dla3Vlcnp0KSArXG4gIGFlcyh4ID0gYW5uYWhtZSwgZmlsbCA9IGJ2LnBvcykgK1xuICBnZW9tX2JhcigpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmxlaG5lbmQgPSBcIiMwQzRDOEFcIixcbiAgICBCZWbDvHJ3b3J0ZW5kID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKFxuICAgIHggPSBcIkVudHNjaGVpZHVuZyBkZXMgVm9sa2VzXCIsXG4gICAgeSA9IFwiQW56YWhsIEFic3RpbW11bmdlblwiLFxuICAgIHRpdGxlID0gXCJBbmFseXNlIGRlciBIYWx0dW5nIGRlcyBQYXJsYW1lbnRzXCIsXG4gICAgZmlsbCA9IFwiUG9zaXRpb24gUGFybGFtZW50XCJcbiAgKSArXG4gIHRoZW1lX21pbmltYWwoKSArXG4gIHRoZW1lKFxuICAgIHBsb3QudGl0bGUgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDE1TCxcbiAgICBmYWNlID0gXCJib2xkXCIsXG4gICAgaGp1c3QgPSAwLjUpLFxuICAgIGF4aXMudGl0bGUueSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTFMKSxcbiAgICBheGlzLnRpdGxlLnggPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTClcbiAgKSArXG4gIGZhY2V0X3dyYXAodmFycyhkMWUxKSlcbmBgYCJ9 -->

```r
ggplot(finanzierungen_gekuerzt) +
  aes(x = annahme, fill = bv.pos) +
  geom_bar() +
  scale_fill_manual(
    values = c(Ablehnend = "#0C4C8A",
    Befürwortend = "tan2")
  ) +
  labs(
    x = "Entscheidung des Volkes",
    y = "Anzahl Abstimmungen",
    title = "Analyse der Haltung des Parlaments",
    fill = "Position Parlament"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  ) +
  facet_wrap(vars(d1e1))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


# Grafik 9.1: Analyse der Haltung des Nationalrates
#### Plot: Abstimmungen im Bereich AHV, Medien, Militär in Verbindung mit Finanzierung und ob diese angenommen oder abgelehnt wurden und die Position des Nationalrats

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGZpbmFuemllcnVuZ2VuX2dla3Vlcnp0KSArXG4gIGFlcyh4ID0gYW5uYWhtZSwgZmlsbCA9IG5yLnBvcykgK1xuICBnZW9tX2JhcigpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmxlaG5lbmQgPSBcIiMwQzRDOEFcIixcbiAgICBCZWbDvHJ3b3J0ZW5kID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKFxuICAgIHggPSBcIkVudHNjaGVpZHVuZyBkZXMgVm9sa2VzXCIsXG4gICAgeSA9IFwiQW56YWhsIEFic3RpbW11bmdlblwiLFxuICAgIHRpdGxlID0gXCJBbmFseXNlIGRlciBIYWx0dW5nIGRlcyBOYXRpb25hbHJhdGVzXCIsXG4gICAgZmlsbCA9IFwiUG9zaXRpb24gTmF0aW9uYWxyYXRcIlxuICApICtcbiAgdGhlbWVfbWluaW1hbCgpICtcbiAgdGhlbWUoXG4gICAgcGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLFxuICAgIGZhY2UgPSBcImJvbGRcIixcbiAgICBoanVzdCA9IDAuNSksXG4gICAgYXhpcy50aXRsZS55ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpLFxuICAgIGF4aXMudGl0bGUueCA9IGVsZW1lbnRfdGV4dChzaXplID0gMTFMKSxcbiAgKSArXG4gIGZhY2V0X3dyYXAodmFycyhkMWUxKSlcbmBgYCJ9 -->

```r
ggplot(finanzierungen_gekuerzt) +
  aes(x = annahme, fill = nr.pos) +
  geom_bar() +
  scale_fill_manual(
    values = c(Ablehnend = "#0C4C8A",
    Befürwortend = "tan2")
  ) +
  labs(
    x = "Entscheidung des Volkes",
    y = "Anzahl Abstimmungen",
    title = "Analyse der Haltung des Nationalrates",
    fill = "Position Nationalrat"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L),
  ) +
  facet_wrap(vars(d1e1))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



# Grafik 9.2: Analyse der Haltung des Ständerates

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGZpbmFuemllcnVuZ2VuX2dla3Vlcnp0KSArXG4gIGFlcyh4ID0gYW5uYWhtZSwgZmlsbCA9IHNyLnBvcykgK1xuICBnZW9tX2JhcigpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmxlaG5lbmQgPSBcIiMwQzRDOEFcIixcbiAgICBCZWbDvHJ3b3J0ZW5kID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKHggPSBcIkVudHNjaGVpZHVuZyBkZXMgVm9sa2VzXCIsIHkgPSBcIkFuemFobCBBYnN0aW1tdW5nZW5cIiwgdGl0bGUgPSAgXCJBbmFseXNlIGRlciBIYWx0dW5nIGRlcyBOYXRpb25hbHJhdGVzXCIsIGZpbGwgPSBcIlBvc2l0aW9uIFN0w6RuZGVyYXRcIikgK1xuICB0aGVtZV9taW5pbWFsKCkgK1xuICB0aGVtZShcbiAgICBwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsXG4gICAgZmFjZSA9IFwiYm9sZFwiLFxuICAgIGhqdXN0ID0gMC41KSxcbiAgICBheGlzLnRpdGxlLnkgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTCksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpXG4gICkgK1xuICBmYWNldF93cmFwKHZhcnMoZDFlMSkpXG5gYGAifQ== -->

```r
ggplot(finanzierungen_gekuerzt) +
  aes(x = annahme, fill = sr.pos) +
  geom_bar() +
  scale_fill_manual(
    values = c(Ablehnend = "#0C4C8A",
    Befürwortend = "tan2")
  ) +
  labs(x = "Entscheidung des Volkes", y = "Anzahl Abstimmungen", title =  "Analyse der Haltung des Nationalrates", fill = "Position Ständerat") +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  ) +
  facet_wrap(vars(d1e1))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

#### Die Grafik 9.1 und 9.2 haben wir nicht für den Blog verwendet, da sie genau das gleiche aussagen wie die Grafik 9.


#### Kontrollieren, ob Initiative nie angenommen, wenn Bundesrat dagegen. Wir wollten dies überprüfen, da wir uns dies nicht vorstellen konnten. Bei unseren untersuchten Abstimmungen ist dies jedoch tatsächlich der Fall.

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuYnVuZGVzcmF0RGFnZWdlbiA8LSBmaWx0ZXIoZmluYW56aWVydW5nZW4sIGJyLnBvcyA9PSAyKVxuYGBgIn0= -->

```r
bundesratDagegen <- filter(finanzierungen, br.pos == 2)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxudGVzdF9maW5hbnppZXJ1bmdlbiA8LSBidW5kZXNyYXREYWdlZ2VuICU+JVxuICBncm91cF9ieShhbm5haG1lKSAlPiVcbiAgY291bnQoKVxuYGBgIn0= -->

```r
test_finanzierungen <- bundesratDagegen %>%
  group_by(annahme) %>%
  count()
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>
# Grafik 10: Wähleranteil untersuchen
#### Data Frame erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuamFMYWdlcjwtIHN1YnNldChhaHZfRmluYW56aWVydW5nLCBzZWxlY3QgPSBjKHRpdGVsX2t1cnpfZCwgYW5uYWhtZSwgamEubGFnZXIpKVxuYGBgIn0= -->

```r
jaLager<- subset(ahv_Finanzierung, select = c(titel_kurz_d, annahme, ja.lager))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Die Zahl in Spalte ja Lager als numeric speichern

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuamFMYWdlciRqYS5sYWdlciA8LSBhcy5udW1lcmljKGphTGFnZXIkamEubGFnZXIpXG5gYGAifQ== -->

```r
jaLager$ja.lager <- as.numeric(jaLager$ja.lager)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot: Wähleranteil der Parteien die die Initiative unterstützen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGphTGFnZXIpICtcbiAgYWVzKHggPSBqYS5sYWdlciwgeSA9IHRpdGVsX2t1cnpfZCwgZmlsbCA9IGFubmFobWUpICtcbiAgZ2VvbV90aWxlKHNpemUgPSAxLjIpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmdlbGVobnQgPSBcIiMwQzRDOEFcIixcbiAgICBBbmdlbm9tbWVuID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKHggPSBcIlfDpGhsZXJhbnRlaWwgaW4gUHJvemVudFwiLCB5ID0gXCJJbml0aWF0aXZlXCIsIHRpdGxlID0gXCJadXNhbW1lbmhhbmcgendpc2NoZW4gV8OkaGxlcmFudGVpbCB1bmQgRXJnZWJuaXMgZGVyIEluaXRpYXRpdmVcIiwgZmlsbCA9IFwiRXJnZWJuaXNcIikgK1xuICB0aGVtZV9taW5pbWFsKCkgK1xuICB0aGVtZShcbiAgICBwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsXG4gICAgaGp1c3QgPSAwKSxcbiAgICBheGlzLnRpdGxlLnkgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTCksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpXG4gIClcbmBgYCJ9 -->

```r
ggplot(jaLager) +
  aes(x = ja.lager, y = titel_kurz_d, fill = annahme) +
  geom_tile(size = 1.2) +
  scale_fill_manual(
    values = c(Abgelehnt = "#0C4C8A",
    Angenommen = "tan2")
  ) +
  labs(x = "Wähleranteil in Prozent", y = "Initiative", title = "Zusammenhang zwischen Wähleranteil und Ergebnis der Initiative", fill = "Ergebnis") +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    hjust = 0),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  )
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>
# Grafik 11: Ergebnisse der Initiativen nach Rechtsform
#### Spaltennamen angepasst

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGF0ZW5fdW1iZW5hbm50IDwtIGRhdGFfY3N2ICU+JVxuICBtdXRhdGUoYW5uYWhtZSA9IGZhY3RvcihjYXNlX3doZW4oXG4gICAgYW5uYWhtZSA9PSAwIH4gXCJBYmdlbGVobnRcIiwgXG4gICAgYW5uYWhtZSA9PSAxIH4gXCJBbmdlbm9tbWVuXCIpKSkgJT4lXG4gIG11dGF0ZShyZWNodHNmb3JtID0gZmFjdG9yKGNhc2Vfd2hlbihcbiAgICByZWNodHNmb3JtID09IDEgfiBcIk9ibGlnYXRvcmlzY2hlcyBSZWZlcmVuZHVtXCIsXG4gICAgcmVjaHRzZm9ybSA9PSAyIH4gXCJGYWt1bHRhdGl2ZXMgUmVmZXJlbmR1bVwiLCBcbiAgICByZWNodHNmb3JtID09IDMgfiBcIlZvbGtzaW5pdGlhdGl2ZVwiLCBcbiAgICByZWNodHNmb3JtID09IDQgfiBcIkdlZ2VuZW50d3VyZiB6dSBWb2xrc2luaXRpYXRpdmVcIixcbiAgICByZWNodHNmb3JtID09IDUgfiBcIlN0aWNoZnJhZ2VcIikpKVxuYGBgIn0= -->

```r
daten_umbenannt <- data_csv %>%
  mutate(annahme = factor(case_when(
    annahme == 0 ~ "Abgelehnt", 
    annahme == 1 ~ "Angenommen"))) %>%
  mutate(rechtsform = factor(case_when(
    rechtsform == 1 ~ "Obligatorisches Referendum",
    rechtsform == 2 ~ "Fakultatives Referendum", 
    rechtsform == 3 ~ "Volksinitiative", 
    rechtsform == 4 ~ "Gegenentwurf zu Volksinitiative",
    rechtsform == 5 ~ "Stichfrage")))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Nur angelehnte und angenommene Initiativen beachtet

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGF0ZW5fb2huZU5BIDwtIGZpbHRlcihkYXRlbl91bWJlbmFubnQsIGFubmFobWUgPT0gXCJBYmdlbGVobnRcIiB8IGFubmFobWUgPT0gXCJBbmdlbm9tbWVuXCIpXG5gYGAifQ== -->

```r
daten_ohneNA <- filter(daten_umbenannt, annahme == "Abgelehnt" | annahme == "Angenommen")
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuSmFocmUgPSBhcy5EYXRlKGRhdGVuX29obmVOQSRkYXR1bSxcIiVkLiVtLiVZXCIpIFxuZ2dwbG90KGRhdGVuX29obmVOQSwgYWVzKEphaHJlLCBkYXRlbl9vaG5lTkEkcmVjaHRzZm9ybSlcbikgK1xuICBnZW9tX2ppdHRlcigpK1xuICBzY2FsZV9maWxsX21hbnVhbChcbiAgICB2YWx1ZXMgPSBjKCdBYmdlbGVobnQnID0gXCIjMEM0QzhBXCIsXG4gICAgJ0FuZ2Vub21tZW4nID0gXCJ0YW4yXCIpXG4gICkgK1xuICBmYWNldF9ncmlkKH5hbm5haG1lKStcbiAgbGFicyh4ID0gXCJKYWhyZVwiLCB5ID0gXCJSZWNodHNmb3JtXCIsIHRpdGxlID0gXCJFcmdlYm5pc3NlIGRlciBJbml0aWF0aXZlbiBuYWNoIFJlY2h0c2Zvcm1cIikrXG4gIHRoZW1lX21pbmltYWwoKStcbiAgdGhlbWUoXG4gICAgcGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLFxuICAgIGZhY2UgPSBcImJvbGRcIixcbiAgICBoanVzdCA9IDAuNSksXG4gICAgYXhpcy50aXRsZS55ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpLFxuICAgIGF4aXMudGl0bGUueCA9IGVsZW1lbnRfdGV4dChzaXplID0gMTFMKVxuICApXG5gYGAifQ== -->

```r
Jahre = as.Date(daten_ohneNA$datum,"%d.%m.%Y") 
ggplot(daten_ohneNA, aes(Jahre, daten_ohneNA$rechtsform)
) +
  geom_jitter()+
  scale_fill_manual(
    values = c('Abgelehnt' = "#0C4C8A",
    'Angenommen' = "tan2")
  ) +
  facet_grid(~annahme)+
  labs(x = "Jahre", y = "Rechtsform", title = "Ergebnisse der Initiativen nach Rechtsform")+
  theme_minimal()+
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  )
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

<br>
<br>

# Grafik 12: Wie der Tenor der Medien gegenüber den Initiativen war

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuTWVkaWF0b24gPSBhcy5udW1lcmljKGRhdGVuX29obmVOQSRtZWRpYXRvbi50b3QpXG5nZ3Bsb3QoZGF0ZW5fb2huZU5BLCBhZXMoTWVkaWF0b24sIGRhdGVuX29obmVOQSRyZWNodHNmb3JtKVxuKSArXG4gIGdlb21fcG9pbnQoKStcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYygnQWJnZWxlaG50JyA9IFwiIzBDNEM4QVwiLFxuICAgICdBbmdlbm9tbWVuJyA9IFwidGFuMlwiKVxuICApICtcbiAgbGFicyh4ID0gXCJNZWRpZW50b25cIiwgeSA9IFwiUmVjaHRzZm9ybVwiLCB0aXRsZSA9IFwiV2llIGRlciBUZW5vciBkZXIgTWVkaWVuIGdlZ2Vuw7xiZXIgZGVuIEluaXRpYXRpdmVuIHdhclwiKStcbiAgdGhlbWVfbWluaW1hbCgpK1xuICB0aGVtZShcbiAgICBwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsXG4gICAgZmFjZSA9IFwiYm9sZFwiLFxuICAgIGhqdXN0ID0gMC41KSxcbiAgICBheGlzLnRpdGxlLnkgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTCksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpXG4gICkrXG4gIGZhY2V0X3dyYXAofmFubmFobWUpXG5gYGAifQ== -->

```r
Mediaton = as.numeric(daten_ohneNA$mediaton.tot)
ggplot(daten_ohneNA, aes(Mediaton, daten_ohneNA$rechtsform)
) +
  geom_point()+
  scale_fill_manual(
    values = c('Abgelehnt' = "#0C4C8A",
    'Angenommen' = "tan2")
  ) +
  labs(x = "Medienton", y = "Rechtsform", title = "Wie der Tenor der Medien gegenüber den Initiativen war")+
  theme_minimal()+
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L)
  )+
  facet_wrap(~annahme)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

<br>
<br>
# Grafik 13: Anzahl befürwortende Inserate

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuSW5zZXJhdGUgPSBhcy5udW1lcmljKGRhdGVuX29obmVOQSRpbnNlcmF0ZS5qYSlcblxuZ2dwbG90KGRhdGVuX29obmVOQSwgYWVzKEluc2VyYXRlLCBkYXRlbl9vaG5lTkEkcmVjaHRzZm9ybSlcbikgK1xuICBnZW9tX2ppdHRlcigpK1xuICBzY2FsZV9maWxsX21hbnVhbChcbiAgICB2YWx1ZXMgPSBjKCdBYmdlbGVobnQnID0gXCIjMEM0QzhBXCIsXG4gICAgJ0FuZ2Vub21tZW4nID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKHggPSBcIkFuemFobCBkZXIgSW5zZXJhdGVcIiwgeSA9IFwiXCIsIHRpdGxlID0gXCJBbnphaGwgYmVmw7xyd29ydGVuZGUgSW5zZXJhdGVcIikrXG4gIHRoZW1lX21pbmltYWwoKStcbiAgdGhlbWUoXG4gICAgcGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLFxuICAgIGZhY2UgPSBcImJvbGRcIixcbiAgICBoanVzdCA9IDAuNSksXG4gICAgYXhpcy50aXRsZS55ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpLFxuICAgIGF4aXMudGl0bGUueCA9IGVsZW1lbnRfdGV4dChzaXplID0gMTFMKSxcbiAgKStcbiAgZmFjZXRfd3JhcCh+YW5uYWhtZSlcbmBgYCJ9 -->

```r
Inserate = as.numeric(daten_ohneNA$inserate.ja)

ggplot(daten_ohneNA, aes(Inserate, daten_ohneNA$rechtsform)
) +
  geom_jitter()+
  scale_fill_manual(
    values = c('Abgelehnt' = "#0C4C8A",
    'Angenommen' = "tan2")
  ) +
  labs(x = "Anzahl der Inserate", y = "", title = "Anzahl befürwortende Inserate")+
  theme_minimal()+
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L),
  )+
  facet_wrap(~annahme)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

<br>
<br>

# Grafik 14: Anzahl ablehnende Inserate im Vergleich zum Ergebnis der Initiativen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuSW5zZXJhdGUgPSBhcy5udW1lcmljKGRhdGVuX29obmVOQSRpbnNlcmF0ZS5uZWluKVxuXG5nZ3Bsb3QoZGF0ZW5fb2huZU5BLCBhZXMoSW5zZXJhdGUsIGRhdGVuX29obmVOQSRyZWNodHNmb3JtKVxuKSArXG4gIGdlb21faml0dGVyKCkrXG4gIHNjYWxlX2ZpbGxfbWFudWFsKFxuICAgIHZhbHVlcyA9IGMoJ0FiZ2VsZWhudCcgPSBcIiMwQzRDOEFcIixcbiAgICAnQW5nZW5vbW1lbicgPSBcInRhbjJcIilcbiAgKSArXG4gIGxhYnMoeCA9IFwiQW56YWhsIGRlciBJbnNlcmF0ZVwiLCB5ID0gXCJSZWNodHNmb3JtXCIsIHRpdGxlID0gXCJBbnphaGwgYWJsZWhuZW5kZSBJbnNlcmF0ZSBpbSBWZXJnbGVpY2ggenVtIEVyZ2VibmlzIGRlciBJbml0aWF0aXZlblwiKStcbiAgdGhlbWVfbWluaW1hbCgpK1xuICB0aGVtZShcbiAgICBwbG90LnRpdGxlID0gZWxlbWVudF90ZXh0KHNpemUgPSAxNUwsXG4gICAgZmFjZSA9IFwiYm9sZFwiLFxuICAgIGhqdXN0ID0gMC41KSxcbiAgICBheGlzLnRpdGxlLnkgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDExTCksXG4gICAgYXhpcy50aXRsZS54ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMUwpLFxuICApK1xuICBmYWNldF93cmFwKH5hbm5haG1lKVxuYGBgIn0= -->

```r
Inserate = as.numeric(daten_ohneNA$inserate.nein)

ggplot(daten_ohneNA, aes(Inserate, daten_ohneNA$rechtsform)
) +
  geom_jitter()+
  scale_fill_manual(
    values = c('Abgelehnt' = "#0C4C8A",
    'Angenommen' = "tan2")
  ) +
  labs(x = "Anzahl der Inserate", y = "Rechtsform", title = "Anzahl ablehnende Inserate im Vergleich zum Ergebnis der Initiativen")+
  theme_minimal()+
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 11L),
    axis.title.x = element_text(size = 11L),
  )+
  facet_wrap(~annahme)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->












<br>
<br>

# Grafik 1 (Demokratie): Übersicht in welcher Rechtsform wie oft abgestimmt wurde seit 1848
#### Data Frame anpassen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGF0YV9jc3YkcmVjaHRzZm9ybVtkYXRhX2NzdiRyZWNodHNmb3JtID09IFwiMVwiXSA8LSBcIk9ibGlnYXRvcmlzY2hlcyBSZWZlcmVuZHVtXCJcbmRhdGFfY3N2JHJlY2h0c2Zvcm1bZGF0YV9jc3YkcmVjaHRzZm9ybSA9PSBcIjJcIl0gPC0gXCJGYWt1bHRhdGl2ZXMgUmVmZXJlbmR1bVwiXG5kYXRhX2NzdiRyZWNodHNmb3JtW2RhdGFfY3N2JHJlY2h0c2Zvcm0gPT0gXCIzXCJdIDwtIFwiVm9sa3Npbml0aWF0aXZlXCJcbmRhdGFfY3N2JHJlY2h0c2Zvcm1bZGF0YV9jc3YkcmVjaHRzZm9ybSA9PSBcIjRcIl0gPC0gXCJEaXJla3RlciBHZWdlbmVudHd1cmYgZWluZXIgVm9sa3Npbml0aWF0aXZlXCJcbmRhdGFfY3N2JHJlY2h0c2Zvcm1bZGF0YV9jc3YkcmVjaHRzZm9ybSA9PSBcIjVcIl0gPC0gXCJTdGljaGZyYWdlXCJcbmBgYCJ9 -->

```r
data_csv$rechtsform[data_csv$rechtsform == "1"] <- "Obligatorisches Referendum"
data_csv$rechtsform[data_csv$rechtsform == "2"] <- "Fakultatives Referendum"
data_csv$rechtsform[data_csv$rechtsform == "3"] <- "Volksinitiative"
data_csv$rechtsform[data_csv$rechtsform == "4"] <- "Direkter Gegenentwurf einer Volksinitiative"
data_csv$rechtsform[data_csv$rechtsform == "5"] <- "Stichfrage"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


#### Plot erstellen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGRhdGFfY3N2KSArXG4gIGFlcyh4ID0gcmVjaHRzZm9ybSwgZmlsbCA9IHJlY2h0c2Zvcm0pICtcbiAgZ2VvbV9iYXIoKSArXG4gIHNjYWxlX2ZpbGxfbWFudWFsKFxuICAgIHZhbHVlcyA9IGMoYERpcmVrdGVyIEdlZ2VuZW50d3VyZiBlaW5lciBWb2xrc2luaXRpYXRpdmVgID0gXCIjQTUwMDI2XCIsXG4gICAgYEZha3VsdGF0aXZlcyBSZWZlcmVuZHVtYCA9IFwiI0Y4OEQ1MVwiLFxuICAgIGBPYmxpZ2F0b3Jpc2NoZXMgUmVmZXJlbmR1bWAgPSBcIiNFNkQzODZcIixcbiAgICBTdGljaGZyYWdlID0gXCIjOEZDM0REXCIsXG4gICAgVm9sa3Npbml0aWF0aXZlID0gXCIjMzEzNjk1XCIpXG4gICkgK1xuICBsYWJzKHkgPSBcIkFuemFobCBJbml0aWF0aXZlblwiLCBmaWxsID0gXCJSZWNodHNmb3JtXCIpICtcbiAgdGhlbWVfbWluaW1hbCgpICtcbiAgdGhlbWUoXG4gICAgYXhpcy50aXRsZS55ID0gZWxlbWVudF90ZXh0KHNpemUgPSAxMkwsXG4gICAgZmFjZSA9IFwiYm9sZFwiKVxuICApK1xuICAgIGxhYnMoXG4gICAgdGl0bGUgPSBcIlJlY2h0c2Zvcm0gZGVyIEluaXRpYXRpdmVuIHNlaXQgMTg0OFwiKStcbiAgdGhlbWUocGxvdC50aXRsZSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTVMLCBoanVzdCA9IDAuNSkpXG5gYGAifQ== -->

```r
ggplot(data_csv) +
  aes(x = rechtsform, fill = rechtsform) +
  geom_bar() +
  scale_fill_manual(
    values = c(`Direkter Gegenentwurf einer Volksinitiative` = "#A50026",
    `Fakultatives Referendum` = "#F88D51",
    `Obligatorisches Referendum` = "#E6D386",
    Stichfrage = "#8FC3DD",
    Volksinitiative = "#313695")
  ) +
  labs(y = "Anzahl Initiativen", fill = "Rechtsform") +
  theme_minimal() +
  theme(
    axis.title.y = element_text(size = 12L,
    face = "bold")
  )+
    labs(
    title = "Rechtsform der Initiativen seit 1848")+
  theme(plot.title = element_text(size = 15L, hjust = 0.5))
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->


<br>
<br>

# Grafik 2 (Demokratie): Alle Volksinitiativen verteilt auf die Jahre seit 1848 und ob diese angenommen oder abgelehnt wurden
#### Data Frame kürzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuaW5pdGlhdGl2ZW4gPC0gIHN1YnNldChkYXRhX2NzdiwgcmVjaHRzZm9ybSA9PSBcIlZvbGtzaW5pdGlhdGl2ZVwiKVxuYGBgIn0= -->

```r
initiativen <-  subset(data_csv, rechtsform == "Volksinitiative")
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

#### Bei AHV Datensatz Spalte annahme 1,0 durch "angenommen" und "abgelehnt" ersetzen

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuaW5pdGlhdGl2ZW4kYW5uYWhtZVtpbml0aWF0aXZlbiRhbm5haG1lID09IFwiMFwiXSA8LSBcIkFiZ2VsZWhudFwiXG5pbml0aWF0aXZlbiRhbm5haG1lW2luaXRpYXRpdmVuJGFubmFobWUgPT0gXCIxXCJdIDwtIFwiQW5nZW5vbW1lblwiXG5gYGAifQ== -->

```r
initiativen$annahme[initiativen$annahme == "0"] <- "Abgelehnt"
initiativen$annahme[initiativen$annahme == "1"] <- "Angenommen"
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->

#### Plot

<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwbG90KGluaXRpYXRpdmVuKSArXG4gIGFlcyh4ID0gZGF0dW0sIGZpbGwgPSBhbm5haG1lKSArXG4gIGdlb21faGlzdG9ncmFtKGJpbnMgPSAzMEwpICtcbiAgc2NhbGVfZmlsbF9tYW51YWwoXG4gICAgdmFsdWVzID0gYyhBYmdlbGVobnQgPSBcIiMwQzRDOEFcIixcbiAgICBBbmdlbm9tbWVuID0gXCJ0YW4yXCIpXG4gICkgK1xuICBsYWJzKFxuICAgIHggPSBcIiBcIixcbiAgICB5ID0gXCJBbnphaGwgVm9sa3Npbml0aWF0aXZlblwiLFxuICAgIHRpdGxlID0gXCJWb2xrc2luaXRpYXRpdmVuIHNlaXQgMTg0OFwiLFxuICAgIGZpbGwgPSBcIkVyZ2VibmlzXCJcbiAgKSArXG4gIHRoZW1lX21pbmltYWwoKSArXG4gIHRoZW1lKFxuICAgIHBsb3QudGl0bGUgPSBlbGVtZW50X3RleHQoc2l6ZSA9IDE1TCxcbiAgICBmYWNlID0gXCJib2xkXCIsXG4gICAgaGp1c3QgPSAwLjUpLFxuICAgIGF4aXMudGl0bGUueSA9IGVsZW1lbnRfdGV4dChzaXplID0gMTJMLFxuICAgIGZhY2UgPSBcImJvbGRcIilcbiAgKVxuYGBgIn0= -->

```r
ggplot(initiativen) +
  aes(x = datum, fill = annahme) +
  geom_histogram(bins = 30L) +
  scale_fill_manual(
    values = c(Abgelehnt = "#0C4C8A",
    Angenommen = "tan2")
  ) +
  labs(
    x = " ",
    y = "Anzahl Volksinitiativen",
    title = "Volksinitiativen seit 1848",
    fill = "Ergebnis"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(size = 15L,
    face = "bold",
    hjust = 0.5),
    axis.title.y = element_text(size = 12L,
    face = "bold")
  )
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->

