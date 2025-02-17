# 🏆 Cell Count Model

## 📌 프로젝트 개요
🎯 **Cell Detection and Verification Model** 프로젝트는 실험실에서 **세포 수를 자동으로 카운팅**하는 것을 목표로 합니다. OpenCV 기반의 **객체 탐지**와 **CNN(ResNet18) 기반의 분류 모델**을 결합하여 높은 정확도를 가진 자동화 시스템을 개발하였습니다. 실험 결과, **99%의 정확도**를 기록하며, 연구자들의 실험 생산성을 높일 가능성을 보여주었습니다.

---

## 🔍 1. 연구 배경 및 필요성
- 기존의 **헤모사이토미터를 이용한 수작업 세포 카운팅**은 시간 소모가 크고, 연구자마다 결과가 다를 수 있음.
- **세포 카운팅 자동화 시스템**을 개발하여 **일관된 결과**와 **높은 정확도**를 보장하는 것이 필요.
- Faster-RCNN을 활용한 시도는 **객체 수가 너무 많아 탐지가 어려운 문제**가 있어 새로운 접근법이 필요.

---

## ⚙️ 2. 모델 아키텍처
본 프로젝트는 **2단계의 탐지 및 검증 과정**을 포함합니다.

1️⃣ **세포 후보 영역 탐지 (Object Detection)**
   - **OpenCV의 `findContour` 함수**를 활용하여 세포가 있을 가능성이 높은 영역을 탐지
   - 탐지된 객체들을 바운딩 박스로 저장하여 후처리 수행

2️⃣ **세포 여부 판별 (Verification)**
   - **CNN 기반 분류 모델(ResNet18)**을 사용하여 탐지된 객체가 실제 세포인지 여부를 판별
   - 이진 분류 (Alive vs Misclassified) 수행

---

## 🏗️ 3. 데이터 처리
📌 **데이터 수집 및 전처리**
- 실험실에서 촬영한 **현미경 이미지를 기반으로 데이터셋 구성**
- OpenCV의 **객체 탐지 결과를 활용하여 세포 후보 영역을 정의**

📌 **데이터 증강 (Data Augmentation)**
- **좌우 반전, 랜덤 회전, 밝기 및 대비 조정, 위치 변환** 등의 기법을 활용하여 데이터 다양성 확보
- **원본 데이터의 10배 규모로 데이터셋 확장**

📌 **라벨링 및 클래스 구성**
- `Alive`: 실제 세포가 존재하는 영역
- `Misclassified`: 세포가 아닌 노이즈 영역

---

## 🎯 4. 모델 학습
📌 **CNN 모델 구성**
- **ResNet18 기반의 분류 모델** 사용
- ImageNet으로 사전 학습된 모델을 활용하여 **전이 학습 (Transfer Learning) 적용**
- 마지막 Fully Connected Layer를 **Alive / Misclassified 2개 클래스로 수정**

📌 **학습 파라미터**
- **손실 함수:** CrossEntropyLoss
- **최적화 알고리즘:** Adam Optimizer (학습률 0.001)
- **배치 크기:** 32
- **하드웨어:** GPU (CUDA) 사용

📌 **학습 과정**
1. **Forward Pass**: 입력 이미지를 모델에 전달하여 예측값 계산
2. **Backward Pass**: 손실 함수 기반으로 가중치 업데이트
3. **정확도 계산**: 학습 및 검증 데이터에 대한 성능 모니터링

---

## 📊 5. 성능 평가
📌 **테스트 결과**
| 메트릭 | Alive 클래스 | Misclassified 클래스 | 전체 평균 |
|--------|--------------|----------------------|------------|
| 정확도 (Accuracy) | 99% | 97% | 99% |
| 정밀도 (Precision) | 1.00 | 0.97 | 0.98 |
| 재현율 (Recall) | 1.00 | 0.97 | 0.98 |
| F1-score | 1.00 | 0.97 | 0.98 |

📌 **한계점**
- 훈련 데이터에서 증강된 데이터로 검증했기 때문에 **실제 일반화 성능 평가 부족**
- 다양한 환경에서의 **조도 변화 및 해상도 차이**에 대한 검증 미흡
- OpenCV 기반 탐지에서 **노이즈를 세포로 탐지하는 경우 발생**

---


## 📚 참고 자료
- 🎥 관련 논문 및 연구 자료 업데이트 예정

---

## 📝 라이선스
📌 **MIT License**

