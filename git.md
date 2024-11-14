# Aide-mémoire des principales commandes Git

## Commandes d'initialisation de base

| Commande                     | Description                                                                                 |
|------------------------------|---------------------------------------------------------------------------------------------|
| `git init`                   | Initialise un nouveau dépôt Git dans le répertoire actuel.                                  |
| `git clone <url>`            | Clone un dépôt distant depuis l’URL spécifiée.                                              |
| `git status`                 | Affiche l’état des fichiers dans le répertoire de travail et de l’index.                    |


## Commandes de configuration utilisateur et paramètres

| Commande                                               | Description                                                                                                                |
|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| `git config --list`                                    | Affiche tous les paramètres de configuration actuels, y compris ceux définis globalement et localement dans le dépôt.      |
| `git config --global user.name "<Nom Utilisateur>"`    | Définit le nom de l’utilisateur pour tous les commits (à configurer une fois pour une utilisation globale).                |
| `git config --global user.email "<email@example.com>"` | Définit l’email de l’utilisateur pour tous les commits.                                                                    |
| `git config --global core.editor "<editeur>"`          | Définit l'éditeur de texte par défaut pour les messages de commit (ex. `vim`, `nano`, `code` pour VS Code).                |
| `git config --global merge.tool "<outil>"`             | Définit l'outil de fusion par défaut, comme `vimdiff`, `meld`, `kdiff3`, etc.                                              |
| `git config --global color.ui auto`                    | Active la coloration syntaxique pour les commandes Git (valeur `auto` recommandée pour une coloration selon le terminal).  |

## Commandes pour faire un commit

| Commande                     | Description                                                                           |
|------------------------------|---------------------------------------------------------------------------------------|
| `git add <fichier>`          | Ajoute un fichier ou des fichiers à l’index pour les inclure dans le prochain commit. |
| `git add .`                  | Ajoute tous les fichiers modifiés à l’index.                                          |
| `git commit -m "message"`    | Crée un commit avec un message spécifié.                                              |

## Commandes de branches

| Commande                     | Description                                                                    |
|------------------------------|--------------------------------------------------------------------------------|
| `git branch`                 | Affiche la liste des branches locales.                                         |
| `git branch <nom>`           | Crée une nouvelle branche.                                                     |
| `git checkout <branche>`     | Bascule vers une branche spécifique.                                           |
| `git switch <branche>`       | Autre méthode pour basculer entre branches.                                    |
| `git branch -d <branche>`    | Supprime une branche locale (si elle a été fusionnée).                         |
| `git branch -D <branche>`    | Force la suppression d'une branche locale, même non fusionnée.                 |

## Commandes pour fusionner des branches

| Commande                     | Description                                                                                |
|------------------------------|--------------------------------------------------------------------------------------------|
| `git merge <branche>`        | Fusionne une branche spécifique dans la branche courante.                                  |
| `git rebase <branche>`       | Rejoue les commits de la branche courante sur la branche spécifiée (alternatif à `merge`). |
| `git rebase -i <commit>`     | Rejoue les commits interactivement pour modifier l’historique avant de fusionner.          |
| `git cherry-pick <commit>`   | Applique un commit spécifique d’une autre branche dans la branche actuelle.                |
| `git merge --abort`          | Annule une fusion en cours en cas de conflits.                                             |
| `git rebase --abort`         | Annule un rebase en cours.                                                                 |

## Commandes pour gérer les dépôts distants et le travail collaboratif

| Commande                     | Description                                                                        |
|------------------------------|------------------------------------------------------------------------------------|
| `git remote add <nom> <url>` | Ajoute un dépôt distant avec un nom donné (souvent `origin`).                      |
| `git fetch <nom>`            | Récupère les modifications du dépôt distant sans les intégrer.                     |
| `git pull`                   | Récupère et fusionne les modifications du dépôt distant dans la branche actuelle.  |
| `git push`                   | Envoie les modifications locales vers le dépôt distant.                            |
| `git push --force`           | Force l’envoi des modifications (utile après un `commit --amend` ou un `rebase`).  |

## Commandes pour revenir sur un commit ou le modifier

| Commande                        | Description                                                                                  |
|---------------------------------|----------------------------------------------------------------------------------------------|
| `git reset --soft <commit>`     | Revenir à un commit en gardant les modifications dans l'index (prêtes à être re-committées). |
| `git reset --mixed <commit>`    | Revenir à un commit en gardant les modifications dans le répertoire de travail.              |
| `git reset --hard <commit>`     | Revenir à un commit en supprimant toutes les modifications.                                  |
| `git revert <commit>`           | Crée un nouveau commit qui annule les changements d’un commit spécifique.                    |

## Commandes de gestion des logs et de l’historique

| Commande                     | Description                                                                                      |
|------------------------------|--------------------------------------------------------------------------------------------------|
| `git log`                    | Affiche l’historique des commits.                                                                |
| `git log --oneline`          | Affiche l’historique des commits en une seule ligne par commit (mode condensé).                  |
| `git reflog`                 | Affiche l'historique de toutes les références (commits, reset, etc.), même ceux supprimés.       |
| `git blame <fichier>`        | Affiche l’historique ligne par ligne d’un fichier, utile pour voir qui a fait chaque changement. |

## Commandes de modification des commits

| Commande                                      | Description                                                                                                               |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| `git commit --amend`                          | Modifie le dernier commit en incluant les changements ajoutés à l’index.                                                  |
| `git commit --amend --no-edit`                | Modifie le dernier commit sans changer le message de commit (utile pour ajouter des fichiers à l'index).                  |
| `git commit --amend -m "<nouveau message>"`   | Modifie le message du dernier commit sans toucher aux autres éléments (ajout de fichiers, auteur, etc.).                  |
| `git commit --amend --date "<nouvelle date>"` | Modifie la date du dernier commit (utilisé pour corriger une date incorrecte).                                            |
| `git commit --amend --author "<Nom <email>>"` | Modifie l’auteur du dernier commit (utile pour corriger le nom ou l'email de l’auteur).                                   |


## Commandes `stash`

| Commande                     | Description                                                                             |
|------------------------------|-----------------------------------------------------------------------------------------|
| `git stash`                  | Enregistre temporairement les modifications non committées et vide l'espace de travail. |
| `git stash list`             | Affiche la liste des modifications sauvegardées (stash).                                |
| `git stash apply`            | Réapplique le dernier stash sans le supprimer de la liste.                              |
| `git stash pop`              | Réapplique le dernier stash et le supprime de la liste.                                 |
| `git stash drop`             | Supprime un stash spécifique.                                                           |
| `git stash clear`            | Supprime tous les stashes enregistrés.                                                  |
