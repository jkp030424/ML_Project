# 데이터셋 및 라이선스 안내

## 원본 데이터셋

본 프로젝트는 VITON-HD 데이터셋의 착용 이미지 일부를 수동 라벨링하여 사용합니다.

- 원본 데이터셋: VITON-HD
- 원 논문: VITON-HD: High-Resolution Virtual Try-On via Misalignment-Aware Normalization
- 용도: 학술 과제 및 연구 목적의 fit label classification baseline
- 라이선스: 공식 GitHub 기준 Creative Commons BY-NC 4.0. 연구 목적 및 비상업 사용 조건으로 안내됩니다.

## 공개 저장소 업로드 주의사항

본 과제 제출용 GitHub 저장소에는 재현을 위해 아래 항목을 포함합니다.

```text
dataset/
embeddings/
models/experiment_7/
results/
```

각 항목의 의미는 다음과 같습니다.

- `dataset/`: VITON-HD 이미지 일부를 수동 라벨링한 소규모 subset
- `embeddings/`: 사전학습 인코더로 추출한 이미지 임베딩
- `models/experiment_7/`: 최종 선택 모델 artifact
- `results/`: 실험 결과 요약 및 평가 결과

단, 아래 파일들은 원본 이미지가 직접 복사되거나 포함될 수 있으므로 공개 저장소에는 포함하지 않는 것을 권장합니다.

```text
results/*/misclassified_images/
원본 이미지가 포함된 시각화 결과
```

`dataset/`, `embeddings/`, `models/experiment_7/`는 모두 VITON-HD 이미지 또는 그 파생 artifact입니다. 본 과제에서는 재현 편의를 위해 포함하지만, 이 파일들 역시 VITON-HD의 CC BY-NC 4.0 조건을 따르며 코드 라이선스에는 포함되지 않습니다.

## 코드 라이선스

본 프로젝트의 코드에는 MIT License를 적용할 수 있습니다. 단, 이 라이선스는 코드에만 적용되며 VITON-HD 원본 이미지, `dataset/`의 라벨링 이미지셋, `embeddings/`의 파생 feature, `models/experiment_7/`의 학습된 모델 artifact에는 자동으로 적용되지 않습니다.

## 재현 방법

본 과제 저장소에는 바로 실행 가능한 소규모 subset이 포함됩니다. 더 큰 규모로 재현하려는 사용자는 VITON-HD 데이터셋을 원 출처에서 직접 받은 뒤, README의 폴더 구조에 맞게 데이터를 배치해야 합니다.

## 참고 링크

- VITON-HD paper: https://openaccess.thecvf.com/content/CVPR2021/html/Choi_VITON-HD_High-Resolution_Virtual_Try-On_via_Misalignment-Aware_Normalization_CVPR_2021_paper.html
- VITON-HD official code: https://github.com/shadow2496/VITON-HD
