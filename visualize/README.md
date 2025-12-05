## 🎥 3D Skeleton 시각화 (npy2video.ipynb)

이 시각화 예제는 NTU RGB+D 스켈레톤 시퀀스를 프레임 단위로 렌더링하여  
**애니메이션 영상(mp4)** 으로 변환하는 과정을 보여줍니다.  
모든 시각화 로직은 **`npy2video.ipynb`**에서 Python + OpenCV 기반으로 구현되었습니다.

아래는 테스트 데이터셋에서 action label **001**을 선택하여 생성한 예시 영상입니다:

🔗 **샘플 영상:**  
<video src="https://raw.githubusercontent.com/UsernameisKoo/Skeleton_train/main/visualize/A001_video.mp4"
       width="600"
       controls>
</video>


시각화에는 다음 요소들이 포함됩니다:
- 기능별 관절 그룹(머리, 몸통, 팔, 손, 다리, 발)에 다른 색상 적용  
- NTU RGB+D 포맷에 따른 본(bone) 연결 구조 표현  
- 각 프레임에 action label 및 frame index 표시  
- 관절 좌표 정규화를 통한 안정적인 화면 배치  

**참고 사항:**
- 시각화에 사용된 샘플은 **`small_test_data.npy`** (테스트 데이터셋)에서 선택되었습니다.
- 타겟 action label을 지정하면 해당 라벨의 첫 번째 샘플을 자동으로 선택합니다.
- 생성된 결과물에는 `.mp4`(H.264 인코딩 영상)와, 시각화에 사용된 스켈레톤 샘플 `.npy` 파일이 포함됩니다.

전체 시각화 및 영상 생성 과정은 **`npy2video.ipynb`**에서 확인할 수 있습니다.
