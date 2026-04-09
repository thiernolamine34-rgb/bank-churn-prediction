#  Prédiction du Churn Bancaire — Bank Customer Churn Prediction

##  Contexte métier

Dans le secteur bancaire, **20% des clients quittent leur banque chaque année**.
Fidéliser un client coûte 5 à 7 fois moins cher qu'en acquérir un nouveau.

Ce projet construit un modèle de machine learning capable d'identifier
les clients à risque **avant** leur départ, pour permettre des actions
CRM ciblées et proactives.

---

##  Objectif

Maximiser le **recall** — détecter le maximum de churners réels,
même au prix de quelques fausses alertes CRM.

> En banque, rater un vrai churner est bien plus coûteux
> qu'alerter inutilement un conseiller.

<img width="308" height="163" alt="image" src="https://github.com/user-attachments/assets/a5c838f8-cce7-4160-9322-16d7e21c60d9" />
<img width="308" height="163" alt="image" src="https://github.com/user-attachments/assets/a5c838f8-cce7-4160-9322-16d7e21c60d9" />

---


##  Dataset

| Caractéristique | Valeur |
|---|---|
| Clients | 10 000 |
| Variables | 12 |
| Pays | France, Espagne, Allemagne |
| Taux de churn | 20% (déséquilibre 80/20) |
| Source | Bank Customer Churn Prediction |

---

##  Démarche — 6 modèles comparés

Avant de choisir XGBoost, 6 modèles ont été testés :

| Modèle | Verdict |
|---|---|
| LogisticRegression | Baseline — référence |
| RandomForestClassifier | Bon mais limité |
| LightGBM | Instable sur ce dataset |
| KNeighborsClassifier | Trop sensible au scaling |
| CatBoost | Temps d'entraînement élevé |
| **XGBoost** | **Retenu** |

**Pourquoi XGBoost :**
`scale_pos_weight` gère nativement le déséquilibre 80/20 +
régularisation L1/L2 intégrée = meilleur recall + overfitting minimal.

---

##  Pipeline technique
Audit données → EDA → Feature Engineering → Baseline
→ Comparaison 6 modèles → Optimisation XGBoost (8 runs)
→ Seuil optimal → SHAP → Recommandations CRM

### Feature Engineering — 4 variables créées

| Variable | Description | Justification |
|---|---|---|
| `balance_nulle` | 1 si solde = 0€ | Comportement atypique |
| `ratio_balance_salaire` | balance / salaire | Mesure d'engagement financier |
| `client_senior` | 1 si âge > 50 ans | Signal fort identifié en EDA |
| `risque_eleve` | inactif + 1 produit | Combinaison des 2 signaux forts |

### Optimisation XGBoost — 8 runs progressifs

| Run | Méthode | Score F2 |
|---|---|---|
| 1 | RandomizedSearchCV | 0.627 |
| 2 | RandomizedSearchCV | 0.665 |
| 3 | RandomizedSearchCV | 0.679 |
| 4 | RandomizedSearchCV | 0.684 |
| 5 | RandomizedSearchCV | 0.668 |
| 6 | GridSearchCV ciblé | 0.687 |
| 7 | GridSearchCV ciblé | 0.693 |
| **8** | **GridSearchCV final** | **0.693** |

Paramètre clé : `scale_pos_weight = 5.0`
→ Recall : 0.63 (run 1) → **0.81 (run final)** — gain de +18 points

---

##  Résultats finaux

| Métrique | Seuil défaut (0.5) | Seuil optimal (0.45) |
|---|---|---|
| Recall | 81.1% | **86.0%** |
| Précision | 44.4% | 40.4% |
| F2-Score | 0.696 | **0.701** |
| ROC-AUC | **0.88** | 0.88 |
| Overfitting | **1.6%** | 1.6% |

---

##  Insights SHAP — Top variables

| Rang | Variable | Impact |
|---|---|---|
| 1 | `age` | Les +50 ans churent massivement |
| 2 | `products_number` | 3-4 produits = alarme extrême (83%) |
| 3 | `risque_eleve` | Notre feature engineerée — validée |
| 4 | `active_member` | Inactivité = 2x plus de risque |
| 5 | `country_Germany` | Taux de churn 2x supérieur |

---

##  Recommandations CRM

**Action 1 — URGENTE**
Alerte conseiller pour tout score > 0.45 + âge > 50 + inactif
→ Offre : bilan patrimonial gratuit + taux épargne préférentiel

**Action 2 — MENSUELLE**
Campagne de réactivation automatique
pour tout client sans transaction depuis 90 jours

**Action 3 — PRODUITS**
Audit des clients avec 3-4 produits
→ Simplifier le portefeuille, réduire les frais cumulés

**Action 4 — MARCHÉ**
Revue de l'offre en Allemagne
→ Taux de churn 2x supérieur aux autres pays

---

##  Technologies

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![XGBoost](https://img.shields.io/badge/XGBoost-optimisé-green)
![SHAP](https://img.shields.io/badge/SHAP-explicabilité-orange)
![Sklearn](https://img.shields.io/badge/Scikit--learn-pipeline-red)
![Pandas](https://img.shields.io/badge/Pandas-data-lightblue)
![Seaborn](https://img.shields.io/badge/Seaborn-visualisation-purple)


---

##  Auteur

**DIALLO THIERNO MOHAMMAD LAMINE**
Étudiant en IA et Cybersécurité
[LinkedIn](https://www.linkedin.com/in/thierno-mohammad-lamine-diallo-1700b2402/)· [GitHub](https://github.com/thiernolamine34-rgb/)

## Visualisation de la performance avec de l'EXPLAINABLE IA:

**MATRICE DE CONFUSION**


<img width="696" height="526" alt="Capture d&#39;écran 2026-04-09 144106" src="https://github.com/user-attachments/assets/9cab0052-f18a-4134-9e1e-dc7a488ba394" />

**COURBE ROC/AUC**


<img width="967" height="675" alt="Capture d&#39;écran 2026-04-09 144139" src="https://github.com/user-attachments/assets/61f4fe6c-d8d5-4ae5-9141-b5fa62ce77c1" />


**FEATURES IMPORTANCE**


<img width="1177" height="758" alt="Capture d&#39;écran 2026-04-09 144215" src="https://github.com/user-attachments/assets/3711dd6a-0420-4f32-9195-1c9396222163" />

**VISUALISATION DE LA FEATURE LA PLUS IMPORTANTE**


<img width="798" height="612" alt="Capture d&#39;écran 2026-04-09 144229" src="https://github.com/user-attachments/assets/5023a9af-c6a2-4b40-8d8f-d4d1e12ba170" />

**ANALYSE D'UN CLIENT**


<img width="1286" height="739" alt="Capture d&#39;écran 2026-04-09 144242" src="https://github.com/user-attachments/assets/de940d82-1292-4690-9fc1-2cbd7159d5a6" />

