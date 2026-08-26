# Nightingale Whistle Matching Lab

Current Biology 2026 논문(whistle matching in wild nightingales)의 핵심 아이디어를
실시간 오디오 입력으로 시각화하는 단일 페이지 프로젝트입니다.
사용자가 업로드한 새 울음(또는 마이크 입력)의 피치와 지속시간을 추적하고,
논문에서 제시한 “Duration → Pitch” 계층 모델이 만들어내는 반응을 함께 보여줍니다.

## 핵심 개념
- 휘슬 지속시간은 SHORT/MIDDLE/LONG(0.14 s, 0.31 s 경계)으로 군집됩니다.
- 비정상 조합 자극에서는 피치와 지속시간 중 하나만 강하게 모방합니다.
- 모델은 지속시간이 먼저 반응 공간을 제한하고, 피치가 뒤따릅니다.

## 구성 요소
- Soundspace 캔버스: Duration (x) × Pitch (y) 공간에 C1/C2/C3 클러스터, 라이브/타겟/모델 지점 표시
- Duration GMM / Pitch GMM: 논문 기반 파라미터의 가중치 변화를 실시간으로 표시
- Waveform + Gate: syllable 길이 추정(에너지 게이트 기반)
- Similarity 패널: Target 대비 Duration/Pitch 유사도와 편향(Bias)

## 실행 방법
로컬 파일 업로드는 `index.html`을 직접 열어도 동작합니다.
마이크 입력은 브라우저 보안 정책상 `http://localhost` 또는 HTTPS에서만 활성화됩니다.

```bash
python -m http.server
```

이후 `http://localhost:8000`에서 열면 마이크 사용 가능.

## 사용 방법
1. 입력 모드 선택: 마이크 또는 파일
2. Playback 목표 설정: 논문에서 사용한 3가지 조합 또는 Custom
3. Hierarchy 선택: Duration → Pitch (논문 핵심 가설) 또는 Pitch → Duration
4. 재생/입력 시작 후 Soundspace 및 GMM 변화 확인

## 컨트롤
- FFT: 피치 추정 해상도
- EMA: 피치/지속시간 부드럽게 추정하는 지수평활
- Gate: 음성 구간 감지 임계값
- Duration Max / Pitch Max: 시각화 범위 상한
- α / β / σ: 논문 모델의 가중치 업데이트 강도

## 참고
- 피치 추정은 FFT 피크 기반 단순 추정이며, 휘슬 계열에 최적화되어 있습니다.
- Google Fonts를 사용하므로 폰트 로딩에는 네트워크가 필요합니다(오프라인에서도 기본 폰트로 동작).

## 파일 구조
- `index.html`: UI + 시각화 + 오디오 분석 로직 전체
