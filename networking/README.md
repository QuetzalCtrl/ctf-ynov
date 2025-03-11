# Networking
### Challenge : Analyse de Fichiers PCAP via FTP

#### **Objectif :**
Les participants doivent accéder à un serveur FTP en mode anonyme, télécharger des fichiers PCAP contenant des captures de trafic VoIP et des trames réseau, puis analyser ces fichiers pour trouver des flags cachés.

#### **Description du Challenge :**

1. **Le Serveur FTP**
        Le serveur FTP aura donc un accès anonyme ou trouvable par crackage de password (read-only).

    **Contiendra :**
        **Le fichier PCAP Trames Réseau :**
            Contiendra une partie du flag hashé 
        **Le fichier PCAP VOIP:**
            Contiendra une partie du flag en clair 
        **Script aidant au déhashage d'un des flags**
            Pourra s'appuyer sur les données des 2 flags pour donner une pseudo clé de déhashage ...

2. **Analyse des Fichiers PCAP :**
        Cette tache consistera à analyser les différents fichiers contenant les fragments de flags .
        
        Les 2 fichiers PCAP seront donc indissociables pour la recherche d'information.

        L'utilisation de Wireshark avec ses différents modes d'analyse de trame seront donc bien utile.

3. **Décodage du fichier PCAP**
        L'utilitaire permettra de déhasher correctement le flag en donnant des indices pour atteindre l'objectif final.
#### **Instructions pour les Participants :**

1. **Connexion au Serveur FTP :**
   - Utilisez un client FTP (comme FileZilla ou la ligne de commande) pour vous connecter à `ftp.example.com` en mode anonyme.
   - Téléchargez les fichiers `voip_capture.pcap` et `network_capture.pcap`.

2. **Analyse du Fichier `voip_capture.pcap` :**
   - Ouvrez le fichier `voip_capture.pcap` avec Wireshark.
   - Utilisez les filtres de Wireshark pour isoler les flux VoIP (par exemple, `sip` ou `rtp`).
   - Écoutez les conversations VoIP pour trouver un flag caché dans les dialogues.
   - **Exemple de flag** : `FLAG{VOIP_SECRET_MESSAGE}`

3. **Analyse du Fichier `network_capture.pcap` :**
   - Ouvrez le fichier `network_capture.pcap` avec Wireshark.
   - Utilisez les filtres de Wireshark pour isoler les trames HTTP ou DNS (par exemple, `http` ou `dns`).
   - Analysez les données pour trouver un flag caché dans les requêtes ou réponses.
   - **Exemple de flag** : `FLAG{NETWORK_SECRET_DATA}`

- **But de ce challenge** :
  - Recherchez des anomalies ou des données inhabituelles dans les trames HTTP ou DNS qui pourraient indiquer la présence d'un flag.
  - Mettre en application des méthodes de déhashage afin d'obtenir l'objectif final
