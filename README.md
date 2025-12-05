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
### **3. npy2video.ipynb**
NTU RGB+D skeleton 시퀀스를 **프레임 기반 시각화 후 고품질 영상(mp4)으로 변환**하는 기능을 구현했습니다.  
데이터 검증·시각적 분석·데모 제작에 활용할 수 있습니다.
**시각화에 사용된 샘플은 테스트 데이터세트(small_test_data.npy)에서 선택되었습니다.**

#### 🔹 주요 구현 내용

- 특정 Action Label 기반 샘플 자동 선택
  - `action_ids` 배열에서 target label에 해당하는 첫 번째 skeleton 샘플을 로드하도록 구현
  - 사용자가 원하는 액션 클래스의 움직임을 빠르게 확인 가능

- Skeleton 좌표 정규화 및 화면 표시
  - ST-GCN 형식 `(3, T, 25, 2)`의 데이터를 불러와 25개 joint 좌표를 정규화
  - 영상 캔버스(size=900)에 균일하게 매핑되도록 scaling 적용
  - Y-좌표 반전으로 실제 화면 좌표계에 맞게 렌더링

- **기능별 Joint Group 색상 시각화**
  - head / body / arms / hands / legs / feet로 그룹화하여 색상 구분
  - NTU RGB+D bone 연결 구조(ntu_pairs)를 기반으로 관절-본 구조를 선으로 연결
  - joint index 라벨을 표시하여 디버깅 및 분석 용이

- **프레임 단위 렌더링 및 영상 생성**
  - OpenCV `cv2.VideoWriter`로 `.avi` 파일 생성
  - grid, skeleton, 라벨 텍스트(액션 라벨, 프레임 번호)를 매 프레임에 렌더링

- **고품질 MP4(H.264) 변환**
  - ffmpeg를 사용해 AVI → H.264 MP4로 변환 (`libx264`, `crf=18`, `yuv420p`)
  - 모든 브라우저와 미디어 뷰어에서 호환되는 최적 형태로 저장

- **원본 Skeleton Sample 저장**
  - 렌더링에 사용된 sample을 `Axxx_video.npy` 로 함께 저장하여 재현성 확보

#### 📁 생성되는 결과물
시각화 실행 후 다음 파일들이 생성됩니다:
``` 
Axxx_video.avi # 임시 생성 영상
Axxx_video.mp4 # H.264 변환된 최종 출력 영상
Axxx_video.npy # 시각화에 사용된 skeleton 데이터 샘플
```

#### 💡 활용 목적
- skeleton 데이터의 **움직임 패턴 검증**
- ST-GCN 입력 데이터 품질 확인 및 디버깅
- 행동 클래스별 skeleton motion 분석
- 프로젝트 발표·데모 영상 생성
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

