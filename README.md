## fork intesion

The ideia behind this fork is to train a AI to generate questions using Portuguese. For that i uses [SQuAD-pt](https://huggingface.co/datasets/squad_v1_pt) a translated version of the famouse dataset [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/) to Portuguese.

It was necessery to change the laguage used in [Conceptnet Numberbatch](https://github.com/commonsense/conceptnet-numberbatch) from english to multilingual and [spaCy 2.0](https://spacy.io/) to pt_core_news_sm to handle it

## fork first results

<i>Original Text:</i>
<br>'O Módulo de Comando (CM) era a cabine cônica da tripulação, projetada para transportar três astronautas desde o lançamento até a órbita lunar e de volta ao pouso oceânico da Terra. Foi o único componente da espaçonave Apollo a sobreviver sem grandes mudanças na configuração, à medida que o programa evoluía desde os primeiros estudos do Apollo. Seu exterior era coberto com uma blindagem térmica ablativa e possuía seu próprio motor de sistema de controle de reação (RCS) para controlar sua atitude e orientar seu caminho de entrada atmosférico. Pára-quedas foram carregados para retardar sua descida até a queda. O módulo tinha 11,42 pés (3,48 m) de altura, 12,83 pés (3,91 m) de diâmetro e pesava aproximadamente 12.250 libras (5.560 kg)'

<i>Generated Questions:</i>
<br> -- : o que entrar produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem
<br> -- : o que entrar produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem para criar
<br> -- : o que entrar produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem produzem para criar 95


## seq2seq

Universal sequence-to-sequence model with attention and beam search for inference decoding. Should work for text summarization, 
neural machine translation, question generation etc. although might require different hyperparameters or data preprocessing.

In my case I've used it for question generation - I've trained the model on reversed [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/)
dataset with paragraphs as my input and questions as my targets. The SQuAD parsing script is also included in the depository. 

The data and checkpoints are not included as they simply weight too much, but feel free to experiment around with the model and let me know
your results! :) 

## Requirements

- Python 3.5
- Tensorflow 1.4
- [Conceptnet Numberbatch](https://github.com/commonsense/conceptnet-numberbatch)
- [spaCy 2.0](https://spacy.io/)
- and of course all the standards like numpy, pandas, pickle etc.

## Workflow

Model uses LSTM cells, Bahdanau attention, Conceptnet Numberbatch for word vector and spaCy for data preprocessing. I believe every function
is nicely commented, so I won't go too much into details here as the code speaks for itself.

For the data preprocessing I'm using spaCy as I've came to rely heavily on it when it comes to any NLP tasks - it's simply brilliant.
In this case, spaCy is used to remove stopwords and punctuation from the dataset as well as exchange any entities in the text into their 
corresponding labels, such as LOC, PERSON, DATE etc, so the model learns dependencies between paragraph and question and does not overfit.

## Sample

The model doesn't produce any staggering results for the question generation task with the dataset I've used, however, I can imaging with more examples the output would make more sense. Possibly training the model without changing the entities with their labels, or more experimenting with data preprocessing (lemmatization, keeping only important nouns/verbs etc) could also yield better retuls.

<i>Original Text:</i>
<br>'Archbishop Albrecht of Mainz and Magdeburg did not reply to Luther\'s letter containing the 95 Theses. He had the theses checked for heresy and in December 1517 forwarded them to Rome. He needed the revenue from the indulgences to pay off a papal dispensation for his tenure of more than one bishopric. As Luther later noted, "the pope had a finger in the pie as well, because one half was to go to the building of St Peter's Church in Rome".'

<i>Converted Text:</i>
<br>ORG PERSON reply ORG 's letter containing 95 theses theses checked heresy DATE forwarded GPE needed revenue indulgences pay papal dispensation tenure bishopric ORG later noted pope finger pie CARDINAL building ORG GPE

<i>Generated Questions:</i>
<br> -- : what was the issue of ORG <EOS>
<br> -- : what was the issue of ORG in GPE <EOS>
<br> -- : what was ORG 's stance of the printed taken in GPE <EOS>
