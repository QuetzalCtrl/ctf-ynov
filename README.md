# ctf-ynov

Projet de développement d'un CTF pour ynov, différentes catégories seront proposées (voir ci-dessous) nous nous sommes répartis le travail comme suit : 
- Networking = Cyril
- OSINT = Adrien
- Reverse = Sam
- Web = Hugo

À la fin de ce projet, nous aimerions avoir 2 à 3 épreuves dans chaque domaine, afin de proposer un CTF complet, voici le détail de chaque séance : 
- 11 mars : Planification et répartition des tâches
- 04 avril : Travail séparé sur chacune de nos disciplines
- 22 avril : Travail séparé sur chacune de nos disciplines
- 25 avril : Travail + 1ère mise en commun de nos travaux afin d'avoir les avis partagés
- 02 juin : Présentation d'une v1 du CTF
- 03 juin : Travail séparé sur la v2 pour amélioration
- 04 juin : Travail séparé sur la v2 pour amélioration
- 24 juin : Travail + mise en commun pour v2
- 25 juin : Derniers détails du projet + préparation d'un PPT de présentation
- 26 juin : Rendu du projet 

## Categories

- [Networking](./networking/README.md) 
- [OSINT](./osint/README.md)
- [Reverse](./reverse/README.md)
- [Web](./web/README.md)

## Infrastructure

Le projet se base sur [la plateforme CTFd](https://github.com/CTFd/CTFd) pour déployer les différents challenges et l'infrastructure nécessaire. 
Pour lancer le CTF : 
```bash
docker pull quetzalctrl/ynov_ctf:v2.0
docker run -p 8000:8000 quetzalctrl/ynov_ctf:v2.0
```
Puis, accédez à votre localhost sur le port 8000 pour commencer les challenges (http://localhost:8000 -> register -> challenges) ! Bonne chance :) 

