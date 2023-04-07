# Synthetic Myanmar-Thai Parallel Corpus 
IBM-1 model with Google Translate based Myanmar-Thai sythetic parallel corpus

- [Introduction](#Introduction)
- [Corpus Development](#Corpus-Development)
- [IBM-1 for Word Translation](#IBM-1-for-Word-Translation)

## Introduction

Myanmar-Thai parallel corpus with 18,373 sentence pairs has been generated using [Google Translate](https://translate.google.com/) Machine Translation system manually. The original corpus is "Myanmar-Rakhine" part of "[myPar](https://github.com/ye-kyaw-thu/myPar/tree/master/my-rk): Myanmar Parallel Corpora for Machine Translation R&D". 

## Corpus Development

## Corpus Preparation

I downloaded Myanmar-Rakhine parallel data from [myPar](https://github.com/ye-kyaw-thu/myPar/tree/master/my-rk): Myanmar Parallel Corpora for Machine Translation R&D". Therefore, please make sure the following citations have been there in case you use Myanmar side data.

- Thazin Myint Oo, Ye Kyaw Thu, Khin Mar Soe, "Statistical Machine Translation between Myanmar (Burmese) and Rakhine (Arakanese)", In Proceedings of ICCA2018, February 22-23, 2018, Yangon, Myanmar, pp. 304-311. 

- Thazin Myint Oo, Ye Kyaw Thu, Khin Mar Soe, "Neural Machine Translation between Myanmar (Burmese) and Rakhine (Arakanese)", In Proceedings of the Sixth Workshop on NLP for Similar Languages, Varieties and Dialects, June 7th 2019, Minneapolis, United States, pp. 80-88.

## Synthetic translation generation
Step 1: Go to ***Insert > Functions > Google > GOOGLETRANSLATE( )*** to use google translate

![image](https://user-images.githubusercontent.com/53138240/230623021-8a0cf1fb-e8f4-4927-be79-3cc3d9729944.png)

Step 2: Fill source and target languages (in our case, **=GOOGLETRANSLATE(A1,"my","th")** where "my" is Myanmar language and "th" is Thai language. <br/>
Language Codes for Google Translate [[Link](https://gist.github.com/JT5D/a2fdfefa80124a06f5a9)]

![image](https://user-images.githubusercontent.com/53138240/230623148-7fe4a4a1-3fd5-4465-8935-f34fd053fbb8.png)

Step 3: Select the cell and drop to the others for duplicating the function to another rows.

![image](https://user-images.githubusercontent.com/53138240/230623544-3836abb1-088c-4495-a737-293b72cfc16b.png)

For more details: https://spreadsheetpoint.com/google-translate-function/

### Text Tokenization

- For Myanmar text, it has been already tokenized by the original authors.
- For Thai text, text tokenization had done using [PyThaiNLP](https://pythainlp.github.io/).

### Corpus Information

```
$ wc *.my
   2485   17104  286978 dev.my
   1812   12478  208710 test.my
  14076   95738 1588605 train.my
  18373  125320 2084293 total
$ wc *.th
   2485   16795  282388 dev.th
   1812   12275  205385 test.th
  14076   93957 1567486 train.th
  18373  123027 2055259 total
```

## IBM-1 for Word Translation

```
Number of Unique Words:
> Myanmar : 16586
> Thai : 6824

maximum iteration: 25
maximum words: 1000
```

Let's see some of the top confident translation pairs (my-th) ...

```
(('ရေဒီယို', 'วิทยุ'), 0.6422682076536538) 
(('အတွက်', 'สำหรับ'), 0.620955276597172)
(('အတန်း', 'ชั้นเรียน'), 0.6086475033551821)
(('မနက်ဖြန်', 'พรุ่งนี้'), 0.5935846447270265)
(('တီဗွီ', 'ทีวี'), 0.5910367450179066)
```

### Evaluation: BLEU

BLEU score measures the n-gram precision with respect to the comparison of hypothesis and reference files. The higher BLEU score means the better the translation model is. We used [nltk](https://www.nltk.org/) library for BLEU score calculation. The following table shows the average BLEU scores using various smoothing functions considering test samples with at-least one word overlap.
<br/> <br/>
For more details of various smoothing functions: https://github.com/nltk/nltk/blob/develop/nltk/translate/bleu_score.py 

| **Smoothing**  | **BLEU** |
|:--------------:|-:|
| Method 1 |0.053|     
| Method 2 |0.222|     
| Method 3 |0.106|     
| Method 4 |0.043|     
| Method 5 |0.081|      
| Method 7 |0.109|      

### Results

The followings are the sample translation of the model on the test data.

```
Sentence Index:  632
Myanmar Sentence: ['သူတို့ရဲ့', 'နမော်နမဲ့နိုင်မှု', 'ကြောင့်', 'သူတို့', 'စာမေးပွဲ', 'ကျ', 'ခဲ့ကြတယ်', '။']
Reference Thai Sentence: ['เนื่องจาก', 'ความ', 'ฟุ้งซ่าน', 'ของ', 'พวกเขา', 'พวกเขา', 'ทำ', 'ให้การ', 'สอบ', 'ของ', 'พวกเขา']
Translated Sentence: ('สอบ', 'ผัก', 'สถานการณ์', 'သူတို့', 'น้ำตา', 'แรก')
Translation BLEU Scores [0.01774239756616722, 0.08389861810900508, 0.03527502360630137, 0.016338026308907974, 0.037172650766057885, 0.04838439061032186]

Sentence Index:  633
Myanmar Sentence: ['ပါးစပ်', 'ပလုတ်ပလောင်း', 'နဲ့', 'စကား', 'မ', 'ပြော', 'နဲ့', '။']
Reference Thai Sentence: ['อย่า', 'พูด', 'ด้วย', 'ปาก']
Translated Sentence: ('ไหม', 'နဲ့', 'คำพูด', 'ပြော', 'မ', 'နဲ့')
Translation BLEU Scores [0, 0, 0, 0, 0, 0]

Sentence Index:  634
Myanmar Sentence: ['သူတို့', 'မင်း', 'ကို', 'မေး', 'ချင်', 'ကြမှာ', '။']
Reference Thai Sentence: ['พวกเขา', 'จะ', 'ต้องการ', 'ถาม', 'คุณ']
Translated Sentence: ('พื้น', 'သူတို့', 'ถาม', 'မင်း', 'อยาก', 'ကို')
Translation BLEU Scores [0.040824829046386304, 0.19304869754804482, 0.08116697886877472, 0.03759340464156993, 0.08553337321327789, 0.11133131628989178]

Sentence Index:  635
Myanmar Sentence: ['ငါ', 'တော်တော်', 'ပင်ပန်း', 'နေတယ်', '။']
Reference Thai Sentence: ['ฉัน', 'เหนื่อย', 'มา', 'ก.']
Translated Sentence: ('နေတယ်', 'ငါ', 'ค่อนข้าง', 'เหนื่อย')
Translation BLEU Scores [0.08034284189446518, 0.31947155212313627, 0.15973577606156814, 0.061033220311973134, 0.09622504486493762, 0.1394721495522781]

Sentence Index:  636
Myanmar Sentence: ['ကျေးဇူးပြုပြီး', 'သူမ', 'ကို', 'တောင်းပန်လိုက်ပါ', '။']
Reference Thai Sentence: ['โปรด', 'ขอโทษ', 'เธอ']
Translated Sentence: ('သူမ', 'โปรด', 'ကို')
Translation BLEU Scores [0.11362193664674995, 0.408248290463863, 0.2259005009024612, 0.07249749990681824, 0.10691671651659736, 0.15587075056736388]

Sentence Index:  637
Myanmar Sentence: ['ခင်ဗျား', 'ဟိုမှာ', 'အမြဲနေ', 'သွား', 'မှာလား', '။']
Reference Thai Sentence: ['คุณ', 'จะ', 'อยู่', 'ที่นั่น', 'เสมอ', 'หรือไม่', '?']
Translated Sentence: ('မှာလား', 'ခင်ဗျား', 'ที่นั่น', 'သွား')
Translation BLEU Scores [0.037951271263104894, 0.15090767577522726, 0.07545383788761363, 0.028830051881449627, 0.04545349273020006, 0.06588197848738886]

Sentence Index:  638
Myanmar Sentence: ['ခင်ဗျားက', 'အိမ်ထောင်နဲ့', 'လား', 'လူလွတ်', 'လား', '။']
Reference Thai Sentence: ['คุณ', 'แต่งงาน', 'หรือ', 'ว่างเปล่า', '?']
Translated Sentence: ('หมายความว่า', 'แล้ว', 'แล้ว', 'လား', 'လား')
Translation BLEU Scores [0, 0, 0, 0, 0, 0]

Sentence Index:  639
Myanmar Sentence: ['သူတို့', 'အဲဒီ', 'မှာ', 'မ', 'စု', 'ခဲ့ကြဘူး', '။']
Reference Thai Sentence: ['พวกเขา', 'ไม่', 'ได้', 'รวบรวม', 'ไว้', 'ที่นั่น']
Translated Sentence: ('မှာ', 'သူတို့', 'မ', 'အဲဒီ', 'เงิน', 'ได้มา')
Translation BLEU Scores [0, 0, 0, 0, 0, 0]

Sentence Index:  640
Myanmar Sentence: ['မနေ့က', 'နေ့လယ်', 'က', 'ကျွန်တော်', 'တီဗွီ', 'ကြည့်နေခဲ့ပါတယ်', '။']
Reference Thai Sentence: ['ฉัน', 'กำลัง', 'ดู', 'ทีวี', 'เมื่อ', 'บ่าย', 'วาน', 'นี้']
Translated Sentence: ('က', 'เมื่อวาน', 'ไป', 'ကျွန်တော်', 'ทีวี')
Translation BLEU Scores [0.029486824119076216, 0.13186908634166958, 0.05862502026550899, 0.0250530827696685, 0.04928879601851102, 0.06648804400266797]
```
