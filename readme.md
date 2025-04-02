# Git Rebase Lab: Exercices avec conflits (Python)

Bienvenue dans ce mini-projet conçu pour **s'entraîner au `git rebase` avec conflit** dans un environnement contrôlé.

L'objectif est de comprendre et maîtriser le comportement de `git rebase` lors de conflits, à travers un cas concret avec du code Python.

---

## 🧠 Objectif pédagogique

- Utiliser `git rebase` pour réappliquer des commits
- Résoudre un conflit lors d'un rebase entre deux branches
- Garder un historique Git propre et linéaire

---

## 🧱 Structure du projet

```bash
.
├── src/
│   ├── calculator.py       # fichier avec conflit simulé
│   ├── utils.py            # fichier inchangé
│   └── constants.py        # fichier inchangé
├── README.md
```

---

## 🧪 Scénario de l'exercice

1. Tu travailles sur une base commune (`develop`)
2. Deux développeurs travaillent en parallèle :
   - `feature/improve-add`: améliore la fonction `add()` dans `calculator.py`
   - `feature/log-add`: ajoute du logging dans cette même fonction
3. Un conflit survient lors du rebase de `feature/improve-add` sur `develop` mis à jour avec `feature/log-add`

---

## 🚀 Étapes à suivre

### 1. Initialisation du dépôt

```bash
git init
git checkout -b develop
mkdir src && touch src/calculator.py src/utils.py src/constants.py
```

Ajoute ce contenu de base dans `calculator.py` :

```python
def add(a, b):
    return a + b
```

Puis :
```bash
git add .
git commit -m "feat: initial version of calculator"
```

### 2. Créer la branche `feature/log-add`
```bash
git checkout -b feature/log-add
```
Modifie `calculator.py` :
```python
def add(a, b):
    print(f"Addition de {a} + {b}")
    return a + b
```
```bash
git commit -am "feat: add logging to add() function"
git checkout develop
git merge feature/log-add
```

### 3. Créer la branche `feature/improve-add`
```bash
git checkout -b feature/improve-add develop
```
Modifie `calculator.py` :
```python
def add(a, b):
    if type(a) != int or type(b) != int:
        raise ValueError("Both arguments must be integers")
    return a + b

```
```bash
git commit -am "feat: improve add() to check types"
```

### 4. Rebase avec conflit 💥
```bash
git rebase develop
```
Git signale un **conflit dans `calculator.py`**.

Résous-le ainsi :
```python
import logging

def add(a, b):
    if isinstance(a, str) or isinstance(b, str):
        raise TypeError("Inputs must be numbers")
    logging.info(f"Adding {a} and {b}")
    return a + b
```
Puis :
```bash
git add src/calculator.py
git rebase --continue
```

---

## ✅ Vérification

```bash
git log --oneline --graph --all
```
Tu dois voir une **histoire linéaire propre** : `feature/improve-add` rejouée au-dessus de `develop`.

---

## 💡 Tips

- Si tu veux tout recommencer :
```bash
rm -rf .git && git init
```
- Pour tester plusieurs fois, fais une copie du dossier initial
- Utilise `git status`, `git log`, et `git diff` souvent pour comprendre ce qui se passe