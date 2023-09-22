# Indonesian DistilBERT Base fine-tuned on Translated Squad v2.0

This repo was forked from [rifkybujana](https://github.com/rifkybujana)'s [IndoBERT-QA](https://github.com/rifkybujana/IndoBERT-QA) for its excellent notebook presentation. Both this repo and IndoBERT-QA has the same task and was trained on the same [Translated SQuAD 2.0](https://github.com/Wikidepia/indonesian_datasets/tree/master/question-answering/squad) for **Extractive Q&A** downstream task.

The difference lies in the underlying pretrained model in which IndoBERT-QA uses [IndoBERT](https://huggingface.co/indolem/indobert-base-uncased) trained by [IndoLEM](https://indolem.github.io/) whereas this repo uses [Indonesian DistilBERT base](https://huggingface.co/cahya/distilbert-base-indonesian) by [cahya](https://huggingface.co/cahya).

**Model Size** (after training): XXmb

## Details of Indonesian DistilBERT base (from their documentation)

This model is a distilled version of the [Indonesian BERT base model](https://huggingface.co/cahya/bert-base-indonesian-1.5G). This model is uncased.

This is one of several other language models that have been pre-trained with indonesian datasets. More detail about its usage on downstream tasks (text classification, text generation, etc) is available at [Transformer based Indonesian Language Models](https://github.com/cahya-wirawan/indonesian-language-models/tree/master/Transformers).

## Details of the downstream task (Q&A) - Dataset

SQuAD2.0 combines the 100,000 questions in SQuAD1.1 with over 50,000 unanswerable questions written adversarially by crowdworkers to look similar to answerable ones. To do well on SQuAD2.0, systems must not only answer questions when possible, but also determine when no answer is supported by the paragraph and abstain from answering.

| Dataset  | Split | # samples |
| -------- | ----- | --------- |
| SQuAD2.0 | train | 130k      |
| SQuAD2.0 | eval  | 12.3k     |

## Model Training

The model was trained on Kaggle with poorman's GPU and 12GB of RAM.

## Results:

| Metric | # Value   |
| ------ | --------- |
| **EM** | **51.61** |
| **F1** | **69.09** |

## Simple Usage (Using Huggingface)

```py
from transformers import pipeline

qa_pipeline = pipeline(
    "question-answering",
    model="boimbukanbaim/indonesian-distilbert-finetuned-squad",
    tokenizer="boimbukanbaim/indonesian-distilbert-finetuned-squad"
)

qa_pipeline({
    'context': """
    Dalam matematika, turunan atau derivatif dari sebuah fungsi adalah cara mengukur sensitivitas perubahan nilai fungsi terhadap perubahan pada nilai variabelnya. Sebagai contoh, turunan dari posisi sebuah benda bergerak terhadap waktu mengukur kecepatan benda bergerak ketika waktu berjalan. Turunan adalah alat penting dalam kalkulus.
    
    Turunan sebuah fungsi satu variabel di suatu titik, jika itu ada, adalah kemiringan dari garis singgung dari grafik fungsi di titik tersebut. Garis singgung adalah hampiran (aproksimasi) linear terbaik dari fungsi di sekitar titik tersebut. Konsep turunan dapat diperumum untuk fungsi multivariabel. Dalam perumuman ini, turunan dianggap sebagai transformasi linear, dengan translasi yang sesuai, menghasilkan hampiran linear dari grafik fungsi multivariabel tersebut. Matriks Jacobi adalah matriks yang merepresentasikan transformasi linear terhadap suatu basis yang ditentukan. Matriks ini dapat ditentukan dengan turunan parsial dari variabel-variabel independen. Pada fungsi multivariabel bernilai real, matriks Jacobi tereduksi menjadi vektor gradien.
    
    Proses menemukan turunan disebut diferensiasi. Kebalikan proses ini disebut dengan antiturunan. Teorema fundamental kalkulus menyatakan hubungan diferensiasi dengan integrasi. Turunan dan integral adalah dua operasi dasar dalam kalkulus satu-variabel.
    
    Konsep turunan fungsi yang universal banyak digunakan dalam berbagai cabang matematika maupun bidang ilmu yang lain. Dalam bidang ekonomi, turunan digunakan untuk menghitung biaya marginal, total penerimaan, dan biaya produksi. Bidang biologi menggunakan turunan untuk menghitung laju pertumbuhan mikroorganisme, dalam bidang fisika untuk menghitung kepadatan kawat, dalam bidang kimia untuk menghitung laju pemisahan, dalam bidang geografi untuk menghitung laju pertumbuhan penduduk, dan masih banyak lagi.
    """,
    'question': "Apa itu turunan?"
})
```

*output:*

```py
{
    'score': 0.1216660588979721,
    'start': 61,
    'end': 159,
    'answer': 'adalah cara mengukur sensitivitas perubahan nilai fungsi terhadap perubahan pada nilai variabelnya'
}
```