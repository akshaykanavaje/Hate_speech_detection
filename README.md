# Hate_speech_detection

We shall assume that the reader is using a .ipynb reader to read the code.

## Dependencies:

* **pandas**
* **numpy**
* **tensorflow** - for the models
* **tensorflow_hub** - for pre trained BERT embedder
* **nltk**
* **ekphrasis** - for data preprocessing 

### Files:

* **Emoticons.p** - this file faciliates the removal of emojis.

## 1 - Running the code to get Statistics for Descriptive Analysis

The report discusses what the most common words, mentions and hashtags are in the datapoints.

First run the code block with the function named **concatenate_tweets**. What this does is combine both the train set and the dev set into one dataframe. We take the doctrine that the trainning set and the development/evaluation set is data that is already known and that the test set is data that is unknown.

1. To get the statistics for the distribution of labels, run the code with function **find_distribution**.
2. To see that there are no missing values, run the code with the comment #code to check for any missing values
3. To see relevant statistics regarding the most common words, run the code blocks in the section *Common Words*
4. To see relevant statistics regarding the most common mentions, run the code blocks in the section *Common Mentions*
5. To see relevant statistics regarding the most common hashtags, run the code blocks in the section *Common Hashtags*
6. For the annotation and occurences for the statistics found on page 4, run the code blocks in the section *Find Annotation of Strings*
6. For the Graphs that we have plotted, run the code blocks in the section called *Plotting / Graphing*

## 2 - Running the code for trainning the models and evalutation on the test set.

### Running the Data Preprocessing Pipeline

To run the code that was used to preprocess the trainning, development and test set (in order to make them into a suitable format for predictions) first run the following code:

1. Run **remove_URLs_Mentions_Hash**
2. Run **remove_numbers**
3. Run **remove_punctuation**
4. Run **lower_case**
5. Run **fix_elongation**
6. Run **remove_stopwords**
7. Run **get_wordnet_pos**
7. Run **lemmatise_words**
8. Run **import ekphrasis**
9. Run the code block used to initialise both the word corrector and the word segmenter.
10. Run **segment_words**
11. Run **word_correction**
12. Run **replace_abbreviations_slangs**
13. Run **remove_emoticons**

After this the full data preprocessing pipeline can be bought into scope by running **preprocessing_pipeline**. The code that applies the preprocessing pipeline can be found in their respected sections (Trainning Set Transformations, Preparing Dev Set and  Preparing Test Set). The usual call is **df2['text'] = df2['text'].apply(preprocessing_pipeline).**

I do not reccomend actually running the datapipeline, as it can take upwards of 1 hour. This is why I included the files train_preprocessed.tsv, dev_preprocessed.tsv and test_preprocessed.tsv which are the respective dataframes but after the preprocessing pipeline. You can load them using a simple **pandas.read_csv command**. 

### Trainning the Models

#### Universal Sentence Embedding and SVM

First import the required modules in **Useful Import for Models**.

Load the preprocessed dataset (whether from the train_preprocessed.tsv or running the preprocessing pipeline on the original trainning set) and name the dataset df2. Run the codeblocks in section ***Splitting Trainning set between Points and Label***

1. Run the code blocks in section Sentence Embeddings
2. Run the block with **train_SVM_X = embeddings**.
4. Run the blocks in the section  **Trainning best SVM**.
5. Evaluate the model on the trainning set with the following two code block.

As a result you should have bought the variable **best_svc_clf** into scope. This is the optimised SVM classifier.

#### RNN LSTM Model

By following the steps below you will bring into scope the regularised LSTM model. 

1. Run the code blocks in section **2.1) Encoding Sentences**.
2. Run the code blocks in section **2.4) Regularising The Model**.
4. Evaluate the model on the trainning set by running the code blocks in secion  **2.5) Evaluate Model Performance on train set**.

At the end you should have a trained RNN LSTM model whose F1 score on the trainning set is similiar to its F1 score on the dev set (Section 2.6).

#### BERT CNN

1. Run the blocks in **3.1) Install and import necessary librarie.**
2. Run the blocks in **3.2) Load the pre-trained BERT model for sentence embedding.**
3. Run the blocks in **3.3) Encode sentences from the dataset using BERT sentence embeddings.**.
4. Run the blocks in **3.4) Split the data into training and validation sets.**
5. Run the blocks in **3.8) Optimizing**

By the end of these instructions the variable **cnn_model** will be an optimised cnn classifier trained on BERT embeddings.

### Evaluating the models on the Development Set

1. You first need to follow the instructions laid out for the preprocessing pipeline
2. Run **0.1) Loading Dev Set**
3. Either apply the preprocessing pipeline in **0.2) Preprocessing Dev Set** or load the processed dev set from the file **dev_preprocessed.tsv** such as in **0.3) Loading Preprocessed dev set**
4. Run the code blocks in **0.4) Dev Set Transformations** to get **dev_X** and **Dev_Y**.

#### Universal Sentence Embedding and SVM

1. Run the code blocks in **1) Evaluating Universal Embedding and SVM**.

#### RNN LSTM

1. Run the code blocks in **2) Evaluating RNN LSTM**.

#### BERT CNN

1. Run the code blocks in **3.1) Preparing Dev Set**
2. Run the code blocks in **3.2) Predict and Evaluate**

### Evaluating the models on the Test Set

All of the code is contained in the section **Final Classification**

1. You first need to follow the instructions laid out for the preprocessing pipeline
2. Run **0.1) Load Train Set**
3. Either apply the preprocessing pipeline in **0.2) Preprocessing Test Set** or load the processed test set from the file **test_preprocessed.tsv** such as in **0.3) Loading preprocessed test set**
4. Run the code blocks in **0.3) Test Set Tranformations** to get **test_X** and **test_Y**.

#### Universal Sentence Embedding and SVM

1. Run the code blocks in **1) SVM**.

#### RNN LSTM

1. Run the code blocks in **2) LSTM RNN**.

#### BERT CNN

1. Run the code blocks in **3) BERT CNN**
