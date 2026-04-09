---

## 📁 Dataset

| Split | Size |
|-------|------|
| Train | 2,424 |
| Validation | 303 |
| Test | 303 |

- **Positive**: 1,515 voice phishing transcripts  
- **Negative**: 1,515 sampled from 728,257 normal dialogues (balanced sampling)  
- Stratified 80 / 10 / 10 split

---

## ⚙️ Tech Stack

`Python` `PyTorch` `HuggingFace Transformers` `KoBERT` `OpenAI Whisper` `Scikit-learn` `Flask` `Pandas` `Naver Cloud Platform (GPU)`

---

## 🔧 Training Config

| Parameter | Value |
|-----------|-------|
| Model | skt/kobert-base-v1 |
| Epochs | 3 |
| Learning Rate | 5e-5 |
| Optimizer | AdamW (weight_decay=0.01) |
| Scheduler | Linear Warmup |
| Max Length | 256 |
| Batch Size | 32 |

---

## 👩‍💻 My Contributions *(4-person team)*

| Area | What I did |
|------|------------|
| **Model** | Designed KoBERTWithLexicon architecture, fine-tuning, threshold tuning |
| **Data** | Balanced sampling strategy, lexicon feature extraction & normalization |
| **Evaluation** | Confusion matrix, PR/ROC curves, AUPRC, ROC-AUC analysis |
| **Backend** | Flask API integration (STT → model inference → JSON response) |

---

## 🚧 Limitations & Next Steps

- Real-time streaming inference not yet implemented  
- Lexicon vocabulary needs periodic updates as phishing scripts evolve  
- Plan to add deepfake voice (AI-synthesized) samples for robustness

---

## 🗂️ Project Structure
