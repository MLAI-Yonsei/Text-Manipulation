# Large Language Models and Text Manipulation
## : Limits and Potential in Handling Altered Linguistic Expressions

Large language models (LLMs) are advancing in many areas, but the extent to which they understand transformed language expressions has not been fully explored. In digital environments, variant language expressions are constantly being created and proliferated without clear rules. Therefore, LLMs need to be able to identify and process informal expressions effectively. In this study, we constructed a dataset of English and Korean variant texts that are difficult for LLMs to process. The dataset consists of visual and pronunciation variant expressions, contextual language such as abbreviations, and segmented consonant and vowel combinations. We evaluated the performance of restoring variant language through prompt engineering and found that Korean performance was relatively lower than English for all models, with difficulties in certain types.

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
