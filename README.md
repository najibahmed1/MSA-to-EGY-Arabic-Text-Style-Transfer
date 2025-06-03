# Modern Standard Arabic to Egyptian Arabic Text Style Transfer

This project explores text style transfer (TST) between Modern Standard Arabic (MSA) and Egyptian Arabic (EGY) using fine-tuned Arabic language models. The aim is to convert formal written MSA into the informal, spoken dialect of Egyptian Arabic — and vice versa — while preserving semantic meaning and stylistic accuracy.

## Overview

Arabic consists of a formal written standard (MSA) and many spoken dialects, such as Egyptian Arabic (EGY). This linguistic diversity poses challenges for natural language processing (NLP). We treat the transformation between MSA and EGY as a text style transfer task rather than a standard translation task.

Our project evaluates two pre-trained Arabic models:
- **AraGPT2** (decoder-only)
- **AraT5** (encoder-decoder)

We fine-tune these models using parallel datasets and evaluate them using both automatic metrics (BLEU) and human evaluation.

## Datasets

We used two publicly available parallel corpora:
- **MADAR Corpus**: Includes MSA and dialectal Arabic translations, including EGY and Levantine.
- **Dial2MSA**: A dataset of Egyptian dialect tweets and their corresponding MSA translations.

Minimal preprocessing was done to ensure clean and consistent formatting.

## Methods

### Model Architectures
- **AraGPT2**: Decoder-only; struggled with style transfer, produced hallucinations and repetitive text.
- **AraT5**: Encoder-decoder; significantly better at preserving meaning and applying correct stylistic features.

### Fine-Tuning
- Fine-tuned models separately for MSA→EGY and EGY→MSA.
- Used Hugging Face’s `Trainer` with:
  - 3 epochs
  - Learning rate of 3e-4
  - Batch size of 4 per device (effective 8 with accumulation)
  - Checkpoint selection based on lowest validation loss

### Evaluation
- **BLEU Scores** (via SacreBLEU)
- **Human Ratings** from native speakers based on:
  - Fluency
  - Accuracy
  - Naturalness

Each generated sample was scored out of 100.

## Results

| Direction         | Baseline BLEU | Fine-Tuned BLEU | Human Evaluation (/100) |
|------------------|----------------|------------------|--------------------------|
| MSA → EGY        | 0.000          | 0.148            | 66.5                     |
| EGY → MSA        | 0.001          | 0.195            | 88.0                     |
| MSA → Levantine  | 0.000          | 0.170            | 65.0                     |
| Levantine → MSA  | 0.000          | 0.235            | 68.0                     |

- AraT5 consistently outperformed AraGPT2.
- Generating MSA from dialectal input yielded better results than the reverse due to standardization.
- BLEU scores are modest but a significant improvement over baseline.

## Key Insights

- AraGPT2 often failed due to its decoder-only architecture.
- AraT5 handled conditional generation better and showed high fluency and stylistic fidelity.
- Translating into EGY remains harder due to dialectal variation and lack of standardization.
- BLEU scores underestimated quality — human evaluations were more optimistic.

## Limitations

- Evaluations based on limited human samples (10 per direction).
- Performance constrained by available data and pre-trained model size.
- Style transfer into dialect remains an open challenge due to multiple valid outputs.

## Future Work

- Build larger and more diverse MSA-EGY datasets.
- Explore reinforcement learning (e.g., RLHF) for fine-tuning based on human preferences.
- Conduct detailed linguistic error analysis (e.g., morphology, syntax).
- Develop an interactive tool for public demos and real-time feedback.

## Authors

- Najib Ahmed  
- Akram Abdulaziz  
- Bashir Ahmed  
- Yi-Chyun Wong  

## References

- Antoun et al., “AraGPT2: Pre-trained transformer for Arabic language generation,” 2020. [arXiv:2012.15520](https://arxiv.org/abs/2012.15520)
- Nagoudi et al., “AraT5: Text-to-text transformers for Arabic language generation,” ACL 2022.
- Bouamor et al., “MADAR Arabic Dialect Corpus and Lexicon,” LREC 2018.
- Mubarak & Darwish, “Dial2MSA: Tweets corpus for converting dialectal Arabic to MSA,” 2018.
- Papineni et al., “BLEU: a method for automatic evaluation of machine translation,” ACL 2002.
- Post, “A call for clarity in reporting BLEU scores,” WMT 2018.
