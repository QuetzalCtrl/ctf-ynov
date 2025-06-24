# Networking & Crypto

## Challenge : Analyse Réseau et Audio

### Mission

Votre équipe d’analystes en cybersécurité vous a fourni des fichiers issus d’une capture réseau suspecte.  
Votre objectif : retrouver les informations cachées pour résoudre ce challenge !

> Ce challenge a été développé en 2025 dans le cadre du projet de sécurité réseau,  
> avec la référence technique **CTF_WIRED_2025** pour l’équipe d’investigation numérique.

---

### Fichiers fournis

- **network_traces.pcap** : Trafic réseau avec une partie du flag cachée.
- **flag_audio.wav** : Fichier audio contenant l’autre partie du flag.

---

### Méthode conseillée

1. **Analyser** le fichier PCAP pour identifier les informations utiles (ports, adresses, etc.).
2. **Écouter** le fichier audio pour obtenir des indices supplémentaires.
3. **Rassembler** toutes les informations pour reconstituer le flag.

---

### Objectif

Trouver les deux parties du flag et les assembler pour obtenir le flag complet.

---

### Outils recommandés

- [Wireshark](https://www.wireshark.org/)
- Un lecteur audio (VLC, Windows Media Player, etc.)

---

### /!\ Notes

- Certaines données peuvent être **hashées**.
- Vous pouvez vérifier le hash avec la partie 2 du flag et le salt.
- La **corrélation des données** entre les fichiers est essentielle pour résoudre ce challenge.

---

### Indices

- Une partie du flag est cachée dans les **ports sources** du trafic réseau.
- L’autre partie est **prononcée dans le fichier audio**.
- Un **code secret** (le salt) est mentionné dans la description du challenge.

---

Bonne chance !
