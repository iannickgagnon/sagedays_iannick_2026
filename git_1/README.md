# Vue d’ensemble du flux de travail Git

Ce document explique le flux de travail Git illustré dans le diagramme ci-dessous.  
Les sections suivent le même **code couleur** que les flèches du schéma afin de faciliter la lecture.

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
