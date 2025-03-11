# Web

## Chall 1 : Cloud

Dans l'idée, créer une instance EC2 sur AWS, qui aurait une snapshot publique --> point d'entrée 
Il faudrait donc créer un volume pour loader la snapshot, faire de l'inspection locale, et remonter à des git logs présents uniquement sur cette snapshot, dans lequel le flag serait caché
lien utile : https://hackingthe.cloud/aws/enumeration/loot_public_ebs_snapshots/

## Chall 2 : log4j 

Petite application web qui utilise une version non patchée de log4j 
--> POC de log4shell
Lien utile : https://github.com/kozmer/log4j-shell-poc

## Chall 3 : XSS, SQLi ou CSRF 

à voir comment le rendre original, mais dans l'idée : une injection peu commune, genre une XSS dans un pdf à upload ou quelque chose d'assez tordu comme ça
Lien utile : https://github.com/pimcore/admin-ui-classic-bundle/security/advisories/GHSA-jfxw-6c5v-c42f
