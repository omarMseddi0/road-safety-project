# Sécurité routière — projet académique STATS19

**Objectif réalisé :** classifier la gravité enregistrée d’une collision corporelle : mortelle, grave ou légère.

Équipe : Oussema Sahbeni, Aziz Zina, Mahdi Zaied, Omar Mseddi et Youssef Takeli.

## Notebooks à exécuter dans l’ordre

1. [Compréhension métier et données](notebooks/01_comprehension_donnees.ipynb) : téléchargement, présentation des trois CSV, qualité et exploration.
2. [Préparation et nettoyage](notebooks/02_preparation_donnees.ipynb) : codes DfT, valeurs manquantes, clés, jointures, agrégats de véhicules et variables temporelles. Le CSV préparé est créé localement dans `data/processed/`.
3. [Modélisation et évaluation](notebooks/03_baseline_gravite.ipynb) : prétraitement, douze configurations, choix sur validation, test, importance des variables, erreurs et courbes affichées dans les cellules.

Tout le code du projet et tous les résultats d’analyse sont dans les notebooks. Aucun script Python, rapport généré, fichier JSON de résultats ou dossier de figures externe n’est nécessaire.

## Données et protocole

Les trois CSV officiels 2021–2025 contiennent 513 801 collisions, 937 265 véhicules et 652 821 victimes. Tous les véhicules sont agrégés par collision ; les victimes servent aux contrôles de cohérence. Le modèle utilise une ligne par collision, avec 24 prédicteurs.

- Entraînement : 2021–2023, 311 349 collisions.
- Validation : 2024, 100 927 collisions ; choix des paramètres sur le F1 macro.
- Test : 2025, 101 525 collisions.

Imputation, standardisation et encodage sont ajustés sur train seulement. La gravité, le nombre de victimes, les identifiants et l’année ne sont pas des prédicteurs. Les CSV bruts et préparés restent locaux et sont ignorés par Git.

## Exécution

Python 3.10 et scikit-learn 1.7.2 ont été utilisés.

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m ipykernel install --user --name road-safety-project --display-name "Python (road-safety-project)"
jupyter lab notebooks/
```

Dans VS Code, choisir le noyau `Python (road-safety-project)`. Exécuter 01, 02 puis 03 avec **Run All**. Le modèle est réellement réentraîné ; le calcul complet prend plusieurs minutes.

## Interprétation

Les scores et erreurs par classe sont présentés dans 03. Le modèle reste faible pour les collisions mortelles ; l’accuracy globale seule ne suffit pas. Il décrit des collisions corporelles déclarées à la police, sans mesurer un risque par trajet ni prouver une causalité. Les changements de déclaration de gravité limitent les comparaisons temporelles. 2025 avait déjà été inspecté lors des premiers essais du projet : une future année indépendante reste nécessaire pour une validation prospective.

Deux objectifs restent pour la suite : classifier la présence d’un cycliste (`has_cycle`) et estimer le nombre de véhicules impliqués (`vehicle_count`), à partir du contexte routier et temporel. Ils réutiliseront le CSV déjà préparé ; la cible et les agrégats de véhicules qui révèlent sa valeur seront exclus des entrées. La présentation initiale du sujet est conservée dans `output/`.

[Source officielle DfT](https://www.gov.uk/government/statistical-data-sets/road-safety-open-data) · [Guide de gravité](https://www.gov.uk/government/publications/guide-to-severity-adjustments-for-reported-road-casualty-statistics). Le dictionnaire de codes est fourni dans `references/`.
