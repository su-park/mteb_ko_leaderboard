# 한글 텍스트 임베딩 모델 리더보드

Date: 2024년 8월 12일
Status: In progress

### Agenda

---

- 한글 텍스트 측정이 가능한 모델만을 대상
- 최근 확보한 GPU를 활용하여 짬짬이 성능 측정 (진행중)
- 최신 모델 또는 연구 결과 입수시 지속적으로 업데이트 예정


### 대상 모델

---

- 한글 텍스트 측정이 가능한 임베딩 모델
- MTEB 리더보드 랭킹순 (언어: 영어)

| no. | Model | Memory Usage (GB) | Embedding Dimensions | Max Tokens | Source |
| --- | --- | --- | --- | --- | --- |
| 1 | SFR-Embedding-2_R | 26.49 | 4096 | 32768 | Salesforce/SFR-Embedding-2_R |
| 2 | gte-Qwen2-7B-instruct | 28.36  | 3584 | 131072 | Alibaba-NLP/gte-Qwen2-7B-instruct |
| 3 | bge-multilingual-gemma2 | 34.43 | 3584 | 8192 | BAAI/bge-multilingual-gemma2 |
| 4 | e5-mistral-7b-instruct | 26.49 | 4096 | 32768 | intfloat/e5-mistral-7b-instruct |
| 5 | LLM2Vec-Meta-Llama-3-supervised  | 27.96 | 4096 | 8192 | McGill-NLP/LLM2Vec-Meta-Llama-3-8B-Instruct-mntp-supervised |
| 6 | multilingual-e5-large-instruct | 2.09  | 1024 | 514 | intfloat/multilingual-e5-large-instruct |
| 7 | multilingual-e5-large  | 2.09  | 1024 | 514 | intfloat/multilingual-e5-large |
| 8 | bge-m3 | 2.11 | 1024 | 8192 | BAAI/bge-m3 |
| 9 | ko-sroberta-multitask  | 1.04 | 768 | 512 | jhgan/ko-sroberta-multitask |

### 한글 텍스트 입력 범위

---

- 모델마다 토크나이저가 상이하여 같은 문장이지만 모델마다 다른 토큰 길이를 보임
- 아래에서는 샘플 입력 텍스트를 기준으로 모델마다 입력 가능한 텍스트 길이를 확인하고자 함
- 샘플 입력 텍스트는 제갈량의 출사표

`bge-m3`

- 출사표 전문 입력 가능함

![plot_1.png](output/plot_1.png)

`Sentence-Roberta-Multitask`

- 출사표 2~3 문단 입력 가능함

![plot_2.png](output/plot_2.png)

`Multilingual-E5-Large`

- 출사표 2~3 문단 입력 가능함

![plot_3.png](output/plot_3.png)

### MTEB 평가

---

- 텍스트 임베딩의 성능을 다면적으로 평가하기 위한 벤치마크 셋
- 평가셋이 대부분 영어 위주로 구성되어 아래에서는 한글 데이터셋만 대상으로 성능 측정

| Task | Definition | Metric |
| --- | --- | --- |
| BitextMining | task of finding parallel sentences in two languages | F1 |
| Classification | task of assigning a label to a text | Accuracy |
| Clustering | task of grouping similar documents together | Validity Measure (V-measure) |
| PairClassification | task of determining whether two texts are similar | Average Precision |
| Reranking | task of reordering a list of documents to improve relevance | Mean Average Precision |
| Retrieval | task of finding relevant documents for a query | nDCG@10 |
| STS | task of determining how similar two texts are | Spearman Correlation |

### 성능 평가

---

| no. | Model | BitextMining kor-eng  | BitextMining eng-kor  | Classification | MultiLabel Classification | Clustering | PairClassification | Reranking  | Retrieval | STS |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SFR-Embedding-2_R | 94.49 | 95.79 | 68.74 | 9.80 | 53.78 | 60.42 | 48.17 | 73.18 | 81.16 |
| 2 | gte-Qwen2-7B-instruct | 94.32 | 95.11 | 72.33 | 9.22 | 53.27 | 74.15 | 50.05 | 74.96 | 85.70 |
| 3 | bge-multilingual-gemma2 | 94.78 | 95.56 | 69.06 | 13.14 | 40.10 | 68.98 | 37.06 | 58.87 | 83.83 |
| 4 | e5-mistral-7b-instruct | 94.53 | 95.94 | 67.47 | 9.08 | 58.02 | 63.08 | 54.92 | 63.08 | 84.54 |
| 5 | LLM2Vec-Meta-Llama-3-supervised | 94.37 | 95.33 | 68.27 | 9.37 | 47.01 | 65.45 | 52.63 | 68.43 | 83.44 |
| 6 | multilingual-e5-large-instruct | 94.91 | 95.83 | 67.03 | 10.20 | 55.76 | 61.18 | 52.86 | 74.52 | 85.15 |
| 7 | multilingual-e5-large | 93.64 | 94.33 | 62.96 | 9.18 | 39.02 | 57.55 | 54.87 | 73.47 | 80.62 |
| 8 | bge-m3 | 94.53 | 95.81 | 63.48 | 10.92 | 38.04 | 61.20 | 59.98 | 72.29 | 83.13 |
| 9 | ko-sroberta-multitask | 69.66 | 57.56 | 61.62 | 8.93 | 36.41 | 65.64 | 48.33 | 60.98 | 85.39 |

| no. | Model | Tatoeba BitextMining kor-eng | Flores BitextMining eng-kor  | Flores BitextMining kor-eng | NTREX BitextMining eng-kor | NTREX BitextMining kor-eng | IWSLT2017 BitextMining eng-kor | IWSLT2017 BitextMining kor-eng | MassiveIntent Classification | MassiveScenario Classification | Klue-TC Classification | SIB200 Classification | MultilingualSentiment Classification | KorHate Classification | KorSarcasm Classification  | KorHateSpeechML MultiLabel Classification | SIB200ClusteringS2S Clustering | Klue-NLI PairClassification | PawsX PairClassification | MIRACL Reranking | Ko-StrategyQA Retrieval | XPQA Retrieval | PublicHealthQA Retrieval | Belebele Retrieval | MIRACL Retrieval | STS17 STS  | KorSTS STS | Klue-STS STS  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SFR-Embedding-2_R | 91.04 | 100.00 | 100.00 | 98.81 | 99.33 | 88.57 | 87.59 | 75.40 | 85.67 | 58.50 | 80.19 | 79.68 | 45.57 | 56.15 | 9.80 | 53.78 | 67.49 | 53.34 | 48.17 | 77.27 | 37.24 | 86.35 | 91.85 |  | 78.86 | 77.39 | 87.24 |
| 2 | gte-Qwen2-7B-instruct | 91.40 | 100.00 | 100.00 | 98.83 | 99.09 | 86.51 | 86.77 | 79.25 | 86.42 | 69.20 | 86.12 | 78.22 | 46.94 | 60.13 | 9.22 | 53.27 | 79.57 | 68.72 | 50.05 | 81.07 | 37.81 | 86.15 | 94.79 |  | 83.86 | 83.47 | 89.77 |
| 3 | bge-multilingual-gemma2  | 92.35 | 100.00 | 99.86 | 98.16 | 98.99 | 88.51 | 87.90 | 73.56 | 81.10 | 63.61 | 77.15 | 81.69 | 48.84 | 57.46 | 13.14 | 40.10 | 80.12 | 57.84 | 37.06 | 51.33 | 37.67 | 65.89 | 80.59 | 19.57 | 81.38 | 81.61 | 88.49 |
| 4 | e5-mistral-7b-instruct | 91.28 | 100.00 | 100.00 | 98.68 | 99.13 | 89.15 | 87.71 | 69.87 | 74.39 | 60.15 | 84.16 | 80.74 | 45.97 | 57.01 | 9.08 | 58.02 | 73.02 | 53.14 | 54.92 | 79.30 | 39.22 | 88.71 | 92.61 |  | 83.69 | 82.27 | 87.65 |
| 5 | LLM2Vec-Meta-Llama-3-supervised | 90.94 | 100.00 | 99.86 | 98.07 | 98.82 | 87.92 | 87.86 | 75.15 | 79.87 | 60.83 | 79.85 | 79.30 | 44.58 | 58.33 | 9.37 | 47.01 | 77.57 | 53.33 | 52.63 | 70.44 | 36.58 | 79.76 | 86.95 | 34.90 | 81.22 | 81.06 | 88.03 |
| 6 | multilingual-e5-large-instruct | 91.76 | 100.00 | 100.00 | 99.08 | 99.39 | 88.42 | 88.49 | 64.15 | 70.50 | 62.24 | 81.71 | 78.29 | 45.30 | 58..17 | 10.20 | 55.76 | 70.54 | 51.81 | 52.86 | 79.86 | 39.74 | 84.87 | 93.60 |  | 84.30 | 82.71 | 88.44 |
| 7 | multilingual-e5-large | 89.70 | 99.42 | 99.86 | 96.46 | 98.93 | 87.11 | 86.08 | 63.74 | 70.66 | 59.68 | 74.60 | 72.57 | 43.18 | 56.26 | 9.18 | 39.02 | 63.42 | 51.68 | 54.87 | 79.82 | 36.99 | 82.88 | 94.18 | 65.56 | 81.04 | 79.24 | 81.59 |
| 8 | bge-m3 | 90.44 | 99.86 | 100.00 | 98.60 | 99.08 | 88.96 | 88.59 | 66.53 | 72.90 | 54.67 | 71.91 | 78.16 | 43.38 | 56.79 | 10.92 | 38.04 | 70.05 | 52.34 | 59.98 | 79.40 | 36.15 | 80.41 | 93.18 | 70.14 | 81.42 | 80.26 | 87.70 |
| 9 | ko-sroberta-multitask | 61.05 | 72.31 | 86.39 | 52.16 | 71.95 | 48.20 | 59.26 | 64.80 | 70.12 | 52.10 | 69.75 | 73.82 | 43.67 | 57.11 | 8.93 | 36.41 | 78.38 | 52.89 | 48.33 | 65.10 | 27.96 | 69.21 | 81.63 | 36.69 | 86.46 | 85.58 | 84.13 |
