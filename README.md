# Arabic-Dialect-Chatbot-AI-Task
This is a chatbot that can reply to you in five dialect (Egyptian, Magharbi, gulf, Levantine, Classical Arabic).
________________________
## [Detect dialect](https://github.com/AntoniosMalak/Arabic-Dialect-Chatbot-AI-Task/tree/main/Detect%20Dialect)
 have `dialect_dataset.csv` that include 2 columns (ids and dialect) and [API](https://recruitment.aimtechnologies.co/ai-tasks) to fetch texts from it.
There are 4 steps as mentioned below.<br>
### 1) [Data fetching notebook from API](https://github.com/AntoniosMalak/Arabic-Dialect-Chatbot-AI-Task/blob/main/Detect%20Dialect/data_fetching.ipynb)
- First condition: The request body must be a JSON as a list of strings, and the size of the list must NOT
exceed 1000
- Second condition: The API will return a dictionary where the keys are the ids, and the values are the text, here
is a request and response.<br>
Because of these conditions we have data with 458197 rows so we worked in these steps........
  - Collect ids astype string in a list.
  - Split ids into two lists.
    - list_1000 => this is a list with length 485 of lists every list on it with length 1000 here we collect (485000).
    - list_197  => this is a single list with length 197.
    at last, we make 2 lists contain all ids.
  - Make a request for 2 lists to fetch all texts.
  - Collect ids and texts into DataFrame.
  - See if we have missing data we don't fetch.
  - Save data as [`collected_data.csv`](https://drive.google.com/file/d/15ZB6a0kQeKS4Se1CAcqogsFt0TaJ5q10/view?usp=sharing)

### 2) [Data pre-processing notebook](https://github.com/AntoniosMalak/Arabic-Dialect-Chatbot-AI-Task/blob/main/Detect%20Dialect/pre-processing%20data.ipynb)
- I worked in the same way as mentioned in [`Aim Technologies blog`](https://aimtechnologies.co/arabic-sentiment-analysis-blog.html?fbclid=IwAR0hlfhCOqd2xpJ3sGUb8yJbN0MzMq4dPPe6swuXwtdbCx1Mrn2I2wei3AM) to prepossessing Arabic texts and add another prepossessing text. <br>
    - `Normalizing similar characters` for example: (أ,إ,ا) should all be (ا). <br>
    - `Removing tashkeel` for example (“وَصيَّة”) should be (“وصية”). <br>
    - `Normalizing mentions and links to a standard form` for example: (@vodafone سعر الباقة كام؟) should be (XmentionX سعر الباقة كام؟).<br>
    - `Removing unnecessary or repeated punctuation or characters` for example: (!!! جداااااا) should be (! جدا).<br>
    - `Removing English words and numerical` for example: my name is انطونيوس ملاك 55457 should be (انطونيوس ملاك). <br>
    - `Replace new line` and make it in same line.<br>
    - `Remove first and end space in text` for example: (“   اهلا بيك    ”) should be (“اهلا بيك”).<br>
    - `Remove punctuation` for example: (“انت بتعمل ايه ؟!.”) should be (“انت بتعمل ايه”).<br>

- Collect prepossessing texts in processed_text column in new data include columns (ids, text, dialect, processed_text)
- Save data as [`processed_data.csv`](https://drive.google.com/file/d/1PWEN8YwApU7PfnZv88-_UIxXH-kcVtyh/view?usp=sharing)

### 3) [Model Training notebook](https://github.com/AntoniosMalak/Arabic-Dialect-Chatbot-AI-Task/blob/main/Detect%20Dialect/model_training.ipynb)
Here we built classification models and Deep learning model.
- classification models:
  - Load data and split it and build methods that can help.
    - **text_fit_predict** method to predict more than feature_extraction techniques(CountVectorizer, TfidfVectorizer) with original data.
  - Trained a lot of classification models but it's so bad so I saw Logistic Regression with solver sag is the best one and I worked with.
  - At last, I chooesed the best model and tried to imporve it with GridSearchCV with cv = 5 but it gave me the same accuarcy so I worked with base model.
- Deep Learning model:
  - Built tokenizer with MAX_NB_WORDS = 50000 and MAX_SEQUENCE_LENGTH = 30
  - Built model with 4 layers Embedding, SpatialDropout1D, LSTM, Dense and used epochs = 20, batch_size = 64, earlystopping with patience = 5
- At last, Save models, tokenizer and tfidf to use it in develop section.


