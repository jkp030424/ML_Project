# 데이터셋 및 라이선스 안내

## 원본 데이터셋

본 프로젝트는 VITON-HD 데이터셋의 착용 이미지 일부를 수동 라벨링하여 사용합니다.

- 원본 데이터셋: VITON-HD
- 원 논문: VITON-HD: High-Resolution Virtual Try-On via Misalignment-Aware Normalization
- 용도: 학술 과제 및 연구 목적의 fit label classification baseline
- 라이선스: 공식 GitHub 기준 Creative Commons BY-NC 4.0. 연구 목적 및 비상업 사용 조건으로 안내됩니다.

## 공개 저장소 업로드 주의사항

본 과제 제출용 GitHub 저장소에는 재현을 위해 `dataset/` 폴더의 소규모 수동 라벨링 subset을 포함합니다. 단, 아래 파생 파일들은 포함하지 않는 것을 권장합니다.

```text
embeddings/
models/
results/*/misclassified_images/
원본 이미지가 포함된 시각화 결과
```

이유는 다음과 같습니다.

1. `embeddings/`는 원본 이미지로부터 직접 생성된 파생 feature입니다.
2. `models/`는 해당 데이터로 학습된 파생 artifact입니다.
3. `misclassified_images/`와 일부 grid 이미지는 원본 이미지를 복사하거나 포함합니다.

따라서 공개 저장소에는 코드, 설정 파일, `dataset/`의 소규모 수동 라벨링 subset, 그리고 원본 이미지를 포함하지 않는 결과 요약 파일만 포함하는 것을 권장합니다.

## 코드 라이선스

본 프로젝트의 코드에는 MIT License를 적용할 수 있습니다. 단, 이 라이선스는 코드에만 적용되며 VITON-HD 원본 이미지, 라벨링 이미지셋, 임베딩 파일, 학습된 모델 weight에는 자동으로 적용되지 않습니다.

## 재현 방법

본 과제 저장소에는 바로 실행 가능한 소규모 subset이 포함됩니다. 더 큰 규모로 재현하려는 사용자는 VITON-HD 데이터셋을 원 출처에서 직접 받은 뒤, README의 폴더 구조에 맞게 데이터를 배치해야 합니다.

## 참고 링크

- VITON-HD paper: https://openaccess.thecvf.com/content/CVPR2021/html/Choi_VITON-HD_High-Resolution_Virtual_Try-On_via_Misalignment-Aware_Normalization_CVPR_2021_paper.html
- VITON-HD official code: https://github.com/shadow2496/VITON-HD
