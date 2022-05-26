## Fork - Intesion

The ideia behind this fork is to train a AI to generate questions using Portuguese. For that i uses [SQuAD-pt](https://huggingface.co/datasets/squad_v1_pt) a translated version of the famouse dataset [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/) to Portuguese.

It was necessery to change the laguage used in [Conceptnet Numberbatch](https://github.com/commonsense/conceptnet-numberbatch) from english to multilingual and [spaCy 2.0](https://spacy.io/) to pt_core_news_sm to handle it

## Fork - Results

<i>Original Text:</i>
<br>'Olhando para aém dos pousos lunares tripulados, a NASA investigou várias aplicações pós-lunares para o hardware Apollo. A Apollo Extension Series (Apollo X) propôs até 30 vôos para a órbita terrestre, usando o espaço no SLA (Spacecraft Lunar Module Adapter) para abrigar um pequeno laboratório orbital (oficina). Os astronautas continuariam a usar o CSM como uma balsa para a estação. Este estudo foi seguido pelo design de uma oficina orbital maior a ser construída em órbita a partir de um estágio superior vazio do S-IVB Saturn, e cresceu no Programa de Aplicações Apollo (AAP). O workshop deveria ser complementado pelas missões Apollo Telescope, que substituiriam o equipamento e o motor do LM por um observatório de telescópio solar. O plano mais ambicioso exigia o uso de um S-IVB vazio como uma espaçonave interplanetária para uma missão aérea de Vênus.'

<i>Converted Text:</i>
<br>'olhando para além dos pousos lunares tripulados, a ORG investigou várias aplicações pós-lunares para o MISC. a MISC (MISC) propôs até 30 vôos para a órbita terrestre, usando o espaço no ORG (MISC) para abrigar um pequeno laboratório orbital (oficina). os astronautas continuariam a usar o ORG como uma balsa para a estação. este estudo foi seguido pelo design de uma oficina orbital maior a ser construída em órbita a partir de um estágio superior vazio do MISC, e cresceu no MISC (ORG). o workshop deveria ser complementado pelas missões MISC, que substituiriam o equipamento e o motor do ORG por um observatório de telescópio solar. o plano mais ambicioso exigia o uso de um MISC vazio como uma espaçonave interplanetária para uma missão aérea de LOC.'

<i>Generated Questions:</i>
<br> -- : como foi chamado o MISC <EOS> <EOS> <EOS> <EOS>
<br> -- : como foi chamado o MISC <EOS> <EOS> <EOS>
<br> -- : qual foi o melhor uso de MISC <EOS> <EOS> <EOS> <EOS> <EOS>


## Fork - Comments

The first results are not satisfactory but it is a start point. 

Help is always welcome if you feel interested please, send a message.


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
