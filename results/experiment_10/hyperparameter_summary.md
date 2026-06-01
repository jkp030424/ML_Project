# Hyperparameter Summary - experiment_10

## Experiment Note

experiment_9에서 rbf_svm의 C를 50.0으로 증가시켰지만, clip_vit_b32 + rbf_svm 조합의 성능이 현재 최고 조합인 clip_vit_b32 + linear_svm(C=15.0)보다 낮게 나타났다. 이는 C=50.0이 과도하게 큰 값일 가능성이 있으므로, 본 실험에서는 rbf_svm의 C를 30.0으로 낮추어 RBF SVM의 적절한 규제 강도 구간을 다시 확인한다.

## Classifier Parameters

### knn_baseline

- Change note: 변경 없음
- Important parameters: `{"n_neighbors": 5, "weights": "distance", "metric": "cosine"}`

### logistic_regression

- Change note: 변경 없음
- Important parameters: `{"C": 1.0, "penalty": "l2", "solver": "lbfgs", "max_iter": 1000, "class_weight": null}`

### linear_svm

- Change note: 현재 최고 성능 설정 유지: C=15.0
- Important parameters: `{"estimator__C": 15.0, "estimator__class_weight": null, "estimator__max_iter": 5000, "cv": 3}`

### rbf_svm

- Change note: 변경: experiment_9의 C=50.0에서 C=30.0으로 감소, gamma는 scale로 유지
- Important parameters: `{"estimator__kernel": "rbf", "estimator__C": 30.0, "estimator__gamma": "scale", "estimator__class_weight": null, "cv": 3}`

### random_forest

- Change note: 변경 없음
- Important parameters: `{"n_estimators": 500, "max_depth": 8, "min_samples_leaf": 5, "min_samples_split": 10, "max_features": "sqrt", "class_weight": null}`

### xgboost

- Change note: 변경 없음
- Important parameters: `{"n_estimators": 200, "max_depth": 3, "learning_rate": 0.03, "subsample": 0.8, "colsample_bytree": 0.7, "min_child_weight": 5, "reg_alpha": 0.1, "reg_lambda": 2.0}`

