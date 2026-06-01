# Hyperparameter Summary - experiment_3

## Experiment Note

SVM C/gamma 조정

## Classifier Parameters

### knn_baseline

- Change note: baseline 유지
- Important parameters: `{"n_neighbors": 5, "weights": "distance", "metric": "cosine"}`

### logistic_regression

- Change note: baseline 유지
- Important parameters: `{"C": 1.0, "penalty": "l2", "solver": "lbfgs", "max_iter": 1000, "class_weight": null}`

### linear_svm

- Change note: C=0.1 → 1.0
- Important parameters: `{"estimator__C": 1.0, "estimator__class_weight": null, "estimator__max_iter": 5000, "cv": 3}`

### rbf_svm

- Change note: C=1.0 → 10.0, gamma=scale 유지
- Important parameters: `{"estimator__kernel": "rbf", "estimator__C": 10.0, "estimator__gamma": "scale", "estimator__class_weight": null, "cv": 3}`

### random_forest

- Change note: baseline으로 복원
- Important parameters: `{"n_estimators": 500, "max_depth": 8, "min_samples_leaf": 5, "min_samples_split": 10, "max_features": "sqrt", "class_weight": null}`

### xgboost

- Change note: baseline으로 복원
- Important parameters: `{"n_estimators": 200, "max_depth": 3, "learning_rate": 0.03, "subsample": 0.8, "colsample_bytree": 0.7, "min_child_weight": 5, "reg_alpha": 0.1, "reg_lambda": 2.0}`

