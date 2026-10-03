# Learning Brain Activity from Public EEG Data

Standalone base document for a new project. Cost: $0. No hardware, no implants, no purchases.

## Scope

**In:** use public, already-recorded EEG datasets to learn how brain signals look and how to classify mental states with Python.
**Out:** buying hardware, recording your own data, real-time systems, phone apps, any invasive work.

**Background goal:** test whether differences in imagined mental activity (including imagined vowels such as "A" vs "I") can be detected from brain signals. Start with the best-documented task, imagined hand movement, then try imagined speech data as a stretch.

## Setup (one evening)

- Python 3.10+ in a virtual environment, or free Google Colab.
- Install `mne`, `scikit-learn`, `numpy`, `matplotlib`; later `torch` and `moabb`.
- Keep one folder per dataset and one notebook per experiment.

## Public data to use

- **PhysioNet EEG Motor Movement/Imagery:** 109 subjects, 64 channels. Best first dataset. MNE downloads it for you (`mne.datasets.eegbci`).
- **BCI Competition IV (2a, 2b):** classic motor-imagery benchmark, good for comparing with published results.
- **MOABB:** Python library that loads many BCI datasets with one interface and fair evaluation.
- **OpenNeuro:** large open repository of EEG and other neuroimaging datasets.
- **Imagined-speech EEG sets:** public datasets exist (for example Kara One). Check each one's license, number of subjects and tasks before use.

## Four-week workflow

1. **Week 1:** load the data, plot raw EEG, mark blinks and muscle noise, understand channels, sampling rate and events.
2. **Week 2:** band-pass filter (8-30 Hz for motor imagery), cut epochs around events, view average band power.
3. **Week 3:** features (band power, CSP) plus a simple classifier (LDA). Cross-validate and report accuracy per subject.
4. **Week 4:** try a small neural net (EEGNet) and compare with the simple model. Write up what worked and what didn't.

## Starter code (untested; run it on your machine)

```python
import mne
from mne.datasets import eegbci
from mne.decoding import CSP
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis as LDA
from sklearn.model_selection import cross_val_score
from sklearn.pipeline import make_pipeline

runs = [4, 8, 12]  # imagined left vs right fist
raws = [mne.io.read_raw_edf(f, preload=True) for f in eegbci.load_data(1, runs)]
raw = mne.concatenate_raws(raws)
eegbci.standardize(raw)
raw.set_montage("standard_1005")
raw.filter(8, 30)

events, _ = mne.events_from_annotations(raw, event_id=dict(T1=2, T2=3))
epochs = mne.Epochs(raw, events, dict(left=2, right=3),
                    tmin=0.5, tmax=3.5, baseline=None, preload=True)
X, y = epochs.get_data(), epochs.events[:, 2]

clf = make_pipeline(CSP(n_components=4), LDA())
print(cross_val_score(clf, X, y, cv=5).mean())
```

## Things to expect

- Chance level for two classes is 50%. Getting 65-80% on good subjects is a normal success; some subjects stay near chance.
- Split train and test by session or subject, not randomly by trial, or accuracy will look falsely high.
- Imagined vowels from scalp EEG are a hard, open research problem. Expect small gains above chance at best, and treat any impressive result with suspicion until it survives proper cross-validation.

## Done when

- You can load a dataset, filter it, epoch it and classify two mental states above chance with honest cross-validation.
- You can explain why accuracy varies between subjects.
- You have a notebook someone else could run to reproduce your numbers.

---

# 공개 EEG 데이터로 뇌 활동 배우기

새 프로젝트를 위한 독립 기반 문서. 비용: $0. 하드웨어, 임플란트, 구매 모두 없음.

## 범위

**포함:** 이미 녹음된 공개 EEG 데이터셋으로 뇌 신호가 어떻게 생겼는지, Python으로 정신 상태를 어떻게 분류하는지 배우기.
**제외:** 하드웨어 구매, 직접 데이터 기록, 실시간 시스템, 폰 앱, 모든 침습적 작업.

**배경 목표:** 상상한 정신 활동의 차이(예: 상상한 모음 "A"와 "I")를 뇌 신호에서 감지할 수 있는지 시험하기. 가장 잘 정리된 과제인 손 움직임 상상으로 먼저 시작하고, 확장 과제로 상상 발화 데이터를 다룹니다.

## 준비 (저녁 한 번)

- 가상환경의 Python 3.10 이상, 또는 무료 Google Colab.
- `mne`, `scikit-learn`, `numpy`, `matplotlib` 설치; 나중에 `torch`와 `moabb`.
- 데이터셋마다 폴더 하나, 실험마다 노트북 하나.

## 사용할 공개 데이터

- **PhysioNet EEG Motor Movement/Imagery:** 피험자 109명, 64채널. 첫 데이터셋으로 최고. MNE가 자동 다운로드합니다(`mne.datasets.eegbci`).
- **BCI Competition IV (2a, 2b):** 운동상상의 고전 벤치마크. 발표된 결과와 비교하기 좋습니다.
- **MOABB:** 여러 BCI 데이터셋을 하나의 인터페이스로 불러오고 공정하게 평가하는 Python 라이브러리.
- **OpenNeuro:** EEG 및 기타 신경영상 데이터가 모인 대규모 공개 저장소.
- **상상 발화 EEG 데이터:** 공개 데이터셋이 있습니다(예: Kara One). 사용 전에 라이선스, 피험자 수, 과제를 확인하세요.

## 4주 작업 흐름

1. **1주차:** 데이터를 불러와 원시 EEG를 그리고, 눈 깜빡임과 근육 노이즈를 표시하며, 채널, 샘플링 속도, 이벤트를 이해합니다.
2. **2주차:** 대역통과 필터(운동상상은 8~30Hz), 이벤트 주변 에포크 분할, 평균 대역 파워 확인.
3. **3주차:** 특징(대역 파워, CSP) + 간단한 분류기(LDA). 교차검증 후 피험자별 정확도를 기록합니다.
4. **4주차:** 작은 신경망(EEGNet)을 시도하고 간단한 모델과 비교합니다. 잘된 점과 안 된 점을 정리합니다.

## 시작 코드 (테스트하지 않음, 본인 컴퓨터에서 실행)

위 영문 섹션의 "Starter code"와 동일한 코드를 사용하세요. 주석만 번역하면 다음과 같습니다: `runs = [4, 8, 12]`는 왼손 vs 오른손 주먹 상상 구간입니다.

## 예상해야 할 것

- 2개 클래스의 우연 수준은 50%입니다. 좋은 피험자에서 65~80%면 정상적인 성공이며, 일부 피험자는 우연 수준에 머뭅니다.
- 훈련/테스트는 시행(trial)별 무작위가 아니라 세션이나 피험자 단위로 나누세요. 그렇지 않으면 정확도가 거짓으로 높게 나옵니다.
- 두피 EEG로 상상한 모음을 구분하는 것은 어렵고 아직 열린 연구 문제입니다. 잘해야 우연 수준을 조금 넘는 정도로 예상하고, 인상적인 결과는 제대로 된 교차검증을 통과하기 전까지 의심하세요.

## 완료 기준

- 데이터셋을 불러와 필터링, 에포크 분할, 두 가지 정신 상태 분류를 정직한 교차검증으로 우연 수준 이상 해냅니다.
- 피험자마다 정확도가 다른 이유를 설명할 수 있습니다.
- 다른 사람이 숫자를 재현할 수 있는 노트북이 있습니다.
