# 🏃 Générateur d'Appréciations EPS

Application web autonome (HTML/CSS/JS) permettant de générer automatiquement des appréciations scolaires pour les élèves d'EPS, exportables au format PRONOTE.

---

## 🚀 Démarrage rapide

1. Téléchargez le fichier `appreciations-eleves.html`
2. Ouvrez-le dans votre navigateur (double-clic)
3. Importez votre fichier Excel élèves
4. Choisissez une catégorie et cliquez sur **Générer**
5. Exportez en Excel

> ✅ Aucune installation requise. Fonctionne 100% localement, sans connexion internet.

---

## 📁 Structure du projet

```
appreciations-eleves.html   ← Application complète (fichier unique)
README.md                   ← Ce fichier
```

---

## 📂 Format du fichier Excel attendu

Le fichier `.xlsx` doit contenir une feuille principale avec les colonnes suivantes :

| Colonne | Description | Exemple |
|---|---|---|
| `Élèves` | Nom de l'élève | DUPONT |
| `Sexe (F/M)` | Genre de l'élève | M ou F |
| `Prénom de l'élève` | Prénom | Lucas |
| `Classe` | Classe de l'élève | 2nde A |

> La détection des colonnes est automatique. Les noms exacts ne sont pas obligatoires — l'application cherche les mots-clés (`élève`, `sexe`, `prénom`, `classe`).

### Onglets reconnus

- **Appréciation** — feuille principale avec la liste des élèves
- **Autre appréciations** — base lexicale (lue automatiquement)
- Les autres onglets sont ignorés

---

## 🏫 Fonctionnement par espace

### Espace Seconde

| Règle | Détail |
|---|---|
| Genre | Appréciation **genrée** (masculin ou féminin) |
| Identité | Commence avec le **prénom** de l'élève |
| Génération | Appréciation de **semestre uniquement** |

### Espace 1ère / Terminale

| Règle | Détail |
|---|---|
| Genre | Toujours au **masculin** (même pour les filles) |
| Identité | **Jamais** de nom/prénom — commence par `L'élève…` ou `Elève…` |
| Génération | Appréciation de **semestre** + **Fiche Avenir** |

---

## 📋 Catégories disponibles

| # | Libellé | Couleur |
|---|---|---|
| 1 | Excellent semestre – moteur | 🟢 |
| 2 | Très bon semestre – sérieux, investi | 🟢 |
| 3 | Semestre satisfaisant | 🟢 |
| 4 | Discret mais sérieux | 🟡 |
| 5 | Sérieux mais manque de concentration | 🟡 |
| 6 | Potentiel irrégulier | 🟡 |
| 7 | Dynamique à canaliser | 🟡 |
| 8 | Manque d'investissement | 🔴 |
| 9 | Inaptitude sur la période | 🟣 |
| 10 | Moteur & esprit collectif | 🟢 |
| 11 | Excellent – collectif | 🟢 |

---

## 🧠 Algorithme de génération

- Chaque catégorie contient entre **3 et 6 modèles de phrases** distincts
- Un **système anti-doublon** garantit qu'aucun élève ne reçoit la même formulation qu'un autre dans la même session
- Si tous les modèles ont été utilisés, le pool se réinitialise pour les suivants
- Chaque appréciation est **modifiable directement** dans le tableau

---

## ✨ Amélioration avec l'IA

### Avec clé OpenRouter (recommandé)

1. Créez un compte gratuit sur [openrouter.ai](https://openrouter.ai)
2. Générez une clé API (`sk-or-v1-…`)
3. Collez-la dans la section **Configuration IA** (en bas du panneau gauche)
4. Cliquez sur le bouton ✨ à côté d'une appréciation

Le modèle utilisé est `mistralai/mistral-7b-instruct:free` (gratuit).

### Sans clé API

L'application génère une reformulation locale à partir d'un autre modèle de la même catégorie.

### Contraintes respectées par l'IA

- Accord de genre (Seconde) ou masculin neutre (Lycée)
- Anonymat préservé (Lycée)
- Longueur maîtrisée (3 phrases maximum)
- Ton scolaire et institutionnel

---

## 📤 Export Excel

Le fichier exporté contient **3 onglets** :

| Onglet | Contenu |
|---|---|
| `Seconde` | Élève · Prénom · Classe · Sexe · Appréciation semestre |
| `1ère-Terminale` | Élève · Classe · Sexe · Appréciation semestre · Fiche Avenir |
| `Export PRONOTE` | Tous les élèves réunis, format colonnes standard |

Le fichier est nommé automatiquement : `Appreciations_EPS_AAAA-MM-JJ.xlsx`

---

## ⚙️ Contraintes techniques

| Élément | Détail |
|---|---|
| Technologies | HTML5 · CSS3 · JavaScript (Vanilla) |
| Librairie Excel | [SheetJS (xlsx.js)](https://sheetjs.com) v0.18.5 via CDN |
| Connexion | Requise uniquement pour charger SheetJS et les polices Google Fonts |
| Serveur | Aucun — fichier local uniquement |
| Compatibilité | Chrome, Firefox, Edge, Safari (versions récentes) |

> En cas d'utilisation hors-ligne complète, remplacez le lien CDN SheetJS par une copie locale de la librairie.

---

## 🔒 Données & Confidentialité

- Aucune donnée élève n'est envoyée sur un serveur externe
- Tout le traitement se fait **dans le navigateur**
- La clé API OpenRouter (si renseignée) est utilisée uniquement pour les appels IA à la demande et n'est pas sauvegardée

---

## 📝 Personnalisation

Pour ajouter de nouveaux modèles d'appréciations, éditez les objets JavaScript dans le fichier HTML :

- `TEMPLATES` — modèles Seconde (genrés, avec prénom)
- `LYCEE_TEMPLATES` — modèles Lycée (masculin neutre, anonymes)
- `FICHE_AVENIR` — modèles Fiche Avenir (ton institutionnel)

Chaque catégorie contient un tableau `M` (masculin) et `F` (féminin) pour `TEMPLATES`, ou simplement un tableau pour `LYCEE_TEMPLATES`.

---

## 👨‍🏫 Auteur

Outil développé pour un usage pédagogique en EPS.  
Généré et maintenu avec l'aide de [Claude](https://claude.ai) — Anthropic.
