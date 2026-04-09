# 🎙️ Voice Phishing Detection — KoBERT + Lexicon Ensemble

**F1 0.80 / ROC-AUC 0.89 on held-out test set (n=303)**

> An NLP pipeline that detects voice phishing calls by combining OpenAI Whisper STT with a custom KoBERT + Lexicon ensemble model, deployed via Flask on Naver Cloud Platform GPU.

---

## 🏆 Highlights

- Built **end-to-end pipeline**: Speech → Whisper STT → KoBERT inference → REST API → React frontend
- Designed **KoBERTWithLexicon**, a custom PyTorch module concatenating KoBERT's CLS embedding with log-odds lexicon features
- Handled **severe class imbalance** (728K normal vs 1.5K fraud) via stratified balanced sampling
- Tuned **decision threshold** to maximize Recall ≥ 0.90, minimizing missed fraud detections

---

## 📊 Results

| Metric | Score |
|--------|-------|
| Accuracy | 79.87% |
| Precision | 78.48% |
| **Recall** | **82.12%** |
| **F1-Score** | **80.26%** |
| AUPRC | 87.83% |
| **ROC-AUC** | **89.05%** |

> Recall prioritized over Precision — a missed fraud call causes greater real-world harm than a false alarm.

---

## 🧠 Model Architecture

    Input Text
      ├── [KoBERT]   CLS token embedding    → 768-dim
      └── [Lexicon]  Log-odds keyword feats → F-dim
                            ↓
                  Concat → Dropout(0.1)
                            ↓
                 Linear Classifier (binary)
                            ↓
               Softmax → fraud probability

---

## 🔄 Pipeline

    Audio File Upload
          ↓
    [OpenAI Whisper Large]  Speech → Text
          ↓
    [Preprocessing]  Tokenization, noise removal
          ↓
    [KoBERTWithLexicon]  Context + keyword feature ensemble
          ↓
    Threshold (0.50) → "Voice Phishing Detected" 🚨

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

## 👩‍💻 My Contributions (4-person team)

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
