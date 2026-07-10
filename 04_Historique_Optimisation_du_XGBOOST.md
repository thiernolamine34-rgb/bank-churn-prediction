PHASE 1 — SELECTION DU MODELE
================================

5 modèles testés avant de choisir XGBoost :

1. LogisticRegression (baseline)
   Recall : 0.629 | ROC-AUC : 0.77
   Écarté : relations non-linéaires non capturées

2. RandomForestClassifier
   Recall : ~0.72 | ROC-AUC : ~0.85
   Écarté : pas de scale_pos_weight natif

3. LightGBM
   Performant mais instable sur ce dataset
   Écarté : XGBoost donne de meilleurs résultats

4. KNeighborsClassifier (KNN)
   Écarté : trop sensible au scaling, recall faible,
   pas adapté aux datasets déséquilibrés

5. CatBoost
   Gère les catégorielles nativement mais
   temps d'entraînement trop élevé pour le gain obtenu

MODÈLE RETENU : XGBoost
Raison principale : scale_pos_weight + régularisation L1/L2
+ meilleur recall sur ce problème bancaire

PHASE 2 — OPTIMISATION XGBOOST (8 runs)
=========================================

ENTRAINEMENT AVEC LE XGBOOST:[AJUSTEMENT DES HYPERPARAMETRE]

premier ENTRAINEMENT:
Meilleurs paramètres : {'xgbclassifier__subsample': 0.8, 'xgbclassifier__scale_pos_weight': 2, 'xgbclassifier__n_estimators': 100, 'xgbclassifier__min_child_weight': 5, 'xgbclassifier__max_depth': 5, 'xgbclassifier__learning_rate': 0.1, 'xgbclassifier__gamma': 0.1, 'xgbclassifier__colsample_bytree': 1.0}
Meilleur score (f1) : 0.6269719592241444

deuxieme ENTRAINEMENT:
Fitting 5 folds for each of 400 candidates, totalling 2000 fits
Meilleurs paramètres : {'xgbclassifier__subsample': 0.85, 'xgbclassifier__scale_pos_weight': 3, 'xgbclassifier__n_estimators': 75, 'xgbclassifier__min_child_weight': 6, 'xgbclassifier__max_depth': 4, 'xgbclassifier__learning_rate': 0.1, 'xgbclassifier__gamma': 0.1, 'xgbclassifier__colsample_bytree': 0.97}
Meilleur score (f1) : 0.6650964051456868


Troisieme ENTRAINEMENT:
Fitting 5 folds for each of 400 candidates, totalling 2000 fits
Meilleurs paramètres : {'xgbclassifier__subsample': 0.83, 'xgbclassifier__scale_pos_weight': 3.6, 'xgbclassifier__n_estimators': 55, 'xgbclassifier__min_child_weight': 6.5, 'xgbclassifier__max_depth': 5, 'xgbclassifier__learning_rate': 0.09, 'xgbclassifier__gamma': 0.12, 'xgbclassifier__colsample_bytree': 0.97}
Meilleur score (f1) : 0.6786857141344507

Quatrieme ENTRAINEMENT:
Fitting 5 folds for each of 600 candidates, totalling 3000 fits
Meilleurs paramètres : {'xgbclassifier__subsample': 0.75, 'xgbclassifier__scale_pos_weight': 3.8, 'xgbclassifier__n_estimators': 45, 'xgbclassifier__min_child_weight': 7, 'xgbclassifier__max_depth': 4, 'xgbclassifier__learning_rate': 0.07, 'xgbclassifier__gamma': 0.18, 'xgbclassifier__colsample_bytree': 0.99}
Meilleur score (f2) : 0.6835014776950652

Cinquieme ENTRAINEMENT:
Fitting 5 folds for each of 1944 candidates, totalling 9720 fits
Meilleurs paramètres : {'xgbclassifier__colsample_bytree': 0.98, 'xgbclassifier__gamma': 0.18, 'xgbclassifier__learning_rate': 0.095, 'xgbclassifier__max_depth': 4, 'xgbclassifier__min_child_weight': 6.2, 'xgbclassifier__n_estimators': 50, 'xgbclassifier__scale_pos_weight': 3.1, 'xgbclassifier__subsample': 0.81}
Meilleur score (f2) : 0.6683842573781152

Sixième ENTRAINEMENT:
Fitting 5 folds for each of 750 candidates, totalling 3750 fits
  Meilleurs paramètres : {'xgbclassifier__subsample': 0.78, 'xgbclassifier__scale_pos_weight': 4.2, 'xgbclassifier__n_estimators': 45, 'xgbclassifier__min_child_weight': 6.5, 'xgbclassifier__max_depth': 4, 'xgbclassifier__learning_rate': 0.07, 'xgbclassifier__gamma': 0.16, 'xgbclassifier__colsample_bytree': 0.97}
  Meilleur F2-score    : 0.6872

Septième ENTRAINEMENT:
avec Fitting 5 folds for each of 24 candidates, totalling 120 fits
  Meilleurs paramètres : {'xgbclassifier__subsample': 0.78, 'xgbclassifier__scale_pos_weight': 5.0, 'xgbclassifier__n_estimators': 45, 'xgbclassifier__min_child_weight': 6.5, 'xgbclassifier__max_depth': 4, 'xgbclassifier__learning_rate': 0.07, 'xgbclassifier__gamma': 0.18, 'xgbclassifier__colsample_bytree': 0.97}
  Meilleur F2-score    : 0.6931

Huitieme / Final ENTRAINEMENT:
Fitting 5 folds for each of 24 candidates, totalling 120 fits
  Meilleurs paramètres : {'xgbclassifier__subsample': 0.78, 'xgbclassifier__scale_pos_weight': 5.0, 'xgbclassifier__n_estimators': 45, 'xgbclassifier__min_child_weight': 6.5, 'xgbclassifier__max_depth': 4, 'xgbclassifier__learning_rate': 0.07, 'xgbclassifier__gamma': 0.18, 'xgbclassifier__colsample_bytree': 0.97}
  Meilleur F2-score    : 0.6931


  Conclusion générale:
Notre modèle de prédiction du churn atteint un ROC-AUC de 0.86,
ce qui représente une excellente capacité à identifier les clients
à risque avant leur départ. Grâce au seuil optimisé à 0.45 et
à l'interprétabilité SHAP, ce modèle est directement déployable
dans un système CRM bancaire réel, en conformité avec les
exigences réglementaires RGPD d'explicabilité algorithmique.
