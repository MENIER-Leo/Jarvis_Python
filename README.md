# Jarvis HUD

Jarvis HUD est un assistant vocal de bureau développé en Python. J'ai créé ce projet pour pouvoir lancer mes applications, contrôler ma musique et faire des recherches rapides sur Wikipédia à la voix, sans avoir à quitter ma fenêtre active ou utiliser `Alt+Tab` quand une application tourne en plein écran.
L'assistant s'affiche sous la forme d'un petit bandeau noir et cyan (style HUD) qui reste maintenu au premier plan sur l'écran pour afficher les réponses.

## Fonctionnement

* **Reconnaissance vocale :** Le script écoute le micro en continu, calibre le bruit ambiant et transcrit les phrases en français via l'API Google Speech Recognition (avec une limite de 7 secondes par commande).
* **Interface :** Tkinter gère mal les boucles infinies sur le thread principal donc toute la partie écoute et traitement tourne en arrière-plan dans un thread séparé (`daemon`). Les textes sont renvoyés vers la fenêtre graphique via `fenetre.after()` pour éviter que l'interface ne fige.
* **Recherche Wikipédia :** Lorsqu'une question commence par *"Qui est"*, *"C'est quoi"* ou *"Recherche"*, le programme interroge directement l'API REST de Wikipédia en français (`requests`) et affiche un résumé de 4 phrases dans le HUD.
* **Contrôle du PC :** Le script utilise les modules `os`, `webbrowser` et `pyautogui` pour ouvrir des sites web, simuler des raccourcis clavier (comme `Ctrl+W` ou la touche `Play/Pause`) et fermer des processus Windows via `taskkill`.

## Prérequis et installation

Le projet est conçu pour tourner sous Windows avec Python 3.

1. Cloner le dépôt :
```bash
git clone [https://github.com/MENIER-Leo/jarvis-hud.git](https://github.com/MENIER-Leo/jarvis-hud.git)
cd jarvis-hud
```

2. Installer les bibliothèques nécessaires :
```bash
pip install SpeechRecognition pyautogui requests pyaudio
```
*(Tkinter est normalement déjà inclus par défaut avec Python sur Windows).*

3. Lancer le script :
```bash
python main.py
```

## Commandes prises en charge

* **Recherches :** *"Recherche [sujet]"*, *"Qui est [sujet]"*, *"C'est quoi [sujet]"*, *"Parle-moi de [sujet]"*
* **Musique (Spotify) :** *"Lance Spotify"*, *"Lance ma playlist Spotify"*, *"Ferme Spotify"*
* **Navigation web :** *"Ouvre YouTube"*, *"Ferme l'onglet"*
* **Applications :** *"Lance Valorant"*, *"Ferme Valorant"*
* **Système :** *"Bonjour"*, *"Merci"*, *"Au revoir"*, *"Quitter"*, *"Arrête-toi"*

## Bibliothèques utilisées

* `tkinter` : Interface graphique (HUD maintenu au premier plan avec l'attribut `-topmost`)
* `speech_recognition` : Capture micro et conversion Speech-to-Text
* `threading` : Exécution parallèle de la boucle d'écoute et de l'interface
* `requests` : Appels HTTP vers l'API MediaWiki (Wikipédia) et lecture de la réponse JSON
* `pyautogui`, `os`, `webbrowser` : Automatisation des actions clavier et gestion des processus

## Améliorations prévues

* Déplacer les chemins d'accès locaux et les liens web dans un fichier de configuration externe (`config.json`) pour éviter de les modifier dans le code.
* Ajouter un mot-clé d'activation (*wake word*) pour que l'assistant n'exécute une commande que lorsqu'on l'appelle.
* Ajouter une option de synthèse vocale (Text-to-Speech) en complément de l'affichage à l'écran.
