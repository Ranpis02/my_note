# 1 Transformer Basic

## 1.1 Stages of NLP Development

- Stage 1: Statistical Models + Data(Feature Engineering)
  - Decision Trees, SVM, HMM, CRF, TF-IDF, BOW
- Stage 2:  Neural Networks + Data
  - Linear, CNN, RNN, GRU, LSTM, Transformer, Word2Vec, GloVe
- Stage 3: Neural Networks + Pre-trained Models + (Small-scale)Data
  - GPT, BERT, RoBERTa, ALBERT, BART, T5
- Stage 4: Neural Networks + Large Pre-trained Models + Prompt
  - ChatGPT, Bloom, LLaMA, MOSS



## 1.2 Transformer and related libraries

> [!tip]
>
> Ref: https://huggingface.co/docs/transformers/v4.51.3/en/index

1. `Transformers`: a library of pretrained <u>natural language processing, computer vision, audio, and multimodal models</u> for inference and training
2. `Tokenizer`: a library prepares the inputs of a model
3. `Datasets`: a hub provides a large collection of datasets that span a variety of domains and slots
4. `Evaluate`: includes a lot of metrics help for evaluating the model's performance
5. `PEFT`: Parameter-Efficient Fine-Tuning, a library for efficiently adapting pre-trained language models to various downstream applications without fine-tuning all the model's parameters
6. `Accelerate`: a library that enables the same PyTorch code to be run across any distributed configuration by adding just four lines of code
7. `Optimum`: a optimization library that supports quantization for Intel, Furiousa, ONNX Runtime, GPTQ, and lower-level PyTorch quantization functions
8. `Gradio`: a tool provides an easy and intuitive interface for running a model from a list of inputs and displaying the outputs in various formats

 

### 1.3 Configure the Environment

1. create a `conda` environment

   ```shell
   conda create -n hugging_face_practice python=3.9 -y
   ```

2. install the `PyTorch`

3. install the `Transformers` and related libs

   ```shell
   pip install transformers gradio
   ```

4. install the other libs

   ```shell
   pip install jupyterlab scikit-learn pandas matplotlib tensorboard
   ```




## 1.4 Small Trial

1. Start a Text-Classification task server

   ```python
   import gradio as gr
   from transformers import *
   # load pipeline through Interface and start text classification task
   gr.Interface.from_pipeline(pipeline('text-classification', model='distilbert/distilbert-base-uncased-finetuned-sst-2-english')).launch()
   ```

2. Start a Reading-Comprehension task server

   ```python
   import gradio as gr
   from transformers import *
   # load pipeline through Inferface and start a reading comprehension server
   gr.Interface.from_pipeline(pipeline('question-answering', model='uer/roberta-base-chinese-extractive-qa')).launch()
   ```



# 2 Component Library

## 2.1 Pipeline

The [Pipeline](https://huggingface.co/docs/transformers/main_classes/pipelines) refers to a process where several steps are followed in a precise order to obtain a predication from a model. These steps can include <u>data preparation, feature extraction and normalization</u>.



**Task types supported by Pipeline**

| 名称                                     | 任务类型   |
| ---------------------------------------- | ---------- |
| text-classification (sentiment-analysis) | text       |
| token-classification (ner)               | text       |
| question-answering                       | text       |
| fill-mask                                | text       |
| summarization                            | text       |
| translation                              | text       |
| text2text-generation                     | text       |
| text-generation                          | text       |
| conversational                           | text       |
| table-question-answering                 | text       |
| zero-shot-classification                 | text       |
| automatic-speech-recognition             | multimodal |
| feature-extraction                       | multimodal |
| audio-classification                     | audio      |
| visual-question-answering                | multimodal |
| document-question-answering              | multimodal |
| zero-shot-image-classification           | multimodal |
| zero-shot-audio-classification           | multimodal |
| image-classification                     | image      |
| zero-shot-object-detection               | multimodal |
| video-classification                     | video      |

and the information can be given by the following code:

```python
from transformers.pipelines import SUPPORTED_TASKS
for k, v in SUPPORTED_TASKS.items():
    print(f"{k}: {v['type']}\n")
```



Create a pipeline based on task type:

```python
from transformers import pipeline
pipeline_tc = pipeline('text-classification')
# the model to be used can be specified here
# such as, pipeline(task='text-classification', model='distilbert/distilbert-base-uncased-finetuned-sst-2-english')

```

, and then we can use the created pipeline to classify text:

```python
pipeline_tc("you are a good boy")
```

> [!note]
>
> If we first specify the task type, then specify the model, we can create a pipeline for specified model.
>
> ```python
> from transformers import pipeline
> 
> pipe_tc = pipeline('text-classification', model='distilbert/distilbert-base-uncased-finetuned-sst-2-english')
> pipe_tc(['very good', 'very bad'])
> ```



Another way to create a pipeline object is to preload the model, then create the pipeline

```python
## Method 2: preload the model, then create the pipeline
from transformers import AutoModelForSequenceClassification, AutoTokenizer, pipeline
model = AutoModelForSequenceClassification.from_pretrained('distilbert/distilbert-base-uncased-finetuned-sst-2-english')
tokenizer = AutoTokenizer.from_pretrained('distilbert/distilbert-base-uncased-finetuned-sst-2-english')
pipe_tc = pipeline('text-classification', model=model, tokenizer=tokenizer)
```



The information about device used for training can be queried by the code below:

```python
pipe_tc.model.device
```



If we are currently working on a question-answering task, for the Pipeline object, the question and context can be specified by parameters `question` and `context`

```python
from transformers import pipeline

# load the model for question-answering
pipeline_tc = pipeline('question-answering', model='distilbert/distilbert-base-uncased-distilled-squad')
pipeline_tc(question='what is the capitial of China?', context='Beijing is the capital of China.')
```



:truck: Example: load a pre-trained detection model, then use the detection model to detect the object

```python
import requests
from PIL import Image, ImageDraw
from transformers import pipeline

# specify the pre-trained model
checkpoint = "google/owlvit-base-patch32"

# load the model for zero-shot object detection
detector = pipeline(model=checkpoint, task="zero-shot-object-detection")

url = "https://huggingface.co/datasets/huggingface/documentation-images/resolve/cb05469baea043eda73891d55e5f2398a2c86db4/transformers/tasks/zero-sh-obj-detection_3.png"

# load images in the form of stream object transmission
im = Image.open(requests.get(url, stream=True).raw)

# convert the image to RGB gamut
im = im.convert('RGB')

# get the predictions by the created detector|
predictions = detector(
  im, 
  candidate_labels=["hat", "sunglasses", "book"]
)

# draw the bounding boxes on the image
draw = ImageDraw.Draw(im)

for prediction in predictions:
    box = prediction['box']
    label = prediction['label']
    score = prediction['score']
    x_min, y_min, x_max, y_max = box.values()
    draw.rectangle((x_min, y_min, x_max, y_max), outline='red', width=1)
    draw.text((x_min, y_min), f'{label}: {round(score, 2)}', fill='red')

im
```



## 2.2 Tokenizer

The `Tokenizer` preprocesses the text data with the steps below:

1. Word Segmentation: use tokenizer to segment words
2. Vocabulary Construction: build a vocabulary based on the result of word segmentation
3. Data Transformation: map the text sequence to the numeric sequence based constructed vocabulary
4. Data Padding and Truncation: Before inputting data to the model in batches, it's necessary to pad data that is too short and truncate data that is too long to ensure the data length is within the acceptable range of the model



How to use?

1. load and Save

   ```python
   from transformers import AutoTokenizer
   
   # load the model frm hugging-face
   tokenizer = AutoTokenizer.from_pretrained('llava-hf/llava-1.5-7b-hf')
   
   # save the tokenizer to a local directory
   tokenizer.save_pretrained('./llava_hf_tokenizer')
   
   # load the tokenizer from the local directory
   tokenizer = AutoTokenizer.from_pretrained('./llava_hf_tokenizer/')
   ```

2. Tokenization

   ```python
   sentence = "I have a dream"
   
   # tokenizer tokenizes the sentence based on SentencePiece algorithm that uses an underscore to mark the beginning of a word 
   tokens = tokenizer.tokenize(sentence)
   ```

3. View a vocabulary and its size

   ```python
   # view the content of the vocabulary
   tokenizer.vocab
   
   # view the size of vocabulary
   tokenizer.vocab_size
   ```

4. Index conversion

   ```python
   # convert the word sequence to the id sequence
   ids = tokenizer.convert_tokens_to_ids(tokens)
   
   # convert the id sequence back to the word sequence
   tokens = tokenizer.convert_ids_to_tokens(ids)
   tokens
   
   # convert the token sequence to a string
   sen_str = tokenizer.convert_tokens_to_string(tokens)
   ```

5. Padding and truncation

   ```
   # padding='max_length': specify the padding strategy as `max_length`. If the encoded token sequence is shorter than `max_length`, it will be padding with a special padding token(often [PAD], typically ID 0) to reach the specified length
   ids = tokenizer.encode(sentence, padding='max_length', max_length=15, truncation=True)
   
   # Secondary method to achieve the same effect as above
   encoded_sequence = tokenizer(sentence, padding='max_length', max_length=15, truncation=True)
   ```

   > [!note]
   >
   > To convert the ID sequence back to the original text, use the following method:
   >
   > ```python
   > origin_text = tokenizer.decode(encoded_sequence['input_ids'], skip_special_tokens=False)
   > ```
   >
   > The encoded sequence is a `dict` that contains `input_ids` and `attension_mask` keys. The content example is as follows:
   >
   > ```tcl
   > # original text: "I have a dream"
   > {'input_ids': [32001, 32001, 32001, 32001, 32001, 32001, 32001, 32001, 32001, 32001, 1, 306, 505, 263, 12561], 'attention_mask': [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1]}
   > ```
   >
   > The exact tokens and ID vary slightly, but for `bert-base-using`, special tokens and their IDS are as follows:
   >
   > - `[PAD]`: Padding token, with ID `0`
   > - `[CLS]`: Classification token, with ID `101`. It is added at the beginning of a sequence, typically for classification task. 
   > - `[SEP]`: Separator token, with ID `102`. It marks the end a sequence or separates two sequence in tasks
   > - `[UNK]`: Unknown token, with ID `100`. It represents words not found in the tokenizer' vocabulary.
   >
   > > [!tip]
   > >
   > > Some tokenizers(e.g., RoBERTa) use `<s>`(ID 0)  instead of `[CLS]` and `</s>`(ID 2) instead of `[SEP]`, and its `[PAD]` is ID 1.





### Difference

```python
# method 1: encode(or tokenzier) the sentence, return the inputs_id directly
tokenizer.encode_plus(sentence, padding='max_length', max_length=15)

# method 2: encode(or tokenzier) the sentence, return the dict contains `inputs_id` and `attention_mask` keys
tokenizer.encode(sentence, padding='max_length', max_length=15)

# method 3: encode(or tokenzier) the sentence, return the inputs_id directly, like the met
tokenizer(sentence, padding='max_length', max_length=15)
```

