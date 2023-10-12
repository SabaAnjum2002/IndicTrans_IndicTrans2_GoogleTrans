<strong>IndicTrans</strong>:
<br>
---

+ To Evaluate the translated articles on the predicted ouput of IndicTrans we need to load the models and unzip them.
```
!wget https://ai4b-public-nlu-nlg.objectstore.e2enetworks.net/indic2en.zip
!unzip indic2en.zip
```
+ You can get the predictions for your original articles from the IndicTrans model using the below code(Telugu to English Translations)
```
from inference.engine import Model
model = Model('indic-en')
def process_line(line):
    hi=str(line)
    processed_line = model.translate_paragraph(hi, 'te', 'en')
    return processed_line
input_file_path = 'original_texts_te.txt'
output_file_path = 'Indic_te.txt'
with open(input_file_path, 'r') as input_file, open(output_file_path, 'a') as output_file:
    # Read each line from the input file
    for line in input_file:
      processed_line = process_line(line)
      output_file.write(processed_line + '\n')
input_file.close()
output_file.close()
```
+ Below code is for Hindi to English Translations
```
def process_line(line):
    hi=str(line)
    processed_line = model.translate_paragraph(hi, 'hi', 'en')
    return processed_line
input_file_path = 'original_texts_hi.txt'
output_file_path = 'Indic_hi.txt'
with open(input_file_path, 'r') as input_file, open(output_file_path, 'a') as output_file:
    # Read each line from the input file
    for line in input_file:
      processed_line = process_line(line)
      output_file.write(processed_line + '\n')
input_file.close()
output_file.close()
```
+ To Detokenize the files we used below bash command
```
!cat {translated_texts_hi.txt,translated_texts_te.txt,Indic_te.txt,Indic_hi.txt} | sacremoses normalize > {translated_texts_hi.detok.txt,translated_texts_te.detok.txtIndic_te.detok.txt,Indic_hi.detok.txt}
```
+ Below command of secrebleu is used to fetch the BLEU and chrF++ metrics 
```
!sacrebleu translated_texts_hi.detok.txt -i Indic_hi.detok.txt -m bleu chrf ter --chrf-word-order 2
```
<strong>IndicTrans2</strong>:
<br>
---
+ To Evaluate the translated articles on the predicted ouput of IndicTrans2 we need to load the models and unzip them.
```
!wget https://indictrans2-public.objectstore.e2enetworks.net/it2_preprint_ckpts/indic-en-preprint.zip
!unzip indic-en-preprint.zip
```
+ You can get the predictions for your original articles from the IndicTrans model using the below code(Telugu to English Translations)
```
from inference.engine import Model
model = Model('indic-en-preprint/fairseq_model', model_type="fairseq")
def process_line(line):
    hi=str(line)
    processed_line = model.translate_paragraph(hi,'tel_Telu', 'eng_Latn')
    return processed_line


input_file_path = 'original_texts_te.txt'     # Replace with the path to your input file
output_file_path = 'Indic2_te.txt'   # Replace with the path to your output file

# Open the input file for reading and the output file for appending
with open(input_file_path, 'r') as input_file, open(output_file_path, 'a') as output_file:
    # Read each line from the input file
    for line in input_file:
      processed_line = process_line(line)
      output_file.write(processed_line + '\n')

# Close the files
input_file.close()
output_file.close()
```
+ Below code is for Hindi to English Translations
```
from inference.engine import Model
model = Model('indic-en-preprint/fairseq_model', model_type="fairseq")
def process_line(line):
    hi=str(line)
    processed_line = model.translate_paragraph(hi,'hin_Deva', 'eng_Latn')
    return processed_line


input_file_path = 'original_texts_hi.txt'     # Replace with the path to your input file
output_file_path = 'Indic2_hi.txt'   # Replace with the path to your output file

# Open the input file for reading and the output file for appending
with open(input_file_path, 'r') as input_file, open(output_file_path, 'a') as output_file:
    # Read each line from the input file
    for line in input_file:
      processed_line = process_line(line)
      output_file.write(processed_line + '\n')

# Close the files
input_file.close()
output_file.close()
```
+ Detokenization and Fetching the metrics is same as IndicTrans

<strong>Googletrans</strong>:
<br>
---
```
!pip install googletrans
rom googletrans import Translator

def translate_file(input_file, output_file, dest_language='en'):
    # Initialize the translator
    translator = Translator()

    # Read the input file
    with open(input_file, 'r', encoding='utf-8') as file:
        input_text = file.read()

    # Translate the text
    translation = translator.translate(input_text, dest=dest_language)

    # Write the translated text to the output file
    with open(output_file, 'w', encoding='utf-8') as file:
        file.write(translation.text + '\n')

# Example usage
input_file_path = 'original_texts_hi.txt'   # Replace with the path to your Hindi input file
output_file_path = 'googletrans_hi.txt'  # Replace with the desired path for the output file

translate_file(input_file_path, output_file_path, dest_language='en')
from googletrans import Translator

def translate_file(input_file, output_file, dest_language='en'):
    # Initialize the translator
    translator = Translator()

    # Read the input file
    with open(input_file, 'r', encoding='utf-8') as file:
        input_text = file.read()

    # Translate the text
    translation = translator.translate(input_text, dest=dest_language)

    # Write the translated text to the output file
    with open(output_file, 'w', encoding='utf-8') as file:
        file.write(translation.text + '\n')

# Example usage
input_file_path = 'original_texts_te.txt'   # Replace with the path to your Hindi input file
output_file_path = 'googletrans_te.txt'  # Replace with the desired path for the output file

translate_file(input_file_path, output_file_path, dest_language='en')
```
+ Detokenization and Fetching the metrics is same as IndicTrans

<strong>Results</strong>
<br>
---
| Model          | BLEU       | chrF2++          | 
| ---------------- | ------------- | ------------- | 
|IndicTrans(Hindi To English)       |{score: 23.6, verbose : 70.7/43.6/29.6/21.2(BP = 0.633 ,ratio = 0.686 ,hyp_len = 11356, ref_len = 16552)}| {score:46.7,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}| 
|IndicTrans2(Hindi To English)      |{score: 41.8, verbose : 75.8/55.0/42.7/34.0(BP = 0.843 ,ratio = 0.854, hyp_len = 14136, ref_len = 16552)}| {score: 62.0,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}        | 
|Google Trans(Hindi To English)    |{score: 58.9, verbose : 75.7/62.7/53.9/46.8(BP = 1.000 ,ratio = 1.138 ,hyp_len = 18839 ,ref_len = 16552)}| {score: 80.0,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}| 
|IndicTrans(Telugu To English)      |{score: 27.7, verbose : 65.0/38.2/24.9/16.9(BP = 0.868 ,ratio = 0.876 ,hyp_len = 15708, ref_len = 17936)} |{score: 52.7,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}|        
|IndicTrans2(Telugu To English) |{score: 41.9, verbose : 73.0/50.2/37.3/28.5(BP = 0.994 ,ratio = 0.946, hyp_len = 16967 ,ref_len = 17936)}|{score: 64.3,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}|        
|Google Trans(Telugu To English)         |{score: 92.6, verbose : 94.0/92.9/92.1/91.3(BP = 1.000 ,ratio = 1.048, hyp_len = 18802, ref_len = 17936)}| {score: 97.4,nrefs: 1,case: mixed,eff: yes,nc: 6,nw: 2}|  

<br>

<strong> Metric Values</strong>
<br>
---
<strong>'BLEU Score'</strong>
**Definition:** Metric that measures the similarity between machine-generated translations and reference translations by counting overlapping n-grams (word sequences) and computing a precision-based score.

<strong>'chrF++ Score'</strong>
**Definition:** Adding word unigrams (individual words) and bigrams (word pairs) to chrF leads to improved correlations with human assessments. This combination, referred to as "chrF++."

<strong>'Brevity Penalty'</strong>
**Definition:** "BP" stands for "Brevity Penalty." The brevity penalty is a component of the BLEU score that penalizes machine translations that are significantly shorter than the reference translations.

<strong>'Number of Characters (nc)'</strong>
**Definition:** "nc" likely stands for "Number of Characters." This metric will evaluate translation quality at a character-level granularity.

<strong>'Number of Words (nw)'</strong>
**Definition:** "nw" likely stands for "Number of Words." This metric will take into account word-level information when evaluating translation quality.

<strong>'Score'</strong>
**Definition:** The "score" represents the actual evaluation score obtained for a specific translation or set of translations.

<strong>'Hypothesis Length (hyp_len)'</strong>
**Definition:** This indicates the length of the machine-generated hypothesis (translation) being evaluated.

<strong>'Reference Length (ref_len)'</strong> 
**Definition:** This number represents the length of the reference translation against which the machine-generated hypothesis is being evaluated.
<br>

<strong>Installation of requirements/dependencies</strong>:
<br>
---
1.Installing the requirements by running the pip install -r requirements.txt


