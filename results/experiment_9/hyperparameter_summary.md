# Hyperparameter Summary - experiment_9

## Experiment Note

experiment_3에서 clip_vit_b32 + rbf_svm 조합의 성능 향상이 확인되었지만, 이후 실험은 linear_svm의 C 조정에 집중되었다. 따라서 본 실험에서는 rbf_svm의 C 값을 50으로 증가시켜 비선형 결정 경계를 사용하는 RBF SVM이 추가적인 성능 향상을 보이는지 확인한다.

## Classifier Parameters

### knn_baseline

- Change note: baseline 유지
- Important parameters: `{"n_neighbors": 5, "weights": "distance", "metric": "cosine"}`

### logistic_regression

- Change note: baseline 유지
- Important parameters: `{"C": 1.0, "penalty": "l2", "solver": "lbfgs", "max_iter": 1000, "class_weight": null}`

### linear_svm

- Change note: experiment_7의 C=15.0
- Important parameters: `{"estimator__C": 15.0, "estimator__class_weight": null, "estimator__max_iter": 5000, "cv": 3}`

### rbf_svm

- Change note: C = 50
- Important parameters: `{"estimator__kernel": "rbf", "estimator__C": 30.0, "estimator__gamma": "scale", "estimator__class_weight": null, "cv": 3}`

### random_forest

- Change note: baseline 유지
- Important parameters: `{"n_estimators": 500, "max_depth": 8, "min_samples_leaf": 5, "min_samples_split": 10, "max_features": "sqrt", "class_weight": null}`

### xgboost

- Change note: baseline 유지
- Important parameters: `{"n_estimators": 200, "max_depth": 3, "learning_rate": 0.03, "subsample": 0.8, "colsample_bytree": 0.7, "min_child_weight": 5, "reg_alpha": 0.1, "reg_lambda": 2.0}`

