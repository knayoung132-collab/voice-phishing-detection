# 🎙️ Voice Phishing Detection — KoBERT + Lexicon Ensemble

**F1 0.80 / ROC-AUC 0.89 on held-out test set (n=303)**

> An NLP pipeline that detects voice phishing calls by combining OpenAI Whisper STT  
> with a custom KoBERT + Lexicon ensemble model, deployed via Flask on Naver Cloud Platform GPU.

---

## 🏆 Highlights

- Built **end-to-end pipeline**: Speech → Whisper STT → KoBERT inference → REST API → React frontend
- Designed **`KoBERTWithLexicon`**, a custom PyTorch module concatenating KoBERT's CLS embedding with log-odds lexicon features
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
