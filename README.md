# Evaluating LLMs Beyond Standard Text
## : A Benchmark on Non-Traditional Text Variations

Non-standard and transformed language is pervasive in digital communication, yet it remains unclear whether large language models (LLMs) can recover the original forms of such expressions. This paper introduces a multilingual benchmark and evaluation framework designed to assess LLMs' ability to restore the original forms and meanings from heavily modified or playful text in both English and Korean. Our dataset includes over 17,000 instances spanning visual and phonetic substitutions, abbreviations, and character-level manipulations. We conduct a comprehensive evaluation of multiple widely used LLMs under three prompting paradigms (Zero-Shot, CoT, CoT+ICL) and explore advanced verification approaches. Results reveal persistent challenges even with sophisticated methods, particularly in Korean tasks involving perceptual and cultural variations. This work establishes a benchmark that remains challenging for current LLMs.

<img width="1101" height="430" alt="intro" src="https://github.com/user-attachments/assets/4b7c8de5-6417-4e58-9577-6355c7755185" />
  
## Setup
To run the experiment, install the required packages.

Python==3.12.3 
```bash
pip install -r requirements.txt
```  

  
## Data Preparation
> **Content Warning**
> This dataset contains non-standard expressions (e.g., leetspeak, slang, and abbreviations) collected from public online sources. Although we removed expressions that could pose serious problems, some profane or offensive language may remain, as such expressions are part of the linguistic phenomena studied in this work.

### 1) Augmentation
Use the `Augmentation` folder inside `Dataset Building` to augment data using the provided pre-augmentation dataset and related scripts. The final dataset is created by merging manually collected and augmented data.  
The raw dataset may change depending on the words used, so it is not provided. The sources of the raw dataset are documented in the research paper.

**Augmentation Tasks:**
- Kor Letter Rotation
- Kor Visual Transform

```bash
Dataset_Building/Augmentation/kor_letter_rotation_rule.ipynb
```
> Applies transformation rules for the Korean Letter Rotation Task dataset augmentation.

```bash 
Dataset_Building/Augmentation/kor_visual_transform_rule.ipynb
```
> Applies transformation rules for the Korean Visual Transform Task dataset augmentation. Manual inspection ensures that the same transformation rule is not applied redundantly.

Conclusively, the collected and augmented data are stored separately within the Dataset Building/Augmentation/Data directory, and their combination has been utilized as the final merged dataset.

### 2) Preprocessing
To finalize the dataset for the experiment, preprocess the collected and augmented raw data. The sources of the raw dataset are documented in the research paper.

```bash
Dataset_Building/eng_phonetic_preprocessing.ipynb
```
> Filters phonetic similarity data for the English Phonetic Substitution task using the `double metaphone` algorithm.

```bash
Dataset_Building/eng_split_letters.ipynb
```
> Splits collected English words into consonants and vowels for the English Consonant & Vowel Combination task.

```bash
Dataset_Building/kor_split_letters.ipynb
```
> Splits collected Korean words into consonants and vowels for the Korean Consonant & Vowel Combination task.  

  
## Run
The constructed dataset is stored in the `Dataset` folder. Use this dataset along with the models in the `Task` folder to evaluate LLMs' word restoration capabilities.

### Available Prompts
The `prompts.json` file contains all prompts for Zero-shot, CoT (Chain-of-Thought), and CoT+ICL (In-Context Learning) settings.
```bash
# Examples of prompts
  "eng_visual_transform": {
    "zrs_prompt": "The given words are leetspeak, which is transformed into a letter similar to the original alphabet.\n\nAnswer format:\nAnswer: [Original word]",
    "cot_prompt": "The given words are leetspeak, which is transformed into a letter similar to the original alphabet. \nFollow these steps to decode the term and find the correct answer:\n1. Analyze each syllable.\n2. Guess visually similar alphabets.\n3. Combine the original words, including the syllables you guessed.\n\nAnswer format:\nProcessing: [Brief decoding steps]\nAnswer: [Original word]",
    "icl_prompt": "The given words are leetspeak, which is transformed into a letter similar to the original alphabet. \nFollow these steps to decode the term and find the correct answer:\n1. Analyze each syllable.\n2. Guess visually similar alphabets.\n3. Combine the original words, including the syllables you guessed.\n\nExample : \"H3ll0, w0rld!\" \n- \"3\" is converted to \"e\". Reason: \"3\" is visually similar to \"E\". \n- \"0\" is converted to \"o.\" Reason: \"0\" is visually similar to \"o.\" \nOutput: \"Hello, world!\"\n\nAnswer format:\nProcessing: [Brief decoding steps]\nAnswer: [Original word]"
```  

### Available Models
```bash
Task/gpt4o_batch.ipynb
Task/gemini.ipynb
Task/claude_batch.ipynb
```

  
## Evaluation Preprocessing
Before evaluating LLM responses, preprocessing is performed using the scripts in the `Preprocessing` folder.

```bash
Preprocessing/abbreviation_preprocessing.ipynb
```
> For English and Korean abbreviation tasks, LLMs generate five responses. This script selects the most similar response to the ground truth for evaluation.

  
## Evaluation
Assess the performance of different models and analyze the results.

```bash
Evaluating/evaluation_main_task.ipynb
```
> Evaluates one task under Zero-Shot, CoT, and CoT+ICL predictions with four metrics: Accuracy, Edit Distance, Sentence-BERT, and chrF++.

- **Normalization**: All text is NFC-normalized and lowercased, and characters other than Latin letters, digits, and Hangul are removed. Accuracy and Edit Distance additionally remove spaces; Sentence-BERT and chrF++ keep word boundaries.
- **Multiple references**: References separated by `|` are treated as alternative answers, and each prediction is scored against the best-matching one.
- **Sentence-BERT**: `all-MiniLM-L6-v2` for English tasks and `jhgan/ko-sroberta-multitask` for Korean tasks.
- **chrF++**: Mean sentence-level score with `char_order=6`, `word_order=2`, and `beta=2` (sacrebleu 2.5.1).
- **English Consonant & Vowel Combination**: For Accuracy, a prediction is also counted as correct if it is an anagram of the reference and a valid English word in the NLTK `words` corpus.
