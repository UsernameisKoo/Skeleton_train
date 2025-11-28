# NTU RGB +D SKELETON ACTION RECOGNITION

## 📌 Introduction
이 레포지토리는 **Spatial Temporal Graph Convolutional Networks (ST-GCN)** 기반으로  
NTU RGB+D Skeleton 데이터를 이용해 **빠른 인간 행동(Quick Actions)** 을 분류하는 프로젝트입니다.

> ⚠️ **원본 레포지토리 출처**  
> 이 프로젝트는 아래 레포지토리를 기반으로 수정 및 재구성되었습니다.  
> 🔗 https://github.com/MeshalAlamr/quick-action-recognition

---

## 🙋‍♂️ My Contributions

이 프로젝트에서 제가 직접 수정·개선한 파일은 아래 두 개입니다:

### **1. `main.ipynb`**  
ST-GCN 모델 구현 및 학습 성능을 개선하기 위해 다음과 같은 작업을 수행했습니다:

- Dropout 비율 조정: `[0.1, 0.2, 0.3, 0.3, 0.3, 0.3]`  
- `early_stop_patience` 기능 추가  
- Gradient Accumulation Step 도입  
- 스케줄러를 `OneCycleLR`로 변경  
- 학습 안정성을 위한 `WeightedRandomSampler` 적용  
- Cross Entropy Loss → **Focal Loss**로 변경  
→ 이를 통해 모델의 학습 안정성과 성능이 전반적으로 개선되었습니다.

---

### **2. `gitclone_dataprocessing.ipynb`**  
Colab 환경에서의 프로젝트 세팅 및 데이터 전처리를 자동화하기 위해 다음 기능을 구현했습니다:

- GitHub 레포지토리 자동 클론  
- NTU RGB+D skeleton 데이터의 **train / val / test 분리 추가**  
- 기존 코드에서 사용하던 **다운샘플링 과정을 제거하여 성능 개선**  
- Colab 환경에 맞춘 경로 설정 및 데이터 구조 정리  

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

레포 안의 **`gitclone_preprocessing_data.ipynb`** 를 실행하면:
NTU RGB+D 의 3D 골격 전처리된 데이터를 다운로드한 후 압축 해제합니다.

---

### ✔ 2. 12개 클래스 데이터 생성

- 다운한 NTU RGB+D skeleton 데이터 전체 60개 액션 중 **이 프로젝트에서 사용하는 12개 액션만 선택**
- 학습/검증/테스트 용 **6개의 데이터 파일**이 자동 생성됩니다.

---

### ✔ 3. 생성된 6개 파일의 저장 위치

Notebook 실행 후 데이터는 아래 경로에 저장됩니다:

``` 
<repo_root>/data/NTU-RGB-D/x-view/
    ├── small_train_data.npy
    ├── small_train_label.pkl
    ├── small_val_data.npy
    └── small_val_label.pkl
    ├── small_test_data.npy
    └── small_test_label.pkl
```


각 파일은 다음 의미를 가집니다:

| 파일명 | 설명 |
|-------|------|
| `small_train_data.npy` | 학습용 skeleton 시퀀스 |
| `small_train_label.pkl` | 학습용 라벨 |
| `small_val_data.npy` | 검증용 skeleton 시퀀스 |
| `small_val_label.pkl` | 검증용 라벨 |
| `small_test_data.npy` | 테스트용 skeleton 시퀀스 |
| `small_test_label.pkl` | 테스트용 라벨 |

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

