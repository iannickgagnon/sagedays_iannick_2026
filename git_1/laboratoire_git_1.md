# Atelier - Reproduction d'un graphe Git

## Objectif

Reproduire l'historique correspondant au graphe de la **Figure 1** dans un nouveau dépôt Git.

<p align="center">
  <img src="https://github.com/iannickgagnon/sagedays_iannick_2026/blob/main/git_1/assets/images/lab_git_history.svg" width=75%>
  <br>
</p>
<p align="center"><strong>Figure 1 -  Historique Git à reproduire</strong></p>

Ce laboratoire vise à consolider les notions suivantes :
- Création de commits (`git commit`) avec messages courts et significatifs (`-m`);
- Création et changement de branche (`git branch`, `git checkout`).
- Fusion de branches (`git merge`).
- Résolution de conflits (`git merge`, `git status`, `git add`).
- Synchronisation avec un dépôt distant (`git push`, `git pull`).

## Graphe cible

Vous devez reconstruire un historique compatible avec la **Figure 1**, contenant les éléments suivants :
- Une branche `main` contenant les commits `m1`, `m2`, `m3`, `m4` et `m5`.
- Une branche `dev` partant de `m1` et contenant les commits `d1`, `d2` et `d3`.
- Une branche `fix` partant de `m3` et contenant le commit `f1`.
- Une fusion de `dev` dans `main` menant à `m4`.
- une fusion de `fix` dans `main` menant à `m5`.

## Contraintes

1. Vous devez utiliser la **ligne de commande Git** pour les opérations principales.
2. Vous devez travailler dans un **nouveau dépôt**.
3. Le dépôt doit être synchronisé avec un dépôt distant (par exemple sur GitHub).
4. Les branches `main`, `dev` et `fix` doivent exister réellement.
5. Le merge qui mène à `m4` doit provoquer **au moins un conflit**, que vous devrez résoudre manuellement.

## Consignes

Vous êtes libres de choisir le contenu exact des fichiers, à condition que l'historique final respecte la structure du graphe.

Une façon simple de procéder consiste à :
- Créer un fichier `README.md` ou `notes.txt`.
- Faire évoluer son contenu différemment sur `main`, `dev` et `fix`.
- Provoquer volontairement un conflit en modifiant les mêmes lignes sur `main` et `dev` avant le merge qui mène à `m4`.
