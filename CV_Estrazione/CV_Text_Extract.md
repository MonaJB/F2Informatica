# Estrazione di Dati dai CV con Python, PyMuPDF e Output in Formato JSON

Nel competitivo mercato del lavoro, i recruiter spesso devono gestire un elevato numero di CV, rendendo la revisione manuale e l'estrazione di informazioni complesse. Python, un linguaggio di programmazione ampiamente utilizzato, offre una soluzione potente per automatizzare questo processo, permettendo ai recruiter di identificare in modo efficiente i candidati qualificati.

## Prerequisiti

Per avviare questo progetto, è necessario avere conoscenze di programmazione Python e familiarità con librerie come PyMuPDF.

Le seguenti librerie Python devono essere installate per far funzionare il codice:
```
!pip install fitz
!pip install PyMuPDF
!pip install Unidecode
!pip install Pillow
```

## Acquisizione dei dati

Il primo passo consiste nell'acquisire una raccolta di CV in formato PDF da diverse fonti, come portali online, candidature dirette o database di reclutamento.

```
import os
import json
import pandas as pd
import numpy as np
import fitz
from unidecode import unidecode
from tabulate import tabulate
import re
import pytesseract
from PIL import Image
from datetime import datetime, timedelta
from dateutil.relativedelta import relativedelta

def mostra_df(df):
    # Imposta il numero massimo di righe e colonne visualizzate
    pd.set_option('display.max_rows', 100)
    pd.set_option('display.max_columns', None)

    # Mostra il DataFrame
    print(df)
```

## Estrazione del testo

Dopo aver acquisito i CV, il passo successivo è convertirli in formato di testo utilizzando PyMuPDF. Questa fase è necessaria poiché librerie come PyMuPDF lavorano su dati testuali.

## Riconoscimento di entità nominate

Con i CV ora in formato di testo, PyMuPDF entra in azione per analizzare il testo e identificare entità come nomi, organizzazioni, località, date e competenze. Queste informazioni sono essenziali per estrarre dettagli rilevanti dai CV.

```
def extract_pdf_to_json_folder(folder_path, output_file):
    # Create an empty dictionary to store data for all PDFs
    all_data = {}

    # Iterate over each file in the specified folder
    for file_name in os.listdir(folder_path):
        # Check if the file has a PDF extension
        if file_name.lower().endswith('.pdf'):
            # Construct the full path to the PDF file
            pdf_path = os.path.join(folder_path, file_name)

            # Extract data from the PDF and append it to the dictionary
            pdf_data = extract_pdf_to_df(pdf_path)
            all_data[file_name] = pdf_data

    # Save the data to the output file with each key-value pair on a single line
    with open(output_file, 'w') as f:
        for key, value in all_data.items():
            f.write(f'"{key}": {json.dumps(value, indent=None)}\n')

    # Return the combined data for all PDFs in JSON format
    return json.dumps(all_data, indent=2)
```

## Organizzazione dei dati

I dati estratti vengono organizzati in un formato strutturato, come un file JSON. Questo rende l'output facilmente leggibile e utilizzabile per ulteriori analisi.

```
def struttura_output(result_df):
  output = pd.DataFrame(result_df).T.reset_index().rename(columns={'index':'Nome File'})
  output['ID Client'] = output['ID Client'].astype(int)
  # Estrarre la data di inizio dalla colonna 'Tempo'
  output['Start'] = output['Data prevista inizio attivita\''].str.extract(r'(\d{2}-\d{2}-\d{4})')
  # Convertire la colonna 'start' in formato datetime
  output['Start'] = pd.to_datetime(output['Start'], format='%d-%m-%Y')
  try:
    # Estrarre la durata e l'unità dalla colonna 'Tempo'
    duration_info = output['Data prevista inizio attivita\''].str.extract(r'(\d+) (\w+)')

    # Mappare le unità temporali a giorni
    unit_to_days = {'giorno': 1, 'giorni': 1, 'mese': 30, 'mesi': 30, 'anno': 365, 'anni': 365}
    output['duration_days'] = duration_info[0].astype(int) * duration_info[1].map(unit_to_days)


    # Calcolare la data di fine aggiungendo la durata in giorni con relativedelta
    output['End date'] = output.apply(lambda row: row['Start'] + relativedelta(days=row['duration_days']), axis=1)

    # Eliminare le colonne temporanee se necessario
    output = output.drop(['duration_days'], axis=1)
  except:
    output['End date'] = ''

  return output
```

```
output.rename(columns={
    "Descrizione Sintetica Profilo": "Summary Description",
    "Profilo": "Categoria",
    "Job Description": "Job Description",
    "Ndeg Risorse richieste": "Available Positions (Total)",
    "Sede di lavoro": "Location"
}, inplace=True)

colonne_comuni = ['Cliente', 'Categoria', 'Summary Description', 'ID Client',
       'Job Description', 'Available Positions (Total)', 'Start', 'End date',
       'Location'] #foglio.columns.intersection(output.columns)


foglio_ = pd.concat([foglio, output[colonne_comuni]]).reset_index(drop=True)
```

## Sfide e considerazioni

Nonostante PyMuPDF offra uno strumento potente, alcune sfide possono influire sull'accuratezza dell'estrazione. La qualità dei CV, l'efficacia del processo di estrazione del testo e le prestazioni dell'algoritmo NER sono fattori cruciali. Inoltre, la variazione nella struttura e formattazione dei CV richiede un parser flessibile che si adatti a stili diversi.

## Conclusione

L'estrazione di dati dai CV utilizzando Python e PyMuPDF è uno strumento prezioso per i recruiter, permettendo loro di semplificare il processo di assunzione, ridurre lo sforzo manuale e identificare in modo efficiente candidati qualificati. Con la crescente domanda di metodi di reclutamento efficienti, le capacità di PyMuPDF nella scomposizione dei CV svolgeranno un ruolo sempre più importante nel futuro del reclutamento. L'output in formato JSON fornisce una struttura organizzata e facilmente analizzabile per ulteriori elaborazioni.
