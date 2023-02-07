# Workflow

See also in : 
 - [English](README.md)

# Sommaire

- [GIT](#git)
  - [Modèle de branchage](#modèle-de-branchage)
    - [Branches de fonctionnalités](#branches-de-fonctionnalités)
    - [Sortie de nouvelles versions](#sortie-de-nouvelles-versions)
    - [Hotfix](#hotfix)
  - [Nommage des commits](#nommage-des-commits)
    - [Syntaxe](#syntaxe)
    - [Type](#type)
    - [Description](#description)
    - [Body](#body)
    - [Footer](#footer)
    - [Exemples](#exemples)
  - [Versionnage](#versionnage)
    - [MAJOR](#major)
    - [MINOR](#minor) 
    - [PATCH](#patch)

- [MODÈLES](#modèles)
  - [Modèle des issues](#modèle-des-issues)
  - [Modèle des pull requests](#modèle-des-pull-requests) 


# Git

## Modèle de branchage

Nous utilisons [OneFlow](https://www.endoflineblog.com/oneflow-a-git-branching-model-and-workflow) comme stratégie Git.
OneFlow est basé sur GitFlow mais n'utilise qu'une seule
branche à longue durée de vie (ici `main`).

### Branches de fonctionnalités

Toute nouvelle fonctionnalité doit être développé sur 
une branche à part, nommé `feature/<my-feature>`. Il n'est 
pas obligatoire de pousser la branche de travail sur le 
dépot, mais il possible de le faire pour éviter de perdre son
travail ou encore lors de travaux longs.

Les branches de travail sont crées de cette manière :
```
git checkout -b feature/<my-feature> main
```

Pousser la branche vers le dépot :
```
git push -u origin feature/<my-feature>
```

Lorsque la fonctionnalité à implémenter est fonctionelle,
on l'implémente dans `main` de cette façon :
```
git checkout feature/<my-feature>
git rebase -i main                               # Permet d'effectuer des modifications aux commits
git checkout main
git merge --no-ff feature/<my-feature>
git push
git branch -d feature/<my-feature>
```

Visuellement :
![](img/feature-branch-rebase-and-merge-final.png)

Si la branche à été poussé vers le dépot :
```
git push origin :feature/<my-feature>
```

Notes : Bien évidemment, il est possible de commit sur
`main`, mais il faut l'éviter le plus possible.

### Sortie de nouvelles versions

Les branches de sortie servent à préparer la sortie de la
nouvelle version du logiciel.

Note sur le numéro de version : Nous utilisons le "schemantic versionning", voir "[versionnage](#versionnage)".

Démarrage de la branche :
```
git checkout -b release/<version-number> <commit-hash>
```

Notes : Les branches de sortie ne démarrent pas forcément
depuis l'état actuel de `main`, d'où la présence de
`<commit-hash>`.

Quand la sortie de la nouvelle version est possible, on 
ajoute la nouvelle version par dessus main :
```
git checkout release/<release-number>
git tag <version-number>
git checkout main
git merge release/<version-number>
git push --tags
git branch -d release/<version-number>
```
Visuellement :
![](img/release-branch-merge-final.png)

Si la branche a été poussé vers le repo :
```
git push origin :release/<version-number>
```

### Hotfix

Les branches "hotfix" se comportent comme des branches 
de fonctionnalité, malgrès que leur présence ne soit pas désiré. Elles permettent de résoudre des problèmes de 
dernière minute.

Création de la branche :
```
git checkout -b hotfix/<version-number> <version-number-to-fix>
```

Si on veut pousser la branche vers le dépot :
```
git push -u origin hotfix/<version-number>
```

Une fois les correctifs appliqués, il est temps de pousser 
la nouvelle version :
```
git checkout hotfix/<version-number>
git tag <version-number>
git checkout main
git merge hotfix/<version-number>
git push --tags
git branch -d hotfix/<version-number>
```

Visuellement :
![](img/hotfix-branch-merge-final.png) 

Si la branche à été poussé sur le dépot :
```
git push origin :hotfix/<version-number>
```

## Nommage des commits

Pour nommer les commits, nous nous sommes inspirés du 
modèle [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

### Syntaxe

La systaxe est composé de 4 parties :
```
<type>: <description>

[body]

[footer]
```

### Type

Il y a 5 types différents :

 1. **feat** : Un commit de type `feat` introduit une nouvelle fonctionnalité dans le code.

 2. **fix** : Un commit de type `fix` résoud un bug dans le code.

 3. **style** : Un commit de type `style` change un élément lié au style dans le code.

 4. **docs** : Un commit de type `docs` ajoute/modifie un élément de la documentation.

 5. **revert** : Un commit de type `revert` annule un commit précédent.

### Description 

Une bonne description commence par une majuscule, et ne se termine pas par un point. Elle doit être écrite en Anglais et doit être formulé à l'impératif. Elle doit faire moins de 50 lettres pour une visibilité optimale. Enfin, elle doit répondre à la question suivante : *Ce commit va..*

Voici quelques verbes-clés qui peuvent aider :
 - Add
 - Remove
 - Create
 - Fix 
 - Release 
 - Modify
 - Update
 - etc

### Body 

Le texte du body doit faire moins de 72 caractères horizontallement. Le body doit expliquer le quoi, le pourquoi et le comment de ce commit.

### Footer 

Le footer contient le numéro de l'issue traité (`Resolve: #123`), et/ou le hash du commit lors d'un `revert` (`Revert: 9efc5d`). Il contient aussi le nom des personnes ayant vérifié une pull request (`Reviewed-by: Someone`).

### Exemples

```
fix: Prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Resolve: #123
```

## Versionnage

Pour le versionnage, nous nous sommes basés sur [Semantic Versionning](https://semver.org/).

### MAJOR

Ce numéro n'est pas modifié automatiquement.

### MINOR

Ce numéro est incrémenté à chaque fusion de fonctionnalité dans `main`.

### PATCH

Ce numéro est incrémenté à chaque hotfix fusioné dans `main`.


# Modèles

## Modèle des issues
```
**Note: for support questions, please ask on the Discord**. This repository's issues are reserved for feature requests and bug reports.

* **I'm submitting a ...**
  - [ ] bug report
  - [ ] feature request
  - [ ] support request => Please do not submit support request here, see note at the top of this template.


* **What do you want to request or report?**



* **What is the current behavior?**



* **If the current behavior is a bug, please provide the steps to reproduce.**



* **What is the expected behavior?**



* **What is the motivation / use case for changing the behavior?**



* **Please tell us about your environment:**
  
  - Version: 1.x.x-abc
  - Python version: 


* **Other information** (e.g. detailed explanation, stacktraces, related issues, suggestions how to fix, links for us to have context, eg. stackoverflow, gitter, etc)

```

## Modèle des pull requests
```
**Please check if the PR fulfills these requirements**
- [ ] A similar PR is not already submitted
- [ ] Commit message(s) and branch name follows our guidelines
- [ ] Tests for the changes have been added (for bug fixes / features)
- [ ] Docs have been added / updated (for bug fixes / features)


**What kind of change does this PR introduce?** (Bug fix, feature, docs update, ...)



**What is the current behavior?** (You can also link to an open issue here)



**What is the new behavior (if this is a feature change)?**


**Other information** (e.g. system tested on, etc)

```
