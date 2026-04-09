                                problème business:
Le churn bancaire (départ de client) représente une perte directe de revenus.
 Un client parti = frais d'acquisition perdus + revenus futurs perdus. 



                            objectif du modèle:
détecter le maximum de clients qui vont partir, même au prix de quelques fausses alertes. 
Rater un vrai churner est bien plus coûteux qu'alerter inutilement un conseiller.




                            utilisé le modèle:
En réalité, le modèle calcule un score de risque churn (0 à 1) pour chaque client, et une alerte est envoyée au conseiller CRM si le score dépasse un seuil (ex. 0.4). 
Le conseiller appelle alors le client pour lui proposer une offre de rétention.




                            contraintes réglementaires:
les modèles de scoring client doivent être explicables. 
C'est pourquoi SHAP est obligatoire — On dois pouvoir dire au client et au régulateur pourquoi il a été ciblé. C'est la directive RGPD / droit à l'explication.



                            variables sont disponibles et lesquelles sont interdites:
customer_id est un identifiant technique → à supprimer. Dans certaines banques, le genre et l'âge sont aussi soumis à des règles anti-discrimination, ce qui justifie une analyse de fairness (biais du modèle par groupe).     


