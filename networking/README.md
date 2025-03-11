# Networking
### Challenge : Analyse de Fichiers PCAP via FTP

#### **Objectif :**
Les participants doivent accéder à un serveur FTP en mode anonyme, télécharger des fichiers PCAP contenant des captures de trafic VoIP et des trames réseau, puis analyser ces fichiers pour trouver des flags cachés.

#### **Description du Challenge :**

1. **Le Serveur FTP**
        Le serveur FTP aura donc un accès anonyme ou trouvable par crackage de password (read-only).

    **Contiendra :**
        - Le fichier PCAP Trames Réseau :
            Contiendra une partie du flag hashé 
        - Le fichier PCAP VOIP:
           Contiendra une partie du flag en clair 
        - Script aidant au déhashage d'un des flags
            Pourra s'appuyer sur les données des 2 flags pour donner une pseudo clé de déhashage ...

3. **Analyse des Fichiers PCAP :**
        Cette tache consistera à analyser les différents fichiers contenant les fragments de flags .
        Les 2 fichiers PCAP seront donc indissociables pour la recherche d'information.
        L'utilisation de Wireshark avec ses différents modes d'analyse de trame seront donc bien utile.

5. **Décodage du fichier PCAP**
        L'utilitaire permettra de déhasher correctement le flag en donnant des indices pour atteindre l'objectif final.

#### **But de ce challenge** :
  - Recherchez des anomalies ou des données inhabituelles dans les trames HTTP ou DNS qui pourraient indiquer la présence d'un flag.
  - Mettre en application des méthodes de déhashage afin d'obtenir l'objectif final
