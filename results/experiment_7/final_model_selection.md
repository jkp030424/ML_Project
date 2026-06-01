# Final Model Selection

- Final experiment: `experiment_7`
- Encoder: `clip_vit_b32`
- Classifier: `linear_svm`
- Parameters: `{'classifier': 'linear_svm', 'C': 15.0, 'class_weight': None, 'max_iter': 5000, 'calibration_cv': 3, 'random_state': 42}`
- Selection method: `manual_selection_after_cv_hyperparameter_search`

## Selection Note

Experiment 7 was selected as the final model based on stratified 5-fold CV. Linear SVM performance improved as C increased up to 15, but decreased again at C=20 and C=30. Therefore, C=15 was selected as the best regularization strength.

## CV Result

- Macro F1: 0.9215 ± 0.0147
- Accuracy: 0.9215
- Ordinal MAE: 0.0785
- Recall tight: 0.9375
- Recall regular: 0.8815
- Recall oversized: 0.9449
