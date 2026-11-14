# Tatouage et Indexation d'Images

Projets réalisés dans le cadre de la matière **Tatouage et Indexation d'Images** (RT5).

**Auteur :** Hanine BEN AMOR

## Contenu du dépôt

| # | Projet | Fichier | État |
|---|--------|---------|------|
| 1 | CBIR, approche classique (descripteurs globaux) | [`01_CBIR_ApprocheClassique_HanineBENAMOR.ipynb`](01_CBIR_ApprocheClassique_HanineBENAMOR.ipynb) | Terminé |
| 2 | CBIR, approche classique (descripteurs locaux : Harris + SIFT) | à venir | À faire |

---

## 1. Recherche d'images par le contenu : descripteurs globaux

Moteur de recherche d'images par le contenu (CBIR, *Content-Based Image Retrieval*) construit sur une partie de la base **COREL** (270 images).

Le système fonctionne en deux phases :

- **Indexation (hors ligne) :** extraction d'un vecteur descripteur pour chaque image de la base.
- **Recherche (en ligne) :** extraction du même descripteur pour l'image requête, puis comparaison avec la base d'index pour renvoyer les images les plus proches.

### Étapes réalisées

**Pipeline de l'énoncé**

1. Lecture de la base et des images requêtes
2. Recherche par image entière (distance euclidienne pixel à pixel)
3. Recherche par couleur (moments : moyenne et variance par canal)
4. Ajout de l'histogramme HSV quantifié
5. Ajout de la texture (GLCM : contraste, corrélation, énergie, homogénéité)
6. Ajout de la forme (HOG)
7. Robustesse aux transformations géométriques (translation, rotation, changement d'échelle, miroir)

**Améliorations**

- Normalisation et pondération des distances par descripteur
- Autres espaces de couleur : RGB, HSV, Lab
- Distances adaptées aux histogrammes : χ², intersection, Bhattacharyya
- Autres descripteurs de texture : filtres de Gabor, LBP
- Autre descripteur de forme : moments de Hu
- Évaluation quantitative sur toute la base : précision@k et mAP
- Interface graphique (Gradio) pour soumettre une image requête

### Principaux résultats

| Configuration | P@5 | mAP |
|---|---|---|
| Image entière | 0,532 | 0,548 |
| Couleur | 0,563 | 0,504 |
| + Histogramme | 0,587 | 0,473 |
| + Texture (GLCM) | 0,415 | 0,403 |
| + HOG | 0,416 | 0,403 |
| 4 descripteurs normalisés et pondérés | | 0,509 |
| Combinaison améliorée avec poids optimisés | | **0,561** |

Avec la concaténation brute de l'énoncé, le contraste GLCM écrase les autres descripteurs, ce qui fait chuter les résultats et rend HOG sans effet. En normalisant chaque distance par bloc puis en pondérant (histogramme HSV avec χ², moments Lab, LBP avec χ², HOG), on obtient la meilleure mAP du projet.

### Exécution

Le notebook est prévu pour **Google Colab**.

1. Copier la base d'images sur Google Drive (lien dans le notebook), dans `My Drive/CBIR_DataSet/` : les images de la base dans `obj_decoys/` et les images requêtes dans `img_requetes/`.
2. Ouvrir le notebook dans Colab et exécuter les cellules dans l'ordre. Le montage du Drive se fait dans la première cellule.

Bibliothèques utilisées : `numpy`, `opencv-python`, `scikit-image`, `matplotlib`, `pandas`, `gradio`.

En local, installer les dépendances puis adapter le chemin de la base dans la cellule de configuration :

```bash
pip install numpy opencv-python scikit-image matplotlib pandas gradio
```
