# Football Match Analysis

Analyse automatique de matchs de football en temps réel avec YOLO et computer vision.

![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)
![YOLO](https://img.shields.io/badge/YOLO-v11-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## Fonctionnalités

- **Détection des joueurs** : Détection en temps réel des joueurs sur le terrain
- **Classification d'équipes** : Attribution automatique des joueurs à leur équipe par couleur de maillot
- **Détection des arbitres** : Identification des arbitres (maillots orange/jaune/rose fluo)
- **Lignes de formation** : Visualisation des lignes tactiques (défense, milieu, attaque)
- **Tracking** : Suivi persistant des joueurs avec ByteTrack
- **Statistiques** : Possession de balle et nombre de passes par équipe
- **Minimap** : Vue aérienne du terrain avec positions des joueurs

## Installation

### Prérequis

- Python 3.9+
- pip

### Installation des dépendances

```bash
cd football-analysis
pip install -r requirements.txt
```

### Dépendances principales

- `ultralytics` - YOLO v11 pour la détection
- `opencv-python` - Traitement d'images
- `numpy` - Calculs numériques
- `scikit-learn` - K-means pour classification des couleurs
- `supervision` - ByteTrack pour le tracking

## Utilisation

### Analyse basique

```bash
python main.py input/video.mp4 output/result.mp4
```

### Options disponibles

```bash
python main.py input.mp4 output.mp4 [OPTIONS]

Options:
  --max-frames N        Nombre maximum de frames à traiter
  --skip-frames N       Frames à sauter entre chaque traitement
  --no-pose            Désactiver l'estimation de pose
  --no-heatmap         Désactiver la génération de heatmaps
  --no-minimap         Désactiver la minimap
  --calibration-frames Nombre de frames pour la calibration (défaut: 30)
```

### Exemples

```bash
# Analyser les 1000 premières frames
python main.py match.mp4 output.mp4 --max-frames 1000

# Traiter 1 frame sur 2 (plus rapide)
python main.py match.mp4 output.mp4 --skip-frames 1

# Sans minimap
python main.py match.mp4 output.mp4 --no-minimap
```

## Structure du projet

```
football-analysis/
├── main.py                 # Point d'entrée principal
├── config.py               # Configuration globale
├── requirements.txt        # Dépendances Python
├── src/
│   ├── detector.py         # Détection YOLO
│   ├── tracker.py          # Tracking ByteTrack
│   ├── team_classifier.py  # Classification d'équipes
│   ├── possession_tracker.py # Tracking possession/passes
│   ├── pose_estimator.py   # Estimation de pose
│   ├── video_processor.py  # Traitement vidéo
│   └── ...
├── visualization/
│   └── overlay.py          # Annotations visuelles
├── utils/
│   └── colors.py           # Palette de couleurs
└── input/                  # Vidéos d'entrée
```

## Configuration

Les paramètres principaux sont dans `config.py` :

```python
# Seuils de détection
detection_confidence = 0.3  # Confiance minimum pour les joueurs
ball_confidence = 0.2       # Confiance minimum pour le ballon

# Device (auto-détecté)
device = "mps"  # Mac M1/M2/M3/M4
device = "cuda" # NVIDIA GPU
device = "cpu"  # CPU
```

## Visualisation

### Cercles de couleur
- **Bleu** : Équipe A
- **Rouge** : Équipe B
- **Orange** : Arbitre
- **Gris** : Non classifié

### Lignes de formation
Les joueurs alignés horizontalement (3+) sont reliés automatiquement pour visualiser les lignes tactiques.

### Footer
Affiche en temps réel :
- Possession de balle (%)
- Nombre de passes par équipe

## Performances

| Device | FPS moyen |
|--------|-----------|
| Mac M4 (MPS) | ~15-20 FPS |
| NVIDIA RTX 3080 | ~25-30 FPS |
| CPU (i7) | ~3-5 FPS |

## Limitations

- La détection dépend de la qualité vidéo et de l'angle de caméra
- Les couleurs d'équipe doivent être suffisamment distinctes
- Les statistiques de possession sont approximatives

## Licence

MIT License - voir [LICENSE](LICENSE)

## Auteur

Projet créé avec l'aide de Claude (Anthropic)
