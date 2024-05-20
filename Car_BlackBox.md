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
drivere Data riguarda al TimeStamp:

Data = FORMAT(DATE(1970, 1, 1) + [timestamp]/86400, "yyyy-MM-dd HH:mm")

