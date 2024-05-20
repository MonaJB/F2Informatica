# Progetto di Simulazione e Analisi dei Comportamenti dei Guidatori

Ho completato un progetto che coinvolge la simulazione dei dati provenienti da una black box installata su veicoli e l'analisi dettagliata dei comportamenti dei guidatori. Di seguito, una breve descrizione del lavoro svolto:

## Descrizione del Progetto

Ho utilizzato Python per simulare il comportamento di vari guidatori, generando dati accurati come timestart, timestamp, accelerazione media, accelerazione massima, velocità media e velocità massima. Questi dati simulati offrono una panoramica completa delle prestazioni dei veicoli in intervalli di un minuto.

Successivamente, ho archiviato questi dati in un database PostgreSQL, garantendo la conservazione e l'accessibilità efficiente di un vasto volume di informazioni. Il database funge da repository centrale per gestire i dati in modo efficace.

## Analisi con Power BI

Su Power BI, ho creato misure e colonne che consentono la classificazione dei guidatori in categorie come "cauto", "normale" o "spericolato". Power BI offre una vasta gamma di strumenti di visualizzazione che ho sfruttato per creare grafici e rappresentazioni grafiche dei dati. Questi strumenti permettono di estrarre ulteriori informazioni dai dati, rendendo la dashboard un'importante risorsa per la comprensione dettagliata del comportamento dei guidatori e delle condizioni di guida.

In particolare, è possibile esaminare l'andamento dettagliato della velocità e dell'accelerazione per giorni specifici, il che può rivelarsi fondamentale in situazioni come incidenti stradali o eventi rilevanti in cui è cruciale comprendere come il veicolo sia stato guidato in una data specifica.

## Visualizzazione su Power BI

Qui di seguito, puoi visualizzare uno screenshot della presentazione su Power BI, dove è possibile scegliere il tipo di targa e analizzare informazioni dettagliate sul comportamento dei guidatori. 
## Il foglio di Main
![Screenshot 2023-11-17 094735](https://github.com/MonaJB/F2Informatica/blob/fbdcaa772a079478f5fc7b1b7edad765076adca5/Screenshot%202024-05-08%20165743.png)
## Il foglio di Andamento di Velocità e Accelerazione 
![Screenshot 2023-11-17 094817](https://github.com/MonaJB/F2Informatica/blob/fbdcaa772a079478f5fc7b1b7edad765076adca5/Screenshot%202024-05-08%20165850.png)

## I misuri Usati:
Derivare la Data riguarda al TimeStamp:
```
Data = FORMAT(DATE(1970, 1, 1) + [timestamp]/86400, "yyyy-MM-dd HH:mm")
```

Prendere il giorno della settimana:
```
GiornoSettimanaNomeCompleto = 
SWITCH(
    'progettoBlackBox'[giorno della settimana],
    1, "7-Domenica",
    2, "1-Lunedì",
    3, "2-Martedì",
    4, "3-Mercoledì",
    5, "4-Giovedì",
    6, "5-Venerdì",
    7, "6-Sabato",
    BLANK()
)
```
Specificare il giorno se è il fine settimana o no:
```
WeekOrWeekend = 
SWITCH(
    'progettoBlackBox'[giorno della settimana],
    1, "Fine settimana",
    2, "Giorni lavorativi",
    3, "Giorni lavorativi",
    4, "Giorni lavorativi",
    5, "Giorni lavorativi",
    6, "Giorni lavorativi",
    7, "Fine settimana",
    BLANK()
)
```
Calculare la media di media distanza per ogni giorno:
```
media Distanza Media per giorno settimana in KM = 
AVERAGEX(
    SUMMARIZE(
        'progettoBlackBox',
        'progettoBlackBox'[giorno della settimana],
        "Somma Giornaliera", AVERAGE('progettoBlackBox'[distanza media])
    ),
    [Somma Giornaliera] / 1000
)
```
Calcolare la Media di Media distanza per ogni giorno:
```
Media giornaliera Distanza Media in KM = 
AVERAGEX(
    SUMMARIZE(
        'progettoBlackBox',
        'progettoBlackBox'[Giorno],
        "Somma Giornaliera", SUM('progettoBlackBox'[distanza media])
    ),
    [Somma Giornaliera] / 1000
)
```
Calcolare la velocità media per ogni giorno:
```
Velocita Media per ogni Giorno = 
AVERAGEX(
    VALUES('progettoBlackBox'[Giorno]),  -- Sostituisci "Tabella" con il nome della tua tabella e "Data" con il nome della colonna della data
    CALCULATE(
        AVERAGE(progettoBlackBox[velocita])  -- Sostituisci "Tabella" con il nome della tua tabella e "Velocita" con il nome della colonna della velocità
    )
)
```
Contare i giorni in cui utilizzati l'auto tra 30 precedenti giorni:
```
Numero Giorni Utilizzo Ultimi 30 Giorni = 
CALCULATE(
    DISTINCTCOUNT('progettoBlackBox'[Giorno]),
    'progettoBlackBox'[Data] > (MAX('progettoBlackBox'[Data]) - 30) &&
    'progettoBlackBox'[Data] <= MAX('progettoBlackBox'[Data])
)
```
Dare un livello per ogni record:
```
label_01 = IF('progettoBlackBox'[velocita] <= 60 &&'progettoBlackBox'[accelerazione_max] >= -1.1 &&'progettoBlackBox'[accelerazione_max] <= 1 &&'progettoBlackBox'[velocita_max] <= 75,0,1)
```
Calcolare un valore a nome Percentuale Etichetta che conta i valori di label_01 per ogni l'auto:
```
Percentuale Etichetta 1 = 
DIVIDE(
    CALCULATE(
        COUNTROWS('progettoBlackBox'),  -- Conta le righe
        'progettoBlackBox'[label_01] = 1  -- Conta solo dove label_01 è 1
    ),
    CALCULATE(
        COUNTROWS('progettoBlackBox') ),
    0
)
```
Definire i tipi della guida in base ai valori di 'Precentuale Etichetta 1' da cui calcoliamo nel precendete formula:
```
Tipo_01 = 
SWITCH(
    TRUE(),
    [Percentuale Etichetta 1] >= 0 && [Percentuale Etichetta 1] <= 0.2, "Cauto",
    [Percentuale Etichetta 1] > 0.2 && [Percentuale Etichetta 1] <= 0.5, "Normale",
    [Percentuale Etichetta 1] > 0.5 && [Percentuale Etichetta 1] <= 1.2, "Spericolato",
    "Valore non valido"
)
```
Gli altri valori che si vengono mostrato sulla Dashboard:

```
Percentuale Velocità = 
DIVIDE(
    COUNTROWS('progettoBlackBox'),
    CALCULATE(
        COUNTROWS('progettoBlackBox'),
        ALL('progettoBlackBox'[Fascia Velocità])
    )
)
```

```
Somma Distanza Media Ultimi 30 Giorni in km = DIVIDE(
CALCULATE(
    SUM('progettoBlackBox'[distanza media]),
    'progettoBlackBox'[Data] > (MAX('progettoBlackBox'[Data]) - 30) &&
    'progettoBlackBox'[Data] <= MAX('progettoBlackBox'[Data])
), 1000)
```
