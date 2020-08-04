# scattertext-problem-replication
Replication of my problems with Scattertext 




I want to compare two comapnies' patent activities by patent class. All patent classes (IPC column in scatter_IPC.csv) have four characters consisting letters and numbers, eg. F03D, F16F. My code is:
```
#######################################################
CODE:
!pip install scattertext
import scattertext as st
import pandas as pd
import spacy
import en_core_web_sm
nlp = en_core_web_sm.load()
nlp = spacy.load('en')

df = pd.read_csv("scatter_IPC.csv")
corpus = st.CorpusFromPandas(df,
                             category_col='Company',
                             text_col='IPC',
                             nlp=nlp).build()

html = st.produce_scattertext_explorer(
    corpus,
    category='Envision', category_name='Envision', not_category_name='Goldwind',
    minimum_term_frequency=20,
    pmi_threshold_coefficient=0,
    width_in_pixels=1000,
    metadata=corpus.get_df()['Company'],
    transform=st.Scalers.dense_rank
)
open('./demo_compact.html', 'w').write(html)
#######################################################
```


The result is:

![GitHub Logo](/images/scattertext.png)
Format: ![Alt Text](url)


I don't know why some IPC classes aren't shown as it should, eg. k, g m. 


Any Idea? 

