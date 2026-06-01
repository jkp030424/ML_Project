# Hyperparameter Summary - 8

## Experiment Note

Sequential tuning after experiment_6. Linear SVM의 C를 증가시켰을 때 C=10.0까지는 성능이 향상되었으나, C=30.0에서는 Macro F1이 감소하고 std가 증가하였다. 따라서 최적점이 C=10.0과 C=30.0 사이에 있는지 확인하기 위해 C=15.0을 실험한다.

## Classifier Parameters

### knn_baseline

- Change note: baseline 유지
- Important parameters: `{"n_neighbors": 5, "weights": "distance", "metric": "cosine"}`

### logistic_regression

- Change note: baseline 유지
- Important parameters: `{"C": 1.0, "penalty": "l2", "solver": "lbfgs", "max_iter": 1000, "class_weight": null}`

### linear_svm

- Change note: experiment_7의 C=15.0에서 C=20.0으로 증가. C=15.0과 C=30.0 사이 탐색
- Important parameters: `{"estimator__C": 20.0, "estimator__class_weight": null, "estimator__max_iter": 5000, "cv": 3}`

### rbf_svm

- Change note: baseline 유지
- Important parameters: `{"estimator__kernel": "rbf", "estimator__C": 1.0, "estimator__gamma": "scale", "estimator__class_weight": null, "cv": 3}`

### random_forest

- Change note: baseline 유지
- Important parameters: `{"n_estimators": 500, "max_depth": 8, "min_samples_leaf": 5, "min_samples_split": 10, "max_features": "sqrt", "class_weight": null}`

### xgboost

- Change note: baseline 유지
- Important parameters: `{"n_estimators": 200, "max_depth": 3, "learning_rate": 0.03, "subsample": 0.8, "colsample_bytree": 0.7, "min_child_weight": 5, "reg_alpha": 0.1, "reg_lambda": 2.0}`

