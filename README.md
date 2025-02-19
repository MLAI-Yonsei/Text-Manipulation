# Large Language Models and Text Manipulation
## : Limits and Potential in Handling Altered Linguistic Expressions

Large Language Models (LLMs) have been developed across various domains, including mathematical reasoning and common sense understanding. However, their ability to process and restore altered linguistic expressions remains underexplored. To achieve human-like language understanding, LLMs must be able to process informal and altered linguistic language as well as standardized text. In today's digital landscape, altered linguistic expressions are constantly being created and proliferated without clear rules, and LLMs need to be able to identify and restore them effectively.  To assess the text manipulation capabilities of LLMs, we constructed a dataset of altered text in English and Korean to evaluate how accurately LLMs can restore it. The dataset contains various types of visual and phonetic modifications and abbreviations for each language. We applied four models and three prompt engineering to measure restoration performance. The experimental results showed that Korean tasks performed weaker than English tasks across all, and certain types of tasks showed common limitations regardless of language. This suggests that LLMs need to go beyond simple character conversion to deal with variant languages, and strengthen their visual perception and pattern reasoning skills, as well as their ability to understand linguistic context.

<img width="1456" alt="intro" src="https://github.com/user-attachments/assets/d2cc58b8-3275-45be-9d5a-1a0f8d95d7a7" />
  
  
## Setup
To run the experiment, install the required packages.

Python==3.12.3 
```bash
pip install -r requirements.txt
```

  
## Data preperation
### 1) Augmentation
Use the `Augmentation` folder inside `Dataset Building` to augment data using the provided pre-augmentation dataset and related scripts. The final dataset is created by merging manually collected and augmented data.

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
> Applies transformation rules for the Korean Visual Transform Task dataset augmentation. Manual inspection ensures no redundant transformations are applied.

### 2) Preprocessing
To finalize the dataset for the experiment, preprocess the collected and augmented raw data. The sources of the raw dataset are documented in the research paper.

```bash
Dataset_Building/Augmentation/eng_phonetic_preprocessing.ipynb
```
> Filters phonetic similarity data for the English Phonetic Substitution task using the `double metaphone` algorithm.

```bash
Dataset_Building/Augmentation/eng_abbreviation_preprocessing.ipynb
```
> Filters abbreviation-compliant data for the English Abbreviation task using the appropriate rules.

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
📌 **GPT o3-mini** is only used for error case analysis in this study.

### Available Prompts
The `prompts.json` file contains prompts for Zero-shot, CoT (Chain-of-Thought), and CoT+ICL (In-Context Learning) settings.

```bash
Task/gpt4o_batch.ipynb
Task/gemini.ipynb
Task/claude_batch.ipynb
Task/gpto3mini.ipynb
```

  
## Evaluation Preprocessing
Before evaluating LLM responses, preprocessing is performed using the scripts in the `Preprocessing` folder.

```bash
Preprocessing/answer_split.ipynb
```
> Parses the original words from the full responses of LLMs based on the predefined `Answer format` in the prompt.

```bash
Preprocessing/abbreviation_preprocessing.ipynb
```
> For English and Korean abbreviation tasks, LLMs generate five responses. This script selects the most similar response to the ground truth for evaluation.

  
## Evaluation
Assess the performance of different models and analyze the results.

```bash
Evaluating/evaluation_main_task.ipynb
```
> Evaluates model performance for the main task.

```bash
Evaluating/evaluation_main_task_eng_consonant_vowel.ipynb
```
> Uses a different evaluation standard for the English Consonant & Vowel Combination task.
  
**Failure Case Analysis** can be performed using the following scripts:

```bash
Evaluating/evaluation_failure_case.ipynb
```
> Analyzes GPT o3-mini failure cases and compares them with other models.

```bash
Evaluating/evaluation_failure_case_eng_consonant_vowel.ipynb
```
> Uses a different evaluation standard for failure cases in the English Consonant & Vowel Combination task.
