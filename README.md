# Voice Phishing Detection — KoBERT + Lexicon Ensemble

**F1 0.80 / ROC-AUC 0.89 on held-out test set (n=303)**

> An NLP pipeline that detects voice phishing calls by combining OpenAI Whisper STT
> with a custom KoBERT + Lexicon ensemble model, deployed via Flask on Naver Cloud Platform GPU.

---

## Highlights

- Built **end-to-end pipeline**: Speech → Whisper STT → KoBERT inference → REST API → React frontend
- Designed **KoBERTWithLexicon**, a custom PyTorch module concatenating KoBERT's CLS embedding with log-odds lexicon features
- Handled **severe class imbalance** (728K normal vs 1.5K fraud) via stratified balanced sampling
- Tuned **decision threshold** to maximize Recall ≥ 0.90, minimizing missed fraud detections

---

## Results

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

## Model Architecture

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

## Pipeline

    Audio File Upload
          ↓
    [OpenAI Whisper Large]  Speech → Text
          ↓
    [Preprocessing]  Tokenization, noise removal
          ↓
    [KoBERTWithLexicon]  Context + keyword feature ensemble
          ↓
    Threshold (0.50) → "Voice Phishing Detected"

---

## Dataset

| Split | Size |
|-------|------|
| Train | 2,424 |
| Validation | 303 |
| Test | 303 |

- **Positive**: 1,515 voice phishing transcripts
- **Negative**: 1,515 sampled from 728,257 normal dialogues (balanced sampling)
- Stratified 80 / 10 / 10 split

---

## Tech Stack

`Python` `PyTorch` `HuggingFace Transformers` `KoBERT` `OpenAI Whisper` `Scikit-learn` `Flask` `Pandas` `Naver Cloud Platform (GPU)`

---

## Training Config

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

## My Contributions *(4-person team)*

| Area | What I did |
|------|------------|
| **Model (led)** | Designed KoBERTWithLexicon architecture, fine-tuning, threshold tuning |
| **Data & Evaluation (shared)** | Balanced sampling, lexicon feature extraction, confusion matrix, PR/ROC analysis |
| **Backend** | Flask API integration (STT → model inference → JSON response) |

---

## Limitations & Next Steps

- Real-time streaming inference not yet implemented
- Lexicon vocabulary needs periodic updates as phishing scripts evolve
- Plan to add deepfake voice (AI-synthesized) samples for robustness

---

---

# 보이스피싱 탐지 모델 — KoBERT + Lexicon 앙상블

**테스트셋(n=303) 기준 F1 0.80 / ROC-AUC 0.89**

> OpenAI Whisper STT와 커스텀 KoBERT + Lexicon 앙상블 모델을 결합하여
> 보이스피싱 음성을 탐지하는 NLP 파이프라인입니다.
> Flask 기반 REST API로 네이버 클라우드 플랫폼 GPU 서버에 배포했습니다.

---

## 주요 성과

- **엔드투엔드 파이프라인** 구축: 음성 → Whisper STT → KoBERT 추론 → REST API → React 프론트엔드
- **KoBERTWithLexicon** 설계: KoBERT CLS 임베딩과 Log-odds Lexicon 피처를 결합한 커스텀 PyTorch 모듈
- **클래스 불균형 처리**: 728K 정상 대화 vs 1.5K 보이스피싱 → 균형 샘플링으로 해결
- **임계값 튜닝**: Recall ≥ 0.90 달성을 위한 최적 임계값 탐색으로 미탐지 최소화

---

## 성능

| 지표 | 값 |
|------|----|
| Accuracy | 79.87% |
| Precision | 78.48% |
| **Recall** | **82.12%** |
| **F1-Score** | **80.26%** |
| AUPRC | 87.83% |
| **ROC-AUC** | **89.05%** |

> 보이스피싱을 놓치는 FN이 오탐(FP)보다 실제 피해가 크므로 Recall을 우선 지표로 설정했습니다.

---

## 모델 아키텍처

    입력 텍스트
      ├── [KoBERT]   CLS 토큰 임베딩    → 768차원
      └── [Lexicon]  Log-odds 키워드 피처 → F차원
                            ↓
                  Concat → Dropout(0.1)
                            ↓
                 Linear Classifier (이진 분류)
                            ↓
               Softmax → 보이스피싱 확률

---

## 파이프라인

    음성 파일 업로드
          ↓
    [OpenAI Whisper Large]  음성 → 텍스트
          ↓
    [전처리]  토큰화, 노이즈 제거
          ↓
    [KoBERTWithLexicon]  문맥 + 키워드 피처 앙상블
          ↓
    임계값(0.50) 초과 → "보이스피싱 탐지"

---

## 데이터셋

| Split | 건수 |
|-------|------|
| Train | 2,424 |
| Validation | 303 |
| Test | 303 |

- **양성**: 보이스피싱 텍스트 1,515건
- **음성**: 일반 대화 728,257건에서 균형 샘플링 1,515건
- Stratified 80 / 10 / 10 분할

---

## 기술 스택

`Python` `PyTorch` `HuggingFace Transformers` `KoBERT` `OpenAI Whisper` `Scikit-learn` `Flask` `Pandas` `Naver Cloud Platform (GPU)`

---

## 학습 설정

| 파라미터 | 값 |
|----------|----|
| 모델 | skt/kobert-base-v1 |
| Epochs | 3 |
| Learning Rate | 5e-5 |
| Optimizer | AdamW (weight_decay=0.01) |
| Scheduler | Linear Warmup |
| Max Length | 256 |
| Batch Size | 32 |

---

## 본인 담당 역할 *(4인 팀)*

| 역할 | 내용 |
|------|------|
| **모델 (본인 주도)** | KoBERTWithLexicon 아키텍처 설계, fine-tuning, 임계값 튜닝 |
| **데이터 · 평가 (팀 공동)** | 균형 샘플링, Lexicon 피처 추출, Confusion Matrix, PR/ROC Curve 분석 |
| **백엔드** | Flask API 연동 (STT → 모델 추론 → JSON 반환) |

---

## 한계 및 개선 방향

- 실시간 스트리밍 음성 탐지 미구현
- 보이스피싱 수법 진화에 따른 Lexicon 업데이트 자동화 필요
- 딥페이크 음성(AI 합성) 데이터 추가 학습 예정
