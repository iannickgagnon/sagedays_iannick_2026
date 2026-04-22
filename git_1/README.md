# Vue d’ensemble du flux de travail Git

Cet atelier s'oriente autour du flux de travail Git illustré dans le diagramme ci-dessous.

---

## Diagramme

```mermaid
flowchart LR

    classDef stash fill:#f3e8ff,stroke:#7e22ce,stroke-width:2px,color:#111111;
    classDef work fill:#f3f4f6,stroke:#6b7280,stroke-width:2px,color:#111111;
    classDef stage fill:#ede9fe,stroke:#7c3aed,stroke-width:2px,color:#111111;
    classDef local fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#111111;
    classDef remote fill:#ffedd5,stroke:#ea580c,stroke-width:2px,color:#111111;

    ST[Stash]:::stash
    WD[Working Directory]:::work
    SA[Staging Area]:::stage
    LR[Local Repository]:::local
    RR[Remote Repository]:::remote

    WD -->|add| SA
    SA -->|commit| LR
    LR -->|push| RR
    RR -->|fetch| LR
    RR -->|pull| WD

    LR -->|checkout| WD
    LR -->|merge| WD
    LR -->|rebase| WD
    LR -->|revert| WD

    WD -->|diff| SA

    WD -->|stash| ST
    ST -->|unstash| WD

    linkStyle 0 stroke:#7c3aed,stroke-width:2px;
    linkStyle 1 stroke:#0284c7,stroke-width:2px;
    linkStyle 2 stroke:#ea580c,stroke-width:2px;
    linkStyle 3 stroke:#0f766e,stroke-width:2px;
    linkStyle 4 stroke:#dc2626,stroke-width:2px;

    linkStyle 5 stroke:#2563eb,stroke-width:2px;
    linkStyle 6 stroke:#0891b2,stroke-width:2px;
    linkStyle 7 stroke:#0891b2,stroke-width:2px;
    linkStyle 8 stroke:#0891b2,stroke-width:2px;

    linkStyle 9 stroke:#6b7280,stroke-width:2px,stroke-dasharray: 5 5;

    linkStyle 10 stroke:#7e22ce,stroke-width:2px;
    linkStyle 11 stroke:#7e22ce,stroke-width:2px,stroke-dasharray: 5 5;
```

---

## 1 - Que représentent les rectangles ?

Le diagramme est organisé autour de cinq régions, chacune représentant **un endroit où les changements peuvent exister à un moment donné** dans Git.

| Rectangle | Description |
|---|---|
| `Stash` | Zone **temporaire** où Git peut mettre de côté des modifications non terminées pour les récupérer plus tard. |
| `Working Directory` | Fichiers tels qu’ils existent actuellement sur le disque (**navigateur de projet de votre IDE**). |
| `Staging Area` | Zone **intermédiaire** où l’on prépare précisément ce qui fera partie du prochain `git commit`. |
| `Local Repository` | Contient **l’historique Git enregistré localement sur la machine**, notamment les commits et les branches locales. |
| `Remote Repository` | Dépôt distant, par exemple sur **GitHub**, qui permet de partager et de synchroniser le projet. |

---

## 2 - Git != GitHub

Il est commun de confondre **Git** et **GitHub**, mais ce sont deux choses différentes.

**Git est un logiciel** de gestion de versions ou *version control system* (VCS) qui s'installe sur votre machine.
C'est lui qui permet de suivre l'historique d'un projet, de créer des commits, de gérer des branches, etc.

**GitHub est un service d'hébergement distant muni d'une interface web**. Dans le diagramme ci-dessus, il correspond uniquement a la région `Remote Repository`. Il sert a partager et à synchroniser le projet, notamment dans le cadre d'un travail collaboratif.

GitHub est très populaire, mais ce n'est **pas la seule option**. Il existe plusieurs alternatives, par exemple :
- **GitLab** ([https://about.gitlab.com/](https://about.gitlab.com/))
- **Bitbucket** ([https://bitbucket.org/product/](https://bitbucket.org/product/))
- **l'auto-hébergement** sur un serveur que l'on contrôle soi-même ([https://about.gitea.com/](https://about.gitea.com/))

En résumé:
- **Git** = l'outil
- **GitHub** = un service en ligne qui peut héberger un dépôt Git

---

## 3 - Installation

Git doit être installé sur votre machine pour pouvoir être utilisé en ligne de commande. L'installation dépend du système d'exploitation. **Il est généralement préinstallé sur macOs et Linux (`git --version`)**.

S'il n'est pas installé, suivez les instructions spécifiques à votre OS ici: [https://git-scm.com/install/](https://git-scm.com/install/).

### 3.1 - Vérification

Une fois l'installation terminée, vérifiez que Git est disponible avec la commande suivante (votre résultat pourrait être différent) :

```
> git --version
git version 2.46.2.windows.1
```

### 3.2 - Configuration initiale

Il est fortement recommandé de configurer son nom et son adresse courriel avant de commencer à faire des commits :

```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
```

Vous pouvez utiliser un nom (`user.name`) de votre choix, mais assurez-vous qu'il vous identifie clairement, car personne n'aime travailler avec `MasterHacker1337`. Sinon, il est important d'utiliser le même courriel (`user.email`) qui est associé a votre compte GitHub, afin que vos commits soient correctement reliés à votre profil ([exemple](https://github.com/iannickgagnon)).

---

## 5 - Premiers pas

Cette partie se concentre sur la dernière région, soit le `Remote Repository`, puisque cela nous permettra de visualiser le flux de travail Git à travers l'interface web de GitHub.

Nous allons:
1. Créer un dépôt distant ([lien](https://docs.github.com/fr/enterprise-cloud@latest/repositories/creating-and-managing-repositories/quickstart-for-repositories)).
2. Créer un fichier nommé `README.md` et y insérer du contenu à partir de l'interface web de GitHub
3. En faire une copie locale dans `Local Repository` (`git clone`)
4. Le modifier dans le `Working Directory`
5. Le déplacer vers le `Staging Area` (`git add`)
6. L'enregistrer dans le `Local Repository` (`git commit`)
7. L'enregistrer dans le `Remote Repository` (`git push`)

> [!IMPORTANT]
> Réalisez les étapes 1 et 2 avant de continuer.

### 5.1 - Étape 3: Cloner un dépôt distant

> [!NOTE]
> **Définition.** Cloner un dépôt consiste à créer une copie locale d'un dépôt distant (`Local Repository` ⟵ `Remote Repository`).

La commande correspondant est la suivante (**référence:** [https://git-scm.com/docs/git-clone](https://git-scm.com/docs/git-clone)):
```markdown
git clone <url_du_depot> [nom_dossier]
```

L'`url_du_depot` est l'adresse menant à la racine de votre dépôt et ressemble généralement à `https://github.com/<votre_nom>/<nom_du_depot>` (avec ou sans `.git` à la fin). Le nom du dossier (`nom_dossier`) correspond au nom du dossier **local** qui contiendra le dépôt cloné.

Après cette opération, l'état actuel est le suivant :
1. Le `Stash` est vide.
2. Le `Working Directory` contient les fichiers clonés.
3. Le `Staging Area` est "vide" (c'est-à-dire déjà à jour avec le `Local Repository`).
4. Les données du `Local Repository` sont stockées dans un dossier caché nommé `.git/`.

> [!IMPORTANT]
> Ouvrez `README.md`, modifiez-le, puis sauvegardez. Passez ensuite à la prochaine étape.

### 5.2 - Étape 5: Déplacer vers le `Staging Area`

> [!NOTE]
> **Définition.** Déplacer des modifications vers le `Staging Area` consiste à indiquer à Git quels changements du `Working Directory` doivent faire partie du prochain commit (`Working Directory` ⟶ `Staging Area`).

La commande correspondante est la suivante (**référence:** [https://git-scm.com/docs/git-add](https://git-scm.com/docs/git-add)):
```markdown
git add <fichier_ou_dossier>
```

Le paramètre `fichier_ou_dossier` désigne l'élément dont on souhaite préparer les modifications. Il peut s'agir d'un fichier précis, d'un dossier, ou encore de `.` pour désigner le dossier courant.

> [!IMPORTANT]
> Avant d'exécuter `git add`, exécutez `git status` pour constater que le fichier modifié n'a pas encore été ajouté au `Staging Area` avec la mention `Changes not staged for commit`. Exécutez-la aussi après pour voir qu'il est ajouté avec la mention `Changes to be committed`.

Par exemple, si vous avez modifié le fichier `README.md`, vous pouvez exécuter la commande suivante :
```markdown
git add README.md
```

Après cette opération, l'état actuel est le suivant :
1. Le `Stash` est vide.
2. Le `Working Directory` contient toujours les fichiers modifiés.
3. Le `Staging Area` contient maintenant une copie instantanée des modifications préparées pour le prochain commit.
4. Le `Local Repository` n'a pas encore été modifié.

### 5.3 - Étape 6: Enregistrer dans le `Local Repository`

> [!NOTE]
> **Définition.** Enregistrer des modifications dans le `Local Repository` consiste à créer un nouveau commit à partir du contenu actuel du `Staging Area` (`Staging Area` ⟶ `Local Repository`).

La commande correspondante est la suivante (**référence:** [https://git-scm.com/docs/git-commit](https://git-scm.com/docs/git-commit)):
```markdown
git commit -m "message du commit"
```

Le paramètre `message du commit` est une courte description des changements enregistrés. Ce message doit être suffisamment clair pour permettre de comprendre rapidement la nature du commit dans l'historique du projet.

Par exemple, si vous venez de préparer les modifications apportées à `README.md`, vous pouvez exécuter la commande suivante :
```markdown
git commit -m "Modifie README.md"
```

Après cette opération, l'état actuel est le suivant :
1. Le `Stash` est vide.
2. Le `Working Directory` est à nouveau "propre" si aucun autre changement n'a été effectué depuis le `git add`.
3. Le `Staging Area` est à jour avec le nouveau commit ou "vide".
4. Le `Local Repository` contient maintenant un nouveau point dans l'historique.

> [!IMPORTANT]
> Exécutez `git commit -m "docs: Modifie README.md"`, puis observez le résultat avec `git log --oneline`.

### 5.4 - Étape 7: Enregistrer dans le `Remote Repository`

> [!NOTE]
> **Définition.** Enregistrer des modifications dans le `Remote Repository` consiste à envoyer les commits du `Local Repository` vers le dépôt distant (`Local Repository` ⟶ `Remote Repository`).

La commande correspondante est la suivante (**référence:** [https://git-scm.com/docs/git-push](https://git-scm.com/docs/git-push)):
```markdown
git push -u origin main
```

Après cette opération, l'état actuel est le suivant :
1. Le `Stash` est vide.
2. Le `Working Directory` est propre si aucun autre changement n'a été effectué.
3. Le `Staging Area` est à jour avec le `Local Repository`.
4. Le `Local Repository` et le `Remote Repository` contiennent maintenant le même nouveau commit.

> [!IMPORTANT]
> Exécutez `git push -u origin main`, puis vérifiez sur GitHub que votre commit apparaît bien dans le dépôt distant.

## 6 - Introduction aux branches

La commande `git push -u origin main` de la section précédente envoie les commits de la **branche** locale `main` située dans le `Local Repository`) vers le dépôt distant nommé `origin` (c.-à-d. celui sur GitHub). Nous n'avons par encore précisé ce qu'est une branche.

> [!NOTE]
> **Définition.** Une branche est un nom donné a une ligne de développement. Elle permet de faire des commits sans modifier directement une autre ligne de developpement, comme `main`.

<p align="center">
  <img src="assets/images/image_1_branche_simple.drawio.svg">
  <br>
</p>
<p align="center"><strong>Figure 1 - Schéma de branches simples</strong></p>

Dans la **Figure 1**, il y a deux branches: `main` et `dev`. La branche `main` est généralement créée par défaut. Dans d'anciens projets, on peut toutefois rencontrer `master`. La branche `dev` est une ligne de développement appart (p. ex. vous travaillez sur un *feature*) qui  contient les noeuds `A-B`, alors que la branche `main` contient les noeuds `A-C-D`. Meme si deux commits mènent a des fichiers semblables (p. ex. `B` et `D`), ils restent differents en Git dès que leurs historiques diffèrent.

Pour revenir à l'exemple de départ, nous avons :
- `origin` : Désigne le dépôt distant configuré automatiquement lors du `git clone`.
- `main` : Désigne la branche locale que l'on souhaite envoyer. Dans notre cas, il s'agit de `main`.
- `-u` : Établit un lien de suivi entre la branche locale et la branche distante correspondante (la prochaine fois,  nous pouvons simplement faire `git push`).

> [!NOTE]
> Le terme `origin` est parfois agaçant au début, mais il n'est rien de plus qu'un surnom choisi par Git, un peu comme le nom d'une variable. Il permet de référencer plus facilement l'adresse complète du dépôt distant. D'ailleurs, vous pouvez remplacer `origin` par l'adresse complète, mais c'est plutôt verbeux et vous comprendrez rapidement pourquoi un alias est utile.

Cela dit, il est important de savoir que cette commande est un raccourci pour la suivante : `git push origin main:main`. La voici dans sa forme générale : 
```markdown
git push origin <source_locale>:<destination_distante>
```
Autrement dit, nous faisons un `push` des commits de la branche locale `main` vers la branchge `main` du dépôt distant `origin`.

## 7 - Mise en oeuvre des branches

Dans cette section, nous allons:
1. Créer une branche nommée `dev` et s'y déplacer (`git checkout`).
2. Créer un nouveau fichier qui contient du code nommé `main.py`
3. Le déplacer dans le `Staging Area` (`git add`)
4. L'enregistrer dans le `Local Repository` (`git commit`)
5. L'enregistrer dans le `Remote Repository` (`git push`)
6. Fusionner la branche `dev` avec la branche `main` (`git merge`).

### 7.1 - Étape 1 : Créer une branche `dev` et s'y déplacer

> [!NOTE]
> **Définition.** Créer une branche consiste à ajouter un nouveau nom de branche dans le `Local Repository`, généralement à partir du commit courant. Se déplacer sur cette branche consiste ensuite à faire en sorte que le `Working Directory` reflète l'état de cette branche.

La commande correspondante est la suivante (**référence :** [https://git-scm.com/docs/git-checkout](https://git-scm.com/docs/git-checkout)) :
```markdown
git checkout -b dev
```

Cette commande effectue deux opérations à la fois :
1. Elle crée une nouvelle branche **locale** nommée `dev`;
2. Elle vous déplace immédiatement sur cette branche.

Vous pouvez observer le résultat comme suit:
```bash
> git branch
* main # ⟵ vous êtes sur la branche 'main' 
> git checkout -b dev
> git branch
* dev # ⟵ vous êtes maintenant sur la branche 'dev'
  main
```

Autrement dit, au lieu de rester sur `main`, vous commencez maintenant à travailler sur une ligne de développement distincte nommée `dev`.

Après cette opération, l'état actuel est le suivant :
1. Le `Stash` est vide.
2. Le `Working Directory` contient les mêmes fichiers qu'avant, puisque la branche `dev` vient d'être créée à partir du commit courant.
3. Le `Staging Area` est inchangé.
4. **Le `Local Repository` contient maintenant deux branches : `main` et `dev`.**
5. **La branche courante est maintenant `dev`.**

> [!IMPORTANT]
> Réalisez les étapes 2, 3 et 4 avant de continuer.

### 7.2 - Étape 5 : L'enregistrer dans le `Remote Repository`

La commande correspondante est la suivante (**référence :** [https://git-scm.com/docs/git-push](https://git-scm.com/docs/git-push)) :
```markdown
git push -u origin dev
```

Cette commande effectue ici trois opérations importantes plutôt que deux comme dans l'exemple précédent :
1. Elle crée une nouvelle branche `dev` sur le dépôt distant si elle n'existe pas encore.
2. Elle envoie les commits de la branche locale `dev` vers le dépôt distant `origin`.
3. Elle établit un lien de suivi entre la branche locale `dev` et la branche distante `dev`.

Vous pouvez observer le résultat comme suit :
```markdown
git push -u origin dev
git branch -v
```

L'option `-v` veut dire "verbeux" et affiche les commits les plus récents sur chacune des branches ainsi que leurs messages. L'option `-vv` veut dire *very verbose* ou "très verbeux" et affiche plus d'information encore.







## Légende

- 🟣 **Violet** : préparation ou mise de côté des changements (`add`, `stash`, `unstash`)
- 🔵 **Bleu** : enregistrement local (`commit`, `checkout`)
- 🟦 **Bleu-cyan** : intégration ou transformation de l’historique (`merge`, `rebase`, `revert`)
- 🟠 **Orange** : envoi vers le dépôt distant (`push`)
- 🟢 **Vert-bleuté** : récupération depuis le dépôt distant (`fetch`)
- 🔴 **Rouge** : mise à jour du répertoire de travail depuis le distant (`pull`)
- ⚪ **Gris pointillé** : comparaison (`diff`)

---

## Les zones du diagramme

## 🟣 Stash

Le **stash** est une zone temporaire où l’on peut mettre de côté des modifications non terminées sans créer de commit.

Cela est utile lorsque :

- on veut changer rapidement de tâche ;
- on veut récupérer ou intégrer autre chose avant de continuer ;
- on ne veut pas encore enregistrer officiellement son travail dans l’historique Git.

### Diagramme ciblé

```mermaid
flowchart LR

    classDef active fill:#f3e8ff,stroke:#7e22ce,stroke-width:2px,color:#111111;
    classDef activeWork fill:#ffffff,stroke:#7e22ce,stroke-width:2px,color:#111111;
    classDef faded fill:#f3f4f6,stroke:#d1d5db,stroke-width:1px,color:#9ca3af;

    ST[Stash]:::active
    WD[Working Directory]:::activeWork
    SA[Staging Area]:::faded
    LR[Local Repository]:::faded
    RR[Remote Repository]:::faded

    WD -->|stash| ST
    ST -->|unstash| WD

    WD -->|add| SA
    SA -->|commit| LR
    LR -->|push| RR
    RR -->|fetch| LR
    RR -->|pull| WD
    LR -->|checkout| WD
    LR -->|merge| WD
    LR -->|rebase| WD
    LR -->|revert| WD
    WD -.->|diff| SA

    linkStyle 0 stroke:#7e22ce,stroke-width:2px;
    linkStyle 1 stroke:#7e22ce,stroke-width:2px,stroke-dasharray: 5 5;

    linkStyle 2 stroke:#d1d5db,stroke-width:1px;
    linkStyle 3 stroke:#d1d5db,stroke-width:1px;
    linkStyle 4 stroke:#d1d5db,stroke-width:1px;
    linkStyle 5 stroke:#d1d5db,stroke-width:1px;
    linkStyle 6 stroke:#d1d5db,stroke-width:1px;
    linkStyle 7 stroke:#d1d5db,stroke-width:1px;
    linkStyle 8 stroke:#d1d5db,stroke-width:1px;
    linkStyle 9 stroke:#d1d5db,stroke-width:1px;
    linkStyle 10 stroke:#d1d5db,stroke-width:1px,stroke-dasharray: 5 5;
```

### Commandes utiles

```bash
git stash
git stash apply
git stash pop
```

### Idée à retenir

- `git stash` enlève temporairement des modifications du **working directory** et les met dans le **stash** ;
- `git stash apply` les remet dans le working directory ;
- `git stash pop` les remet aussi, puis retire l’entrée du stash.

### Exemple

```bash
git stash
git pull
git stash pop
```

Dans cet exemple :

1. on met les changements locaux de côté ;
2. on met à jour le projet ;
3. on réapplique les changements mis en attente.

---

### ⬜ Working Directory

Le **working directory** correspond aux fichiers tels qu’ils existent actuellement sur le disque.  
C’est l’espace dans lequel on modifie réellement le code, la documentation ou les ressources du projet.

C’est généralement le point de départ de la plupart des actions Git :

- on modifie des fichiers ;
- on les compare ;
- on les ajoute à l’index ;
- on peut aussi les mettre temporairement dans le stash.

---

### 🟣 Staging Area

La **staging area** (ou **index**) sert à préparer le prochain commit.

Elle permet de choisir précisément ce qui sera enregistré dans l’historique local.  
Autrement dit, elle agit comme une zone intermédiaire entre les fichiers modifiés et le commit final.

---

### 🟦 Local Repository

Le **local repository** contient l’historique Git enregistré localement sur la machine.

C’est là que se trouvent :

- les commits ;
- les branches locales ;
- l’état versionné du projet connu localement.

---

### 🟧 Remote Repository

Le **remote repository** est le dépôt distant, par exemple sur GitHub.

Il sert à :

- partager le projet avec d’autres personnes ;
- centraliser l’historique ;
- synchroniser le travail entre plusieurs machines ou plusieurs collaborateurs.

---

## 🟣 Ajouter des changements : `add`

### Commande
```bash
git add <fichier>
```

### Rôle
La commande `add` prend des modifications présentes dans le **working directory** et les place dans la **staging area**.

### Idée à retenir
On ne commit pas directement depuis les fichiers modifiés.  
On commence par sélectionner ce que l’on veut inclure dans le prochain commit.

### Exemple
```bash
git add README.md
```

---

## 🔵 Enregistrer dans l’historique local : `commit`

### Commande
```bash
git commit -m "Mon message"
```

### Rôle
La commande `commit` prend ce qui se trouve dans la **staging area** et l’enregistre dans le **local repository**.

### Idée à retenir
Le commit crée un point d’historique local contenant un état précis du projet.

### Exemple
```bash
git commit -m "Ajout d'une explication du workflow Git"
```

---

## 🟠 Envoyer vers le dépôt distant : `push`

### Commande
```bash
git push
```

### Rôle
La commande `push` envoie les commits du **local repository** vers le **remote repository**.

### Idée à retenir
Tant qu’on n’a pas fait `push`, les commits restent seulement sur la machine locale.

---

## 🟢 Récupérer depuis le distant : `fetch`

### Commande
```bash
git fetch
```

### Rôle
La commande `fetch` récupère les nouvelles informations du **remote repository** vers le **local repository**, sans modifier directement les fichiers du working directory.

### Idée à retenir
`fetch` met à jour ce que Git sait du dépôt distant, mais ne fusionne pas automatiquement ces changements dans les fichiers de travail.

---

## 🔴 Mettre à jour les fichiers de travail : `pull`

### Commande
```bash
git pull
```

### Rôle
Dans le schéma présenté ici, `pull` est représenté comme une action qui apporte le contenu du **remote repository** jusque dans le **working directory**.

### Idée à retenir
Pédagogiquement, cela signifie que `pull` met à jour les fichiers sur lesquels on travaille.

### Remarque
En interne, Git passe aussi par le dépôt local, mais cette simplification rend le diagramme plus facile à lire.

---

## 🔵 Revenir à un état connu : `checkout`

### Commande
```bash
git checkout <branche>
```

### Rôle
Dans ce diagramme, `checkout` part du **local repository** et met à jour le **working directory**.

### Idée à retenir
`checkout` permet de replacer les fichiers de travail dans un état déjà connu par Git, par exemple celui d’une branche ou d’un commit.

### Exemple
```bash
git checkout main
```

---

## 🟦 Intégrer ou transformer l’historique local

Ces commandes partent du **local repository** et ont un impact visible sur le **working directory** dans le schéma.

---

### 🟦 `merge`

#### Commande
```bash
git merge <branche>
```

#### Rôle
`merge` fusionne deux historiques.

#### Idée à retenir
On combine les changements d’une autre branche avec la branche courante.

---

### 🟦 `rebase`

#### Commande
```bash
git rebase <branche>
```

#### Rôle
`rebase` réapplique des commits sur une nouvelle base.

#### Idée à retenir
Cela permet souvent d’obtenir un historique plus linéaire que `merge`.

---

### 🟦 `revert`

#### Commande
```bash
git revert <commit>
```

#### Rôle
`revert` crée un nouveau commit qui annule les effets d’un commit précédent.

#### Idée à retenir
On corrige l’historique sans le réécrire.

---

## ⚪ Comparer des états : `diff`

### Commande
```bash
git diff
```

### Rôle
Dans ce diagramme, `diff` est représenté entre le **working directory** et la **staging area**.

### Idée à retenir
Cela permet de voir ce qui a changé.

### Remarque
Git permet d’autres comparaisons aussi, mais cette représentation garde le diagramme simple.

---

## 🟣 Mettre de côté temporairement : `stash`

### Commande
```bash
git stash
```

### Rôle
`stash` prend des modifications du **working directory** et les place dans la zone **stash**.

### Idée à retenir
C’est utile quand on veut interrompre son travail sans faire de commit.

---

## 🟣 Restaurer ce qui a été mis de côté : `unstash`

### Commandes possibles
```bash
git stash apply
```

ou

```bash
git stash pop
```

### Rôle
Cette opération ramène les changements stockés dans le **stash** vers le **working directory**.

### Idée à retenir
On reprend plus tard un travail mis en pause.

---

## Scénario typique

Voici un scénario simple correspondant au diagramme :

1. Je modifie un fichier dans le **working directory**.
2. J’utilise `git diff` pour voir les changements.
3. J’utilise `git add` pour envoyer les changements dans la **staging area**.
4. J’utilise `git commit` pour enregistrer ces changements dans le **local repository**.
5. J’utilise `git push` pour envoyer mes commits dans le **remote repository**.

Autre scénario :

1. Je ne suis pas prêt à faire un commit.
2. J’utilise `git stash` pour mettre mes changements de côté.
3. Je fais ce que j’ai à faire ailleurs.
4. J’utilise `git stash apply` ou `git stash pop` pour récupérer mon travail.

---

## Résumé rapide

| Zone | Rôle |
|---|---|
| Stash | Mettre temporairement des changements de côté |
| Working Directory | Modifier les fichiers |
| Staging Area | Préparer le prochain commit |
| Local Repository | Conserver l’historique local |
| Remote Repository | Partager et synchroniser le projet |

| Commande | Déplacement simplifié |
|---|---|
| `add` | Working Directory → Staging Area |
| `commit` | Staging Area → Local Repository |
| `push` | Local Repository → Remote Repository |
| `fetch` | Remote Repository → Local Repository |
| `pull` | Remote Repository → Working Directory |
| `checkout` | Local Repository → Working Directory |
| `merge` | Local Repository → Working Directory |
| `rebase` | Local Repository → Working Directory |
| `revert` | Local Repository → Working Directory |
| `diff` | Working Directory ↔ Staging Area |
| `stash` | Working Directory → Stash |
| `unstash` | Stash → Working Directory |

---

## Note pédagogique

Ce diagramme est volontairement **simplifié**.  
Son but est d’aider à comprendre les grandes directions du flux Git sans entrer immédiatement dans tous les détails internes.

Certaines commandes, comme `pull`, `checkout` ou `diff`, peuvent être décrites plus finement dans une explication avancée.  
Ici, elles sont représentées de manière à rester lisibles et utiles pour l’apprentissage.
