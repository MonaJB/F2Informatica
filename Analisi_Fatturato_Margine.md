# Analisi Fatturato, Margine e Costi per Cliente e Risorse

## Introduzione

Finalizzati al controllo della redditività aziendale, per migliorare le strategie aziendali e prendere decisioni informate, sono stati analizzati i dati del fatturato: i costi e i margini per anno, mese, commessa, cliente e risorsa. Questo progetto fornisce una visione chiara e dettagliata delle performance finanziarie dell'azienda utilizzando una dashboard interattiva in Power BI.

## Struttura della Dashboard

### 1. Fatturato

Il primo foglio della dashboard, denominato **"Fatturato"**, presenta il totale annuale e mensile del fatturato. Questo foglio include una rappresentazione grafica chiara dei valori di fatturato per ogni cliente, con una suddivisione percentuale per ogni commessa. Questo permette di visualizzare rapidamente le performance finanziarie dell'azienda e identificare i principali contributori al fatturato.



*Nella foto sopra, possiamo vedere la scheda "Fatturato" con i dettagli del totale annuale e mensile, e la rappresentazione grafica per ogni cliente.*

### 2. Margine

Il secondo foglio, chiamato **"Margine"**, offre un'analisi simile a quella del fatturato, ma con un focus sui margini di profitto. Anche in questo caso, vengono visualizzati il totale annuale e mensile del margine, insieme a una rappresentazione grafica per ogni cliente e la percentuale per ogni commessa. Questo foglio è fondamentale per identificare le aree di maggiore redditività e ottimizzare le strategie aziendali.



*Nella foto sopra, possiamo vedere la scheda "Margine" con i dettagli del totale annuale e mensile, e la rappresentazione grafica per ogni cliente.*

### 3. Risorsa

Il terzo foglio della dashboard, intitolato **"Risorsa"**, fornisce un’analisi dettagliata del costo totale per risorsa. Utilizzando grafici a colonna, questo foglio mostra i costi mensili per risorsa, permettendo di monitorare e gestire efficacemente i costi associati a ciascuna risorsa. Questa visualizzazione è cruciale per la gestione ottimale delle risorse aziendali e per garantire un utilizzo efficiente delle stesse.

### 4. Dax e i Misuri

```
TotFattAnnual = CALCULATE (
    SUM ( 'Dati2'[tot fatt gglav] ),
    FILTER (
        'Dati2',

        'Dati2'[Cliente] IN VALUES ( 'Dati2'[cliente] ) &&
        'Dati2'[titolare commessa] IN VALUES('Dati2'[titolare commessa])
    )
)
```

```
TotFattMensil = CALCULATE (
    SUM ( 'Dati2'[tot fatt gglav] ),
    FILTER (
        'Dati2',
        'Dati2'[Mese] = SELECTEDVALUE('Dati2'[Mese]) &&
        'Dati2'[Cliente] IN VALUES ( 'Dati2'[cliente] ) &&
        'Dati2'[titolare commessa] IN VALUES('Dati2'[titolare commessa])
    )
)
```

```
Month Name = 
SWITCH(Dati2[Mese],
1, "1 January",
2, "2 Febbruary",
3, "3 March",
4, "4 April",
5, "5 May",
6, "6 June",
7, "7 July",
8, "8 August",
9, "9 Settember",
10, "10 October",
11, "11 November",
12, "12 December")
```

*Nella foto sopra, possiamo vedere la scheda "Risorsa" con l’analisi dettagliata del costo totale per risorsa e i grafici a colonna che mostrano i costi mensili.*

## Dashboard 2:

![Fatturato.png](https://github.com/MonaJB/F2Informatica/blob/Assets/Fatturato.png)


## Dashboard 2:

![Risorsa.png](https://github.com/MonaJB/F2Informatica/blob/Assets/Risorsa.png)



## Conclusione

L'utilizzo di Power BI per creare questa dashboard interattiva ha permesso di ottenere una visione completa e dettagliata delle performance finanziarie dell'azienda. Analizzando il fatturato, i margini e i costi per cliente e risorsa, è possibile prendere decisioni informate e migliorare le strategie aziendali. Questa dashboard rappresenta uno strumento fondamentale per il controllo della redditività e l'ottimizzazione delle risorse aziendali.


