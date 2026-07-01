# Prognose des BIP-Wachstums Norwegens

![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![R Markdown](https://img.shields.io/badge/R%20Markdown-Report-75AADB)
![Zeitreihen](https://img.shields.io/badge/Methode-Zeitreihenanalyse-2E7D32)

Eine ökonometrische Zeitreihenanalyse, die die jährliche **BIP-Wachstumsrate Norwegens**
(1991–2024) mit **AR-** und **ADL-Modellen** prognostiziert. Als Frühindikator dient die
Arbeitslosenquote, motiviert durch die *Okunsche Beziehung* zwischen Wirtschaftswachstum
und Arbeitsmarkt.

> Studienleistung im Kurs *Ökonometrie und Zeitreihenanalyse* (HfWU Nürtingen-Geislingen, SoSe 2026).
> Vollständig in **R / R Markdown** umgesetzt und reproduzierbar.

📄 **Der fertige Bericht (4 Seiten, PDF):** [`output/bericht.pdf`](output/bericht.pdf)

---

## Worum geht es?

Ziel ist eine echte Out-of-Sample-Prognose: Die Modelle werden auf den Jahren **1991–2017**
trainiert und auf den **ungesehenen Jahren 2018–2024** getestet. So lässt sich messen, wie gut
ein Modell *wirklich* vorhersagt – und nicht nur, wie gut es vergangene Daten nachzeichnet.

**Kernschritte der Analyse:**

- **Datenaufbereitung** dreier World-Bank-Zeitreihen (BIP-Wachstum, BIP-Niveau, Arbeitslosenquote)
- **Stationaritätsanalyse** mit ADF- und KPSS-Test sowie ACF/PACF; Entscheidung zur Differenzierung
  über eine *Triangulation* mehrerer Kriterien statt eines einzelnen Tests
- **Schätzung & Vergleich von fünf Modellen:** AR(1), AR(2), ADL(1,1), ADL(2,2) und
  ADL(2,2)+Krisen-Dummy 2009
- **Modelldiagnostik** mit dem Breusch-Godfrey-Test auf Residual-Autokorrelation
- **Bewertung der Prognosegüte** über RMSE und MAE auf Niveau­ebene, abgesichert mit einem
  Diebold-Mariano-Test

## Wichtigste Ergebnisse

- Das **ADL(2,2)+D2009** erzielt den niedrigsten Prognosefehler – rund 30 % unter dem
  einfachen AR(1)-Benchmark.
- Die Arbeitslosenquote enthält erkennbare **Vorlaufinformation** für das BIP-Wachstum.
- Ehrliche Einordnung der Grenzen: Bei nur 7 Testjahren ist der Vorsprung statistisch
  **nicht formal abgesichert** (Diebold-Mariano), und der COVID-19-Einbruch 2020 ist als
  Out-of-Distribution-Schock grundsätzlich nicht prognostizierbar.

## Projektstruktur

```
bip-prognose-norwegen/
├── zeitreihen_analyse.Rmd     # kompletter Analyse-Code (R Markdown)
├── daten/                     # Rohdaten (World Bank, World Development Indicators)
│   ├── bip_wachstum.csv
│   ├── bip_niveau.csv
│   └── arbeitslosenquote.csv
├── output/
│   ├── bericht.pdf            # gerenderter Bericht
│   └── praesentation.pptx     # Präsentationsfolien
└── README.md
```

## Selbst reproduzieren

**Voraussetzungen:** R (≥ 4.0), gerne mit RStudio, und eine LaTeX-Installation für die
PDF-Ausgabe (am einfachsten via `tinytex::install_tinytex()`).

```r
# benötigte Pakete einmalig installieren
install.packages(c("forecast", "tseries", "lmtest", "dynlm", "knitr", "rmarkdown"))

# Bericht erzeugen (im Projektordner ausführen)
rmarkdown::render("zeitreihen_analyse.Rmd")
```

Alle Datenpfade sind **relativ**, das Projekt läuft also direkt nach dem Klonen –
ohne dass Pfade angepasst werden müssen.

## Daten

Quelle: **World Bank – World Development Indicators** (öffentlich zugänglich).
Indikatoren: `NY.GDP.MKTP.KD.ZG` (BIP-Wachstum), `NY.GDP.MKTP.CD` (BIP-Niveau),
`SL.UEM.TOTL.ZS` (Arbeitslosenquote).

## Verwendete Methoden & Werkzeuge

`R` · `R Markdown` · AR- & ADL-Modelle · ADF/KPSS-Stationaritätstests ·
Breusch-Godfrey · Diebold-Mariano · Train/Test-Validierung · RMSE/MAE

---

**Autor:** Pablo Heinold
