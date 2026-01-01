🎯 Vue d'ensemble du projet:

1.1 Le Contexte Médical

Le paludisme est une maladie infectieuse causée par des parasites du genre Plasmodium, transmis par la piqûre de moustiques anophèles. Selon l'OMS, elle cause des centaines de milliers de décès par an. Le dépistage précoce est vital pour la survie du patient.
1.2 La Problématique

Actuellement, la méthode de référence ("Gold Standard") pour le diagnostic est la microscopie optique. Un technicien de laboratoire examine une goutte de sang (frottis) sur une lame de verre pour y chercher visuellement le parasite. Cependant, cette méthode présente des limites majeures :

  **Temps : L'examen d'une seule lame peut prendre 15 à 30 minutes.

  **Expertise : Elle nécessite un personnel hautement qualifié, souvent rare dans les zones endémiques.

  **Fatigue : La répétition de la tâche entraîne une fatigue oculaire et mentale, augmentant le risque de faux diagnostics.

1.3 La Solution Proposée (Approche Technique)

Ce projet vise à automatiser cette étape de détection visuelle grâce à l'Intelligence Artificielle. Nous utilisons une approche de Classification Binaire d'Images supervisée.

Le système est basé sur un Réseau de Neurones Convolutif (CNN). Contrairement aux algorithmes classiques, le CNN est capable d'apprendre par lui-même les caractéristiques visuelles complexes (formes, textures, taches colorées) qui distinguent une cellule infectée.
1.4 Objectifs du Projet

  **Automatisation : Réduire le temps d'analyse de plusieurs minutes à quelques millisecondes.

  **Accessibilité : Proposer un outil capable de fonctionner avec une précision élevée (>95%).

  **Pédagogie : Explorer l'implémentation de réseaux de neurones profonds avec la bibliothèque PyTorch.

🗂️ Description du Dataset

**Source des données : Le jeu de données utilisé est public et provient du National Institutes of Health (NIH). Il est disponible via Kaggle sous le nom : Cell Images for Detecting Malaria.

**Structure et Volumétrie : Le dataset contient un total de 27 558 images de cellules sanguines, réparties équitablement :

***13 779 images de la classe Parasitized (Infectées).

***13 779 images de la classe Uninfected (Saines).

**Caractéristiques des images :

***Format : PNG.

***Dimensions d'origine : Variables (de 40x40 à 200x200 pixels environ).

***Canaux : 3 canaux couleurs (RGB).

**Prétraitement appliqué : Vu la variation des dimensions, toutes les images ont été redimensionnées (Resized) en 64x64 pixels et normalisées (conversion en Tenseurs PyTorch) avant d'être injectées dans le réseau de neurones.
**Dataset Structure:
Dataset/
├── Parasitized/
│   ├── C100P61ThinF_IMG_20150918_144104_cell_162.png
│   ├── C100P61ThinF_IMG_20150918_144104_cell_163.png
│   └── ...
└── Uninfected/
    ├── C1_thinF_IMG_20150604_104722_cell_9.png
    ├── C1_thinF_IMG_20150604_104722_cell_10.png
    └── ...
🧠 Architecture du Modèle (CNN)

**Type de modèle : Nous avons conçu un Réseau de Neurones Convolutif (CNN) "from scratch" (parti de zéro) en utilisant le framework PyTorch. Ce type de modèle est l'état de l'art pour le traitement d'images.

**Structure du réseau (MalariaCNN) : Le modèle agit en deux phases principales :

***Extraction de caractéristiques (Feature Extraction) :

****2 blocs de Convolution (Conv2d) suivis d'activation ReLU et de Pooling (MaxPool2d).

****Objectif : Détecter automatiquement les formes (bords, textures, taches du parasite) tout en réduisant la dimensionnalité de l'image.

***Classification (Tête du réseau) :

****Aplatissement des données (Flatten).

****Couches entièrement connectées (Linear).

****Sortie : Une couche finale avec activation Sigmoid.

**Configuration de l'entraînement :

***Fonction de perte (Loss) : BCELoss (Binary Cross Entropy), spécifique pour la classification binaire.

***Optimiseur : Adam (Adaptive Moment Estimation) avec un taux d'apprentissage (learning rate) de 0.001.

***Batch Size : 32 images par itération.
📈 Performances et Résultats

**Métriques Globales : Après un entraînement sur 5 à 10 époques, le modèle atteint des performances robustes sur le jeu de validation (données jamais vues durant l'entraînement) :

***Précision (Accuracy) : ~95% (Le modèle a raison 95 fois sur 100).

***Perte (Loss) : < 0.20 (Indique une grande confiance dans les prédictions).

**Analyse Graphique : Les courbes d'apprentissage montrent une convergence saine : la perte diminue régulièrement et la précision augmente, sans signe majeur de surapprentissage (overfitting) (l'écart entre la courbe d'entraînement et de validation reste faible).

**Matrice de Confusion : L'analyse des erreurs montre que le modèle excelle à minimiser les Faux Négatifs (cas où une cellule infectée est classée comme saine), ce qui est le critère le plus critique pour un diagnostic médical.
