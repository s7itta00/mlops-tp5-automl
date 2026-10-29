# Modele manuel ou modele AutoML ? (TP05)

| Critere                    | Manuel (rf 500/16)                         | AutoML (FLAML 90 s)                          |
|----------------------------|--------------------------------------------|----------------------------------------------|
| Famille de modele          | foret aleatoire                            | xgboost (gradient boosting)                  |
| F1 sur le jeu de test      | 0.8669                                     | 0.8582                                       |
| Temps de recherche machine | 2.9 s                                      | 90.3 s                                       |
| Temps humain (estimation)  | ~45 min (reglage au TP2)                   | ~10 min (script + lancement)                 |
| Latence mediane (ms)       | 46.3 / 45.9 (moy. ~46.1)                   | 16.5 / 15.6 (moy. ~16.1)                     |
| Latence p95 (ms)           | 74.0 / 64.1 (moy. ~69.1)                   | 31.4 / 20.5 (moy. ~26.0)                     |
| Taille du fichier (Mo)     | 13.37                                      | 0.29                                         |
| Explicabilite              | importance des variables, 500 arbres,      | importance des variables, 333 arbres,        |
|                            | profondeur max 16 (arbres profonds,        | max_leaves = 4 (arbres tres petits), reglages|
|                            | difficiles a lire un par un)               | lisibles dans les parametres cfg_...         |

## Decision (5 lignes) : quel modele mettre en production, et pourquoi ?

Je mets en production le modele AutoML (xgboost, alias production de fraude-automl).
Il perd seulement 0.0087 de F1 (0.8582 contre 0.8669), un ecart faible face aux gains operationnels.
Il repond environ 3 fois plus vite (mediane ~16 ms contre ~46 ms) et pese 46 fois moins (0.29 Mo contre 13.37 Mo).
Ces gains comptent pour l'image Docker, la charge a grande echelle et un usage mobile.
Si la detection de fraude exige le meilleur F1 possible, le modele manuel reste une alternative valable.