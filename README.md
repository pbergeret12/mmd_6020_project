# Projet MMD6020 — Analyse et prédiction de la schizophrénie par IRM fonctionnelle

**Pierre Bergeret — Sabrina Grenier**

Ce document vise à expliquer comment utiliser le code de ce dépôt afin de reproduire les résultats obtenus dans le cadre de notre projet pratique MMD6020.

---

## 1. Téléchargement des données

Les données utilisées sont :

- Le dataset **ds000030** d’OpenNeuro  
  <https://openneuro.org/datasets/ds000030/versions/1.0.0>

- Les données du projet **COBRE**  
  <https://fcon_1000.projects.nitrc.org/indi/retro/cobre.html>  
  Le dataset COBRE nécessite une inscription au projet *INDI* sur le site NITRC avant téléchargement.  
  Après avoir créé un compte, vous pouvez vous inscrire ici :  
  <https://www.nitrc.org/projects/fcp_private/>  
  (approbation automatique sous 24h maximum)

- Le dataset **ds004302** d’OpenNeuro  
  <https://openneuro.org/datasets/ds004302/versions/1.0.1>

---

## 2. Préprocessing

Les données ont été prétraitées et les connectomes fonctionnels calculés à l’aide du logiciel **Halfpipe**, qui fonctionne particulièrement bien avec des données de neuroimagerie au format BIDS.

- Lien vers le GitHub de Halfpipe : <https://github.com/HALFpipe/HALFpipe>  
- Manuel utilisateur : <https://fmri.science/halfpipe/>

Les datasets OpenNeuro sont déjà au format BIDS.

Le dataset COBRE a dû être modifié après téléchargement des données brutes (voir le notebook `bidsification_cobre`).

Les connectomes ont été générés à partir de l’atlas fonctionnel **Schaefer2018Combined** avec **434 ROI**, composé de :
- 400 parcelles corticales (atlas Schaefer400)  
  <https://github.com/ThomasYeoLab/CBIG/tree/master/stable_projects/brain_parcellation/Schaefer2018_LocalGlobal>
- 17 ROI sous-corticales (issues de FreeSurfer)
- 17 ROI du cervelet (atlas Buckner2011)

L’atlas 3D est disponible en NIFTI, et l’ordre des labels correspondants dans un fichier TSV associé, dans le dossier `atlas`.

Les datasets utilisés pour entraîner les modèles ont été créés à l’aide des notebooks du dossier `preprocessing/df_creation`, puis concaténés avec le notebook `create_df_schizo.ipynb`.

Tous les notebooks du dossier `analysis` sont conçus pour fonctionner avec les datasets :

- `schaefcomb_Wang2023Simple_dfschizo.tsv`
- `schaefcomb_Wang2023SimpleGSR_dfschizo.tsv`

Téléchargeables sur le Drive :  
<https://drive.google.com/drive/folders/1h6Z99gmPFacNOXRMUgfrTdlU2xRijZjF>  
(Veuillez demander l’accès après inscription au projet NITRC pour utiliser les données COBRE.)

---

## 3. Préparation de l’environnement pour exécuter les scripts

Afin de pouvoir tester les notebooks (après avoir téléchargé les datasets depuis le Drive), exécutez les commandes suivantes.

### Sous Linux / macOS :

```bash
python -m venv venv_mmd6020
source venv_mmd6020/bin/activate
pip install -r requirements.txt
```

### Sous Windows (PowerShell / Terminal) :

```powershell
python -m venv venv_mmd6020
.env_mmd6020\Scriptsctivate
pip install -r requirements.txt
```

---

## 4. Analyses démographiques des cohortes et quantification des variables confondantes

Le notebook `analysis/demographic_analysis` permet de produire les figures illustrant :

- la composition des cohortes ;
- la distribution des variables confondantes.

---

## 5. Entraînement des modèles

Les notebooks du dossier `model` permettent d’entraîner les différents modèles testés :

- régression logistique Ridge  
- régression logistique Lasso  
- SVM  
- Random Forest  
- réseau de neurones basique

Le notebook `learning_curve_ridge` entraîne un modèle de régression logistique Ridge sur des sous-échantillons de taille croissante (+10 % à chaque itération).

Les poids des modèles et les métriques sont exportés dans des dossiers dédiés à la racine du projet.

---

## 6. Interprétation des poids

Le notebook `analysis/results_interpretation/interpretation.ipynb` contient le code utilisé pour analyser les poids des modèles.  
Il utilise les labels de l’atlas **Schaefer2018Combined** afin de relier chaque variable à un réseau fonctionnel principal.

---
