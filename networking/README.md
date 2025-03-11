# Networking
### Challenge : Analyse de Fichiers PCAP via FTP

#### **Objectif :**
Les participants doivent accéder à un serveur FTP en mode anonyme, télécharger des fichiers PCAP contenant des captures de trafic VoIP et des trames réseau, puis analyser ces fichiers pour trouver des flags cachés.

#### **Description du Challenge :**

1. **Le Serveur FTP :**
   - Le serveur FTP aura un accès anonyme ou trouvable par crackage de mot de passe (read-only).

   **Contenu :**
   - **Fichier PCAP Trames Réseau :**
     - Contiendra une partie du flag hashé.
   - **Fichier PCAP VoIP :**
     - Contiendra une partie du flag en clair.
   - **Script d'Aide au Déhashage :**
     - Pourra s'appuyer sur les données des deux flags pour donner une pseudo-clé de déhashage.

2. **Analyse des Fichiers PCAP :**
   - Cette tâche consistera à analyser les différents fichiers contenant les fragments de flags.
   - Les deux fichiers PCAP seront donc indissociables pour la recherche d'informations.
   - L'utilisation de Wireshark avec ses différents modes d'analyse de trames sera donc très utile.

3. **Décodage du Fichier PCAP :**
   - L'utilitaire permettra de déhasher correctement le flag en donnant des indices pour atteindre l'objectif final.

#### **But de ce Challenge :**
- Rechercher des anomalies ou des données inhabituelles dans les trames HTTP ou DNS qui pourraient indiquer la présence d'un flag.
- Mettre en application des méthodes de déhashage afin d'obtenir l'objectif final.
