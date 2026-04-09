#🎙️ Voice Phishing Detection — KoBERT + Lexicon Ensemble
Achieved F1 0.80 / ROC-AUC 0.89 on held-out test set (n=303)
An NLP pipeline that detects voice phishing calls in real time by combining OpenAI Whisper STT with a custom KoBERT + Lexicon ensemble model, deployed as a Flask web service on Naver Cloud Platform GPU.

highlights

Built end-to-end pipeline: speech → Whisper STT → KoBERT inference → REST API → React frontend
Designed KoBERTWithLexicon, a custom PyTorch module that concatenates KoBERT's CLS embedding with log-odds lexicon features, outperforming KoBERT-only baseline
Handled severe class imbalance (728K normal vs 1.5K fraud samples) via stratified balanced sampling
Tuned decision threshold to maximize Recall ≥ 0.90, minimizing missed fraud detections (FN=27)


results
MetricScoreAccuracy79.87%Precision78.48%Recall82.12%F180.26%AUPRC87.83%ROC-AUC89.05%

Recall prioritized over Precision — a missed fraud call causes greater harm than a false alarm.


model architecture
Input Text
  ├── [KoBERT]   CLS token embedding    → 768-dim
  └── [Lexicon]  Log-odds keyword feats → F-dim
                        ↓
              Concat → Dropout(0.1)
                        ↓
             Linear Classifier (binary)
                        ↓
           Softmax → fraud probability

dataset
SplitSizeTrain2,424Validation303Test303

Positive: 1,515 voice phishing transcripts
Negative: sampled 1,515 from 728,257 normal dialogues (balanced)
Stratified 80 / 10 / 10 train-val-test split


tech stack
Python PyTorch HuggingFace Transformers KoBERT OpenAI Whisper Scikit-learn Flask Pandas Naver Cloud Platform (GPU)

training config
pythonMODEL     = "skt/kobert-base-v1"
EPOCHS    = 3
LR        = 5e-5
OPTIMIZER = AdamW  (weight_decay=0.01)
SCHEDULER = LinearWarmup
MAX_LEN   = 256
BATCH     = 32

my contributions (4-person team)
AreaWhat I didModelDesigned KoBERTWithLexicon architecture, fine-tuning, threshold tuningDataBalanced sampling strategy, lexicon feature extraction & normalizationEvaluationConfusion matrix, PR/ROC curves, AUPRC, ROC-AUC analysisBackendFlask API integration (STT → model inference → JSON response)

limitations & next steps

Real-time streaming inference not yet implemented
Lexicon vocabulary needs periodic updates as phishing scripts evolve
Plan to add deepfake voice (AI-synthesized) samples for robustness

