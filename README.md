# Fit Label Classification for VTON Images

VITON-HD 착용 이미지에서 상의의 핏을 `tight`, `regular`, `oversized` 3개 클래스로 분류하는 실험 코드입니다. 사전학습 이미지 인코더로 임베딩을 추출한 뒤, 전통적인 머신러닝 분류기를 학습하여 소규모 수동 라벨링 데이터에서도 핏 라벨 분류가 가능한지 확인합니다.

## 1. 프로젝트 개요

온라인 의류 구매와 가상피팅에서는 실제 착용 시의 핏 정보가 중요하지만, VTON 계열 데이터셋에는 의류 치수나 착용 핏 라벨이 포함되지 않는 경우가 많습니다. 본 프로젝트는 기존 VTON 이미지에 `tight / regular / oversized` 핏 라벨을 부여할 수 있는 baseline 분류 모델을 구축하는 것을 목표로 합니다.

최종 실험에서는 VITON-HD 이미지 중 수동 라벨링한 450장을 사용했습니다.

| 클래스 | 이미지 수 |
| --- | ---: |
| tight | 151 |
| regular | 150 |
| oversized | 149 |
| 총합 | 450 |

최종 모델은 `OpenAI CLIP ViT-B/32` 임베딩과 `Linear SVM(C=15)` 조합입니다.

## 2. 주요 결과

최종 모델은 train set 전체로 재학습한 뒤 held-out test set에서 1회 평가했습니다.

| 지표 | 값 |
| --- | ---: |
| Test Accuracy | 0.926 |
| Test Macro F1 | 0.927 |
| Test Ordinal MAE | 0.074 |

KNN baseline 대비 성능은 다음과 같습니다.

| 모델 | Accuracy | Macro F1 | Ordinal MAE |
| --- | ---: | ---: | ---: |
| KNN baseline | 0.750 | 0.749 | 0.250 |
| CLIP ViT-B/32 + Linear SVM(C=15) | 0.926 | 0.927 | 0.074 |

## 3. Repository Structure

권장 저장소 구조는 아래와 같습니다.

```text
.
├── README.md
├── requirements.txt
├── environment.yaml
├── fit_label_experiment_colab.ipynb
├── dataset/
│   ├── tight/
│   ├── regular/
│   └── oversized/
├── embeddings/
│   ├── clip_vit_b32/
│   ├── openclip_vit_l14/
│   └── dinov2_vit_b14/
├── models/
│   └── experiment_*/
└── results/
    └── experiment_*/
```

본 과제 제출용 저장소에는 바로 실행 가능한 재현을 위해 수동 라벨링한 소규모 `dataset/` subset을 포함합니다. 단, `embeddings/`, `models/`, 원본 이미지가 복사된 분석 결과 폴더는 제외하는 것을 권장합니다. 자세한 이유는 [데이터셋 및 라이선스](#8-데이터셋-및-라이선스)를 참고하세요.

## 4. Installation

본 실험은 Google Colab 환경을 기준으로 작성되었습니다.

### pip 사용

```bash
pip install -r requirements.txt
pip install git+https://github.com/openai/CLIP.git
```

### conda 사용

```bash
conda env create -f environment.yaml
conda activate fit-label-classification
```

Colab에서는 notebook 첫 셀에서 필요한 패키지를 설치하도록 구성할 수 있습니다.

## 5. Data Preparation

Google Drive 또는 로컬 프로젝트 폴더에 아래와 같은 형태로 데이터를 배치합니다.

```text
ML/
├── dataset/
│   ├── tight/
│   ├── regular/
│   └── oversized/
├── embeddings/
├── models/
├── results/
└── fit_label_experiment_colab.ipynb
```

각 라벨 폴더에는 해당 핏으로 수동 라벨링한 VITON-HD 착용 이미지를 넣습니다.

```text
dataset/tight/*.jpg
dataset/regular/*.jpg
dataset/oversized/*.jpg
```

실험에서 사용한 라벨링 기준은 다음과 같습니다.

| 핏 유형 | 어깨선 | 몸통 | 소매 | 윤곽 |
| --- | --- | --- | --- | --- |
| tight | 딱 맞음 | 딱 붙음 | 팔에 붙음 | 뚜렷함 |
| regular | 어깨와 거의 맞음 | 적당한 여유 | 적당히 널널함 | 일부만 보임 |
| oversized | 어깨보다 아래 | 몸보다 크게 넓음 | 넓거나 처짐 | 거의 안 보임 |

## 6. Reproduction

### 6.1 기본 경로 수정

`fit_label_experiment_colab.ipynb`의 Config 셀에서 프로젝트 경로를 수정합니다.

```python
BASE_DIR = Path("/content/drive/MyDrive/ML")
DATASET_DIR = BASE_DIR / "dataset"
EMBED_DIR = BASE_DIR / "embeddings"
MODEL_ROOT_DIR = BASE_DIR / "models"
RESULT_ROOT_DIR = BASE_DIR / "results"
```

### 6.2 실험 실행

notebook을 위에서부터 실행합니다.

```text
1. Install Packages
2. Import Libraries
3. Mount Google Drive
4. Config
5. Device
6. Load and Validate Dataset
7. Stratified Train / Test Split
8. Feature Extractor Loading Functions
9. Extract and Save Embeddings
10. Classifier Definitions
11. Metrics
12. Stratified 5-Fold Cross Validation on Train Set
13. Select Best Model
```

임베딩은 한 번 생성한 뒤 재사용할 수 있습니다.

```python
FORCE_REBUILD_EMBEDDINGS = False
```

### 6.3 최종 모델 평가

본 프로젝트의 최종 선택 모델은 아래 설정입니다.

```python
FINAL_EXPERIMENT_NAME = "experiment_7"
FINAL_ENCODER = "clip_vit_b32"
FINAL_CLASSIFIER = "linear_svm"
```

최종 모델은 train set 전체로 재학습한 뒤 test set에서 한 번만 평가합니다.

```text
14. 최종 모델 선택
15. 최종 Test 평가
16. Confusion Matrix
17. Test 예측 결과 저장
18. 오분류 이미지 저장
19. 오분류 이미지 Grid 시각화
20. 오분류 요약
```

## 7. Experiments

총 3개 인코더와 6개 분류기를 비교했습니다.

### Encoders

| 인코더 | 특징 | 임베딩 차원 |
| --- | --- | ---: |
| OpenAI CLIP ViT-B/32 | baseline encoder | 512 |
| OpenCLIP ViT-L/14 | 더 큰 CLIP 계열 encoder | 768 |
| DINOv2 ViT-B/14 | self-supervised visual encoder | 768 |

### Classifiers

| 모델 | 주요 설정 |
| --- | --- |
| KNN baseline | `k=5`, `weights=distance`, `metric=cosine` |
| Logistic Regression | `C=1.0`, `penalty=l2`, `solver=lbfgs` |
| Linear SVM | 최종 `C=15`, `CalibratedClassifierCV(cv=3)` |
| RBF SVM | `gamma=scale`, C 탐색 |
| Random Forest | `n_estimators=500`, depth/leaf 규제 |
| XGBoost | `max_depth`, `learning_rate`, `reg_lambda` 조정 |

모델 선정은 train set 내부의 stratified 5-fold cross validation 결과를 기준으로 수행했습니다. Test set은 최종 모델 선정 이후 최종 성능 확인에만 사용했습니다.

## 8. 데이터셋 및 라이선스

### VITON-HD

본 프로젝트의 원본 이미지는 VITON-HD 데이터셋에서 일부를 수동 라벨링하여 사용했습니다. VITON-HD는 가상피팅 연구를 위해 공개된 데이터셋이며, 원 논문과 공식 저장소를 반드시 인용해야 합니다. 공식 GitHub 기준 VITON-HD는 연구 목적 사용을 전제로 하며, Creative Commons BY-NC 4.0 라이선스로 제공됩니다.

중요: 본 저장소의 `dataset/`은 VITON-HD 원본 이미지에서 파생된 소규모 수동 라벨링 subset입니다. 과제 재현을 위한 비상업적 학술 목적에 한해 포함하며, 이미지 사용 조건은 원본 VITON-HD의 CC BY-NC 4.0 조건을 따릅니다. 공개 배포 범위가 수업 제출을 넘어서는 경우에는 원 데이터셋의 사용 조건을 다시 확인해야 합니다.

권장 방식:

```text
GitHub에 포함:
- notebook/code
- README
- requirements.txt 또는 environment.yaml
- dataset/ 안의 소규모 수동 라벨링 subset
- 결과 요약 CSV/그림 중 원본 이미지가 포함되지 않는 파일

GitHub에서 제외:
- embeddings/
- models/
- misclassified_images/
- 원본 이미지가 포함된 figure/grid
```

### 수동 라벨

`tight / regular / oversized` 라벨은 본 프로젝트에서 1인 라벨러가 직접 부여했습니다. 라벨 기준과 실험 결과는 재현 가능하도록 README와 notebook에 기록했습니다.

### 코드 라이선스

본 저장소의 실험 코드에는 MIT License를 적용하는 것을 권장합니다. 단, 이 라이선스는 코드에만 적용되며 VITON-HD 원본 이미지와 그 파생 데이터에는 적용되지 않습니다.

## 9. Citation

본 프로젝트를 사용할 경우 아래 논문을 함께 인용하세요.

```bibtex
@inproceedings{choi2021vitonhd,
  title={VITON-HD: High-Resolution Virtual Try-On via Misalignment-Aware Normalization},
  author={Choi, Seunghwan and Park, Sunghyun and Lee, Minsoo and Choo, Jaegul},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  year={2021}
}

@inproceedings{radford2021clip,
  title={Learning Transferable Visual Models From Natural Language Supervision},
  author={Radford, Alec and Kim, Jong Wook and Hallacy, Chris and Ramesh, Aditya and Goh, Gabriel and Agarwal, Sandhini and Sastry, Girish and Askell, Amanda and Mishkin, Pamela and Clark, Jack and Krueger, Gretchen and Sutskever, Ilya},
  booktitle={International Conference on Machine Learning},
  year={2021}
}

@inproceedings{cherti2023reproducible,
  title={Reproducible Scaling Laws for Contrastive Language-Image Learning},
  author={Cherti, Mehdi and Beaumont, Romain and Wightman, Ross and Wortsman, Mitchell and Ilharco, Gabriel and Gordon, Cade and Schuhmann, Christoph and Schmidt, Ludwig and Jitsev, Jenia},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  year={2023}
}

@article{oquab2023dinov2,
  title={DINOv2: Learning Robust Visual Features without Supervision},
  author={Oquab, Maxime and Darcet, Timothée and Moutakanni, Theo and others},
  journal={Transactions on Machine Learning Research},
  year={2024}
}
```

## 10. Notes

- 본 프로젝트는 학술 과제 및 연구 목적의 baseline 실험입니다.
- 최종 성능은 1인 수동 라벨링 데이터셋에 대한 결과이며, 라벨러 편향이 존재할 수 있습니다.
- 실제 서비스 적용 전에는 다수 라벨러 검증, 데이터 확장, 부위별 fit label 확장 등이 필요합니다.

## 11. External Links

- VITON-HD paper: https://openaccess.thecvf.com/content/CVPR2021/html/Choi_VITON-HD_High-Resolution_Virtual_Try-On_via_Misalignment-Aware_Normalization_CVPR_2021_paper.html
- VITON-HD official code: https://github.com/shadow2496/VITON-HD
