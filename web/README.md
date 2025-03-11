# Web

## Chall 1 : Cloud

Dans l'idée, créer une instance EC2 sur AWS, qui aurait une snapshot publique --> point d'entrée 
Il faudrait donc créer un volume pour loader la snapshot, faire de l'inspection locale, et remonter à des git logs présents uniquement sur cette snapshot, dans lequel le flag serait caché

## Chall 2 : log4j 

Petite application web qui utilise une version non patchée de log4j 
--> POC de log4shell

## Chall 3 : XSS, SQLi ou CSRF 

à voir comment le rendre original, mais dans l'idée : une injection peu commune, genre une XSS dans un pdf à upload ou quelque chose d'assez tordu comme ça