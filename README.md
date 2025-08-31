# dacon_car
# 🚗 차량 분류 모델 프로젝트
**DACON HAI! – Hecho AI Challenge (2025.06)**
서동현 · 유동현 · 김준희
<img src="https://github.com/user-attachments/assets/1de54dfb-b7fb-4027-9b9d-f06606b54245" alt="딥러닝발표자료" width="500">\

---
## 🗂 발표 자료
- [📂 발표자료 PDF](https://github.com/tjehdgus/dacon_car/blob/main/%EC%B0%A8%EB%9F%89.pdf)
- 프로젝트의 자세한 내용은 발표자료에서 확인할 수 있습니다:
# 📖 프로젝트 개요
자동차 산업의 디지털 전환이 가속화되면서 **차종을 빠르고 정확하게 분류하는 기술**은 중고차 거래, 차량 관리, 자동 주차 및 보안 시스템에서 핵심 경쟁력으로 자리 잡고 있습니다.
본 프로젝트는 **대규모 차량 이미지 데이터셋을 활용하여 딥러닝 기반 차량 분류 모델을 개발**하고, 다양한 데이터 전처리·증강 기법과 최신 모델을 실험해 최적의 성능을 도출하는 것을 목표로 했습니다.

---

## 📂 데이터
- **Train**: 396개 클래스, 33,137장 → 전처리 후 33,074장
- **Test**: 8,258장

**데이터 전처리**
- 노이즈 이미지 제거 (훼손, 트렁크 열림, 내부 사진 등)
- 분류 불가능 이미지 제외

**EDA 결과**
- 이미지 크기: 대부분 500–900px(너비), 400–600px(높이)
- 클래스별 분포: 55~88장 (대부분 클래스가 80장 이상)

---

## 🛠️ 데이터 증강 기법
- **AutoAugment / RandomAugment**: 강화학습 기반/랜덤 정책
- **Mixup / CutMix**: 데이터 혼합 기법 (α=0.3~0.5)
- **RandomShadow, SunFlare**: 조명 조건 다양화
- **RandomErasing**: 일부 영역 마스킹 → 일반화 성능 향상
- (Mosaic은 분류 태스크에 부적합하여 제외)

---

## 🧠 모델링
### 실험 모델
- Swin Transformer (LB: 0.2259)
- EVA-02 (LB: 0.2002)
- DeiT v3 (LB: 0.1799)
- BEiT v2 (LB: 0.2166)
- **ConvNeXt v2 (최종 선정, LB: 0.1718)**

### 최종 모델 (ConvNeXt v2)
- Batch size: 64
- Epochs: 50
- Image size: 384/512
- Learning rate: 5e-5
- Weight decay: 0.05
- Label smoothing: 0.05
- Mixup & CutMix 적용
- EMA & SWA 적용
- Early stopping patience: 10

---

## 🔧 추가 기법
- **EMA (Exponential Moving Average)** → 안정적 성능 향상
- **SWA (Stochastic Weight Averaging)** → 일반화 성능 개선
- **Warmup + CosineAnnealing / ReduceLROnPlateau** → 학습률 스케줄링
- **Temperature Scaling** → 예측 확률 보정
- **Curriculum Learning** → 난이도 기반 학습 전략

---

## 📊 최종 결과
- **Public LB**: 0.11653 (LogLoss)
- **Private LB**: 0.12452 (LogLoss) → **36위**

---

## 🌐 향후 활용 가능성
- **주차 차량 자동 분류 시스템**: CCTV 영상 기반 차량 라벨링
- **중고차 거래 플랫폼**: 연식/차종 오기재 방지, 신뢰성 강화
- **자동 보안·관리 시스템**: 실시간 차종 식별
