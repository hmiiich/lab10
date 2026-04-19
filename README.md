#lab10-Guide d'installation de Frida /mohammed-amine hmich
LAB 10 — Frida : Explication détaillée de chaque étape
________________________________________
🎯 D'abord, comprendre ce qu'est Frida
Avant de commencer, tu dois savoir que Frida a deux parties :
•	Le client → s'installe sur ton PC (les commandes que tu tapes)
•	Le serveur → s'installe sur le téléphone Android
Les deux doivent communiquer ensemble via ADB.
________________________________________
Étape 1 — Installation du client Frida sur ton PC
Ce que tu dois faire :
Tu commences par préparer Python sur ta machine, parce que Frida est un outil Python. Avant d'installer quoi que ce soit, tu vérifies que Python est déjà installé en tapant python --version dans ton terminal. Si tu vois un numéro de version s'afficher, Python est prêt. Sinon, tu l'installes selon ton système.
Ensuite tu installes Frida avec pip. C'est simple, une seule commande suffit. Une fois installé, tu vérifies en tapant frida --version — si un numéro comme 16.x.y s'affiche, c'est bon, le client est prêt sur ton PC.
 LAB 10 — Frida : Explication détaillée de chaque étape
________________________________________
🎯 D'abord, comprendre ce qu'est Frida
Avant de commencer, tu dois savoir que Frida a deux parties :
•	Le client → s'installe sur ton PC (les commandes que tu tapes)
•	Le serveur → s'installe sur le téléphone Android
Les deux doivent communiquer ensemble via ADB.
________________________________________
Étape 1 — Installation du client Frida sur ton PC
Ce que tu dois faire :
Tu commences par préparer Python sur ta machine, parce que Frida est un outil Python. Avant d'installer quoi que ce soit, tu vérifies que Python est déjà installé en tapant python --version dans ton terminal. Si tu vois un numéro de version s'afficher, Python est prêt. Sinon, tu l'installes selon ton système.
Ensuite tu installes Frida avec pip. C'est simple, une seule commande suffit. Une fois installé, tu vérifies en tapant frida --version — si un numéro comme 16.x.y s'affiche, c'est bon, le client est prêt sur ton PC.
 
________________________________________
Étape 2 — Installation des outils Android (ADB)
Ce que tu dois faire :
ADB (Android Debug Bridge) est l'outil qui permet à ton PC de communiquer avec le téléphone ou l'émulateur. Sans ADB, Frida ne peut pas envoyer ses instructions au téléphone.
Tu l'installes sur ton PC, puis tu branches ton téléphone (ou tu lances ton émulateur). Tu tapes adb devices pour vérifier que le téléphone est bien détecté. Si tu vois emulator-5554 device ou un identifiant similaire s'afficher, la connexion est établie et tu peux passer à l'étape suivante.
 <img width="980" height="246" alt="image" src="https://github.com/user-attachments/assets/f6e8b1bb-ba44-4948-83b7-2ed99e17f156" />
________________________________________
Étape 3 — Récupérer et déployer frida-server sur Android
C:\Users\pc>adb shell getprop ro.product.cpu.abi
x86_64
Ce que tu dois faire :
C'est l'étape la plus importante. Tu dois installer la partie serveur de Frida directement sur le téléphone Android.
Premièrement, tu identifies l'architecture de ton téléphone (arm64, x86, etc.) avec la commande adb shell getprop ro.product.cpu.abi. Le résultat te dit quel fichier télécharger.
Deuxièmement, tu vas sur la page GitHub de Frida et tu télécharges le fichier frida-server qui correspond à ton architecture et à ta version de Frida (les deux versions doivent être identiques, sinon ça ne marchera pas).
Troisièmement, tu envoies ce fichier sur le téléphone avec ADB, tu lui donnes les permissions d'exécution, puis tu le lances en mode root. À ce moment-là, le serveur Frida tourne en arrière-plan sur le téléphone et attend les instructions de ton PC.
________________________________________
Étape 4 — Test de connexion depuis le PC
frida-clr-17.9.1-windows-x86_64
Ce que tu dois faire :
 <img width="980" height="566" alt="image" src="https://github.com/user-attachments/assets/243e2e1a-7f68-461b-9eb8-669030c564cf" />
<img width="980" height="488" alt="image" src="https://github.com/user-attachments/assets/5769b84c-bf8d-443b-bc17-96cd53a62430" />

 
Maintenant tu vérifies que ton PC et le téléphone se parlent bien. Tu tapes frida-ps -U sur ton PC. Si la connexion fonctionne, tu verras s'afficher la liste de toutes les applications qui tournent en ce moment sur le téléphone, avec leur nom et leur PID (numéro de processus).
présence de frida-server.
 <img width="980" height="86" alt="image" src="https://github.com/user-attachments/assets/bd9d8e47-8f7a-459e-b881-1f05c8d51089" />

Pour permettre la communication entre le PC et frida-server, configurer la redirection des ports Frida : 
Si cette liste s'affiche, félicitations — Frida est opérationnel et tu peux commencer à analyser des applications.
 <img width="980" height="724" alt="image" src="https://github.com/user-attachments/assets/1ed9658c-2ddb-4e34-858b-d55c7a533e35" />
<img width="980" height="724" alt="image" src="https://github.com/user-attachments/assets/f8f1f5ba-2e89-43f9-8436-bb1d761da897" />

________________________________________
Étape 5 — Injection minimale pour valider
Ce que tu dois faire :
C'est ton premier test réel avec Frida. Tu choisis une application simple qui tourne sur le téléphone, et tu lui injectes un petit script JavaScript de test. Ce script ne fait rien de destructif — il affiche juste un message pour confirmer que l'injection fonctionne.
Le but de cette étape est uniquement de valider que tout le pipeline fonctionne : ton PC envoie un script → frida-server le reçoit → l'app l'exécute → tu vois le résultat sur ton terminal.
________________________________________
Étape 6 — Explorer la console interactive Frida
Ce que tu dois faire :
Frida dispose d'une console interactive (comme un terminal JavaScript) qui te permet d'explorer une application en temps réel sans écrire de script à l'avance. Tu te connectes à une app en cours d'exécution et tu peux taper des commandes directement.
 <img width="980" height="302" alt="image" src="https://github.com/user-attachments/assets/1801d909-6fd9-45e1-91a3-5a8c7052eda5" />

 
Dans cette console tu peux explorer les classes Java de l'application, lister les méthodes disponibles, et commencer à comprendre comment l'app est structurée de l'intérieur. C'est comme avoir une loupe sur le code de l'app pendant qu'elle tourne.
 
 <img width="980" height="373" alt="image" src="https://github.com/user-attachments/assets/a75e8896-5227-40a5-8bd6-23ce2ce9003d" />

 <img width="980" height="681" alt="image" src="https://github.com/user-attachments/assets/6d1c5ac6-175e-4d84-98a4-9d428f8529cf" />
<img width="980" height="523" alt="image" src="https://github.com/user-attachments/assets/a408630a-0e78-439c-bb3d-ff214bfda46a" />



Rôle :
Cette commande énumère les classes Java chargées et filtre celles liées à l’application. Intérêt sécurité : 
•	repérer les classes métier ;
•	identifier les composants d’authentification, de réseau ou de stockage ;
•	mieux comprendre la surface fonctionnelle de l’application.


Vérifier la présence de bibliothèques liées au chiffrement ou au TLS
<img width="980" height="802" alt="image" src="https://github.com/user-attachments/assets/50273c98-dcfa-461b-8733-eee1637fbab9" />

 
Observer les accès au système de fichiers
Dans un contexte d’analyse de sécurité, il est également utile de vérifier si l’application ouvre des fichiers sensibles. Exemple d’idée de test : 
•	surveiller les fonctions open et read ;
•	observer si l’application accède à des fichiers de configuration, certificats, préférences locales ou bases SQLite.
Intérêt sécurité : 
•	comprendre comment l’application stocke ou lit ses données ;
•	repérer les emplacements potentiellement sensibles ;
•	relier le comportement natif au stockage local.


Vérifier les informations de base sur le processus
<img width="980" height="530" alt="image" src="https://github.com/user-attachments/assets/54ebb2b4-32e1-4d41-994b-114b1462b040" />

 
Étape 7 — Observer le chiffrement, stockage et appels réseau
Ce que tu dois faire :
Tu vas maintenant écrire des scripts Frida plus ciblés pour observer trois choses précises :
Le chiffrement : tu interceptes les appels aux fonctions de chiffrement (AES, RSA, etc.) pour voir quelles données sont chiffrées et avec quelles clés. Tu pourras voir les données avant qu'elles soient chiffrées.
Le stockage local : tu surveilles ce que l'app écrit dans SharedPreferences ou SQLite pour détecter si des données sensibles (mots de passe, tokens) sont stockées sans protection.
Les appels réseau : tu interceptes les requêtes HTTP/HTTPS pour voir ce que l'app envoie aux serveurs, y compris les headers d'authentification.
________________________________________
Étape 8 — Hooker des méthodes Java (SharedPreferences, SQLite, sécurité)
Ce que tu dois faire :
Le "hooking" c'est le cœur de Frida. Tu vas intercepter des méthodes Java spécifiques au moment où elles s'exécutent pour voir leurs paramètres et leurs résultats.
Pour SharedPreferences : tu hooks la méthode getString() pour voir toutes les valeurs lues depuis le stockage de l'app en temps réel.
Pour SQLite : tu hooks execSQL() et rawQuery() pour voir toutes les requêtes SQL que l'app exécute, ce qui te révèle la structure de la base de données.
Pour les vérifications de sécurité : tu hooks des méthodes comme la vérification root ou le SSL pinning pour les faire retourner un faux résultat — par exemple, forcer isRooted() à retourner false même si le téléphone est rooté. C'est ce qu'on appelle le bypass.


________________________________________
Étape 2 — Installation des outils Android (ADB)
Ce que tu dois faire :
ADB (Android Debug Bridge) est l'outil qui permet à ton PC de communiquer avec le téléphone ou l'émulateur. Sans ADB, Frida ne peut pas envoyer ses instructions au téléphone.
Tu l'installes sur ton PC, puis tu branches ton téléphone (ou tu lances ton émulateur). Tu tapes adb devices pour vérifier que le téléphone est bien détecté. Si tu vois emulator-5554 device ou un identifiant similaire s'afficher, la connexion est établie et tu peux passer à l'étape suivante.
 ________________________________________
Étape 3 — Récupérer et déployer frida-server sur Android
C:\Users\pc>adb shell getprop ro.product.cpu.abi
x86_64
Ce que tu dois faire :
C'est l'étape la plus importante. Tu dois installer la partie serveur de Frida directement sur le téléphone Android.
Premièrement, tu identifies l'architecture de ton téléphone (arm64, x86, etc.) avec la commande adb shell getprop ro.product.cpu.abi. Le résultat te dit quel fichier télécharger.
Deuxièmement, tu vas sur la page GitHub de Frida et tu télécharges le fichier frida-server qui correspond à ton architecture et à ta version de Frida (les deux versions doivent être identiques, sinon ça ne marchera pas).
Troisièmement, tu envoies ce fichier sur le téléphone avec ADB, tu lui donnes les permissions d'exécution, puis tu le lances en mode root. À ce moment-là, le serveur Frida tourne en arrière-plan sur le téléphone et attend les instructions de ton PC.
________________________________________
Étape 4 — Test de connexion depuis le PC
frida-clr-17.9.1-windows-x86_64
Ce que tu dois faire :
 
 
Maintenant tu vérifies que ton PC et le téléphone se parlent bien. Tu tapes frida-ps -U sur ton PC. Si la connexion fonctionne, tu verras s'afficher la liste de toutes les applications qui tournent en ce moment sur le téléphone, avec leur nom et leur PID (numéro de processus).
présence de frida-server.
 
Pour permettre la communication entre le PC et frida-server, configurer la redirection des ports Frida : 
Si cette liste s'affiche, félicitations — Frida est opérationnel et tu peux commencer à analyser des applications.
 
________________________________________
Étape 5 — Injection minimale pour valider
Ce que tu dois faire :
C'est ton premier test réel avec Frida. Tu choisis une application simple qui tourne sur le téléphone, et tu lui injectes un petit script JavaScript de test. Ce script ne fait rien de destructif — il affiche juste un message pour confirmer que l'injection fonctionne.
Le but de cette étape est uniquement de valider que tout le pipeline fonctionne : ton PC envoie un script → frida-server le reçoit → l'app l'exécute → tu vois le résultat sur ton terminal.
________________________________________
Étape 6 — Explorer la console interactive Frida
Ce que tu dois faire :
Frida dispose d'une console interactive (comme un terminal JavaScript) qui te permet d'explorer une application en temps réel sans écrire de script à l'avance. Tu te connectes à une app en cours d'exécution et tu peux taper des commandes directement.
 
Dans cette console tu peux explorer les classes Java de l'application, lister les méthodes disponibles, et commencer à comprendre comment l'app est structurée de l'intérieur. C'est comme avoir une loupe sur le code de l'app pendant qu'elle tourne.
 
 
 


Rôle :
Cette commande énumère les classes Java chargées et filtre celles liées à l’application. Intérêt sécurité : 
•	repérer les classes métier ;
•	identifier les composants d’authentification, de réseau ou de stockage ;
•	mieux comprendre la surface fonctionnelle de l’application.


Vérifier la présence de bibliothèques liées au chiffrement ou au TLS

 
Observer les accès au système de fichiers
Dans un contexte d’analyse de sécurité, il est également utile de vérifier si l’application ouvre des fichiers sensibles. Exemple d’idée de test : 
•	surveiller les fonctions open et read ;
•	observer si l’application accède à des fichiers de configuration, certificats, préférences locales ou bases SQLite.
Intérêt sécurité : 
•	comprendre comment l’application stocke ou lit ses données ;
•	repérer les emplacements potentiellement sensibles ;
•	relier le comportement natif au stockage local.


Vérifier les informations de base sur le processus

 
Étape 7 — Observer le chiffrement, stockage et appels réseau
Ce que tu dois faire :
Tu vas maintenant écrire des scripts Frida plus ciblés pour observer trois choses précises :
Le chiffrement : tu interceptes les appels aux fonctions de chiffrement (AES, RSA, etc.) pour voir quelles données sont chiffrées et avec quelles clés. Tu pourras voir les données avant qu'elles soient chiffrées.
Le stockage local : tu surveilles ce que l'app écrit dans SharedPreferences ou SQLite pour détecter si des données sensibles (mots de passe, tokens) sont stockées sans protection.
Les appels réseau : tu interceptes les requêtes HTTP/HTTPS pour voir ce que l'app envoie aux serveurs, y compris les headers d'authentification.
________________________________________
Étape 8 — Hooker des méthodes Java (SharedPreferences, SQLite, sécurité)
Ce que tu dois faire :
Le "hooking" c'est le cœur de Frida. Tu vas intercepter des méthodes Java spécifiques au moment où elles s'exécutent pour voir leurs paramètres et leurs résultats.
Pour SharedPreferences : tu hooks la méthode getString() pour voir toutes les valeurs lues depuis le stockage de l'app en temps réel.
Pour SQLite : tu hooks execSQL() et rawQuery() pour voir toutes les requêtes SQL que l'app exécute, ce qui te révèle la structure de la base de données.
Pour les vérifications de sécurité : tu hooks des méthodes comme la vérification root ou le SSL pinning pour les faire retourner un faux résultat — par exemple, forcer isRooted() à retourner false même si le téléphone est rooté. C'est ce qu'on appelle le bypass.

