# NTU RGB +D SKELETON ACTION RECOGNITION

## 📌 Introduction
이 레포지토리는 **Spatial Temporal Graph Convolutional Networks (ST-GCN)** 기반으로  
NTU RGB+D Skeleton 데이터를 이용해 **빠른 인간 행동(Quick Actions)** 을 분류하는 프로젝트입니다.

> ⚠️ **원본 레포지토리 출처**  
> 이 프로젝트는 아래 레포지토리를 기반으로 수정 및 재구성되었습니다.  
> 🔗 https://github.com/MeshalAlamr/quick-action-recognition

---

## 📦 Prerequisites
- Python 3.6+
- PyTorch (CUDA 지원 권장)
- NumPy, Pandas  
- Jupyter Notebook

---

## 📁 Dataset Preparation

이 프로젝트는 **NTU-RGB+D (x-view protocol)** 스켈레톤 데이터를 사용합니다.

### ✔ 1. NTU-RGB+D Skeleton 데이터 준비
NTU RGB+D 공식 사이트 또는 원본 레포에서 제공한 링크를 통해 데이터를 받습니다.  
(원본 데이터 자체는 이 레포에는 포함되지 않음)

---

### ✔ 2. 다운샘플링 & 12개 클래스 데이터 생성

레포 안의 **`gitclone_preprocessing_data.ipynb`** 를 실행하면:

- NTU RGB+D skeleton 데이터를 **프레임 반으로 다운샘플링**하고
- 전체 60개 액션 중 **이 프로젝트에서 사용하는 12개 액션만 선택**
- 학습/검증용 **4개의 데이터 파일**이 자동 생성됩니다.

  <repo_root>/data/NTU-RGB-D/x-view/
├── small_train_data.npy
├── small_train_label.pkl
├── small_val_data.npy
└── small_val_label.pkl


각 파일은 다음 의미를 가집니다:

| 파일명 | 설명 |
|-------|------|
| `small_train_data.npy` | 학습용 skeleton 시퀀스 |
| `small_train_label.pkl` | 학습용 라벨 |
| `small_val_data.npy` | 검증용 skeleton 시퀀스 |
| `small_val_label.pkl` | 검증용 라벨 |

---

## 🧪 Training

모델 학습은 **`model.ipynb`** 파일을 실행해서 진행합니다.

Notebook에서 다음 작업이 자동으로 수행됩니다:

- 데이터 로드  
- 그래프(A matrix) 생성  
- ST-GCN 모델 구성  
- 학습 및 검증 실행  
- 모델 저장  

---

## 📊 Visualization
Skeleton 시각화 예제는 `visualize/` 폴더에 위치합니다.  
(MATLAB 기반 예제가 포함되어 있으며 필요 시 Python 버전도 작성 가능)

---

## 📚 Reference
Sijie Yan et al., 2018.  
**Spatial Temporal Graph Convolutional Networks for Skeleton-Based Action Recognition.**

🔗 https://arxiv.org/abs/1801.07455

---


---

### ✔ 3. 생성된 4개 파일의 저장 위치

Notebook 실행 후 데이터는 아래 경로에 저장됩니다:

