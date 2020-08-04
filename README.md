# scattertext-problem-replication
Replication of my problems with Scattertext 




1. I want to compare two comapnies' patent activities by patent class. All patent classes (IPC column in scatter_IPC.csv) have four characters consisting letters and numbers, eg. F03D, F16F. My code is:
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

![GitHub Logo](/scattertext.PNG)


I don't know why some IPC classes aren't shown as it should, eg. k, g m. 


Any Idea? 

2. If I build corpus like this:
```
#######################################################
corpus = st.CorpusFromParsedDocuments(
    df, category_col='Company', parsed_col='IPC'
    ).build().get_unigram_corpus().compact(st.AssociationCompactor(2000))
#######################################################
```

I receive an error: 

```
#######################################################
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-33-d2e073311427> in <module>()
      1 corpus = st.CorpusFromParsedDocuments(
----> 2     df, category_col='Company', parsed_col='IPC'
      3     ).build().get_unigram_corpus().compact(st.AssociationCompactor(2000))
      4 
      5 html = st.produce_scattertext_explorer(

5 frames
pandas/_libs/reduction.pyx in pandas._libs.reduction.compute_reduction()

pandas/_libs/reduction.pyx in pandas._libs.reduction.Reducer.get_result()

/usr/local/lib/python3.6/dist-packages/scattertext/features/FeatsFromSpacyDoc.py in get_feats(self, doc)
     47 		'''
     48                 ngram_counter = Counter()
---> 49                 for sent in doc.sents:
     50                         unigrams = self._get_unigram_feats(sent)
     51                         bigrams = self._get_bigram_feats(unigrams)

AttributeError: 'str' object has no attribute 'sents'
#######################################################
```

Why am I doing wrong? 

