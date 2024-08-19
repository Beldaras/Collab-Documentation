------

Version : 0.2
Auteur(s) : Roblot Jean-Philippe

------

## Review Philippe :
- L'emplacement + SHA1 ou **MD5** pour dire qu'un processus est ok
- Pour certains processus, même avec le bon ou mauvais emplacement et MD5, il faut regarder ce qui est fait avec pour savoir si legit ou non
- compléter les emplacements, son rôle, son éditeur, si il est signé
- ==voir site process library ou file.net==
- ==Voir Lolbas==, mettre les binaires correspondants ou voir pour intégrer le tableau de Lolbas
- Pour conhost, corriger "plus d'une dizaine de conhost" par "cmd", c'est cmd qu'il faudra contrôler
- CSRSS est un service, lui aussi lié à CMD
- séparer les services actif all time de ceux appelés par une app ou commande
- Pour ceux qui ne sont pas toujours présents, qu'est ce qui les lance ??
- Vérifier ce qu'est Local Service (compte utilisateur particulier à connaître)
- préciser CompPkgSrv.exe
- expliquer ce qu'est COM
- Dllhost, svchost, rundll32 => voir si les dll chargées sont toute legit pour savoir si malicieux
- dwm => corriger l'utilisation anormal du CPU, c'est un non sens, un virus ne consomme pas de CPU car est censé rester discret
- Explorer => corriger processus parent de tout ce qui est lancé en cliquant
- LsaIso = > protection de sécurité de lsass, permet stocker des objets à protéger dans une pseudo VM pour isoler accessible seulement par windows (Credential Guard)
- svchost héberge le code utilisé par les services, c'est service.exe qui gère les services en eux-même. Il faut regarder sa ligne de commande => à approfondir
- MsEdge Redirect ???
- ntoskernel pas visible dans la liste des processus => trouver l'explication : ==**apparait sous le nom system**==
- Regedit.exe ou Regedit32.exit ou regedt32.exe
- Ordinateur \HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node => Sous-sytème windows pour rétrocomp 32 bits
- compléter ce que fait runtime broker
- Sedsvc  win10 ET Win11, à compléter
- SettingSyncHost => à compléter
- SmartScreen => trouver son nouveau nom : Smart App Control
- Intégrer l'usage de process explorer
# Processus Système Windows

Cette partie a pour objectif de savoir si un processus ou un fichier est légitime ou pas.
## Emplacements :
Connaître les emplacements des fichiers systèmes Windows aide à ne pas faire d’erreurs et dissocier les vrais processus légitimes Windows, des processus malveillants.  
L’emplacement générique étant C:\Windows\system32

L’emplacement joue beaucoup, ainsi :

- `C:\Windows\system32\svchost.exe` est légitime
- `C:\Windows\system\svchost.exe` est malveillant
- `C:\Users\<users>\AppData\svchost.exe` est aussi malveillant.

Windows embarque aussi des applications Windows Store.  
Les applications Windows Store en natif (Edge, Cortana, etc) se trouvent dans `C:\Windows\SystemApps\`

Pour voir la liste des processus en cours :
- ctrl+ alt + supp pour ouvrir le gestionnaire des tâches.
- [Process Explorer](https://www.malekal.com/process-explorer-gestionnaire-taches-avance/), un gestionnaire de tâches avancé proposé par [SysInternals](https://www.malekal.com/meilleurs-outils-sysinternals/)
## Liste

- Audiodg.exe – Audio Device Graph Isolation 
	- Editeur : Microsoft Corporation
	- Localisation : `C:\Windows\System32`
	- Description : Il s’agit d’un processus protégé introduit avec Windows Vista et Server 2008 qui héberge le moteur audio séparément du service audio Windows hébergé par « svchost.exe ».

- ApplicationFrameHost.exe – Application Frame Host
	- Editeur : Microsoft Corporation
	- Signature : Oui
	- Localisation : `C:\Windows\System32` 
	- Description : introduit avec Windows 10, une instance de celui-ci est appelée pour s’exécuter en arrière-plan chaque fois qu’une ou plusieurs applications Windows (ou applications du Windows Store) sont actives.

- AggregatorHost – Microsoft Aggregator Host
	- Editeur : Microsoft Corporation
	- Localisation : indéterminée
	- Description : processus système responsable de la collecte et de l’agrégation de données provenant de diverses sources. Ces données sont ensuite utilisées par d’autres applications ou services au sein du système d’exploitation Windows.

- Browser_broker.exe – Browser Broker
	- Editeur : Microsoft Corporation
	- Signature : Oui
	- Localisation : `C:\Windows\System32`
	- Description : introduit avec Windows 10, c'est un composant logiciel du navigateur Microsoft Edge. Ce dernier n'tant pas compatible avec certains sites Web nécessitant des compléments de navigateur ou des extensions ActiveX, le mode Entreprise d’Edge utilise «browser_broker.exe» afin d’appeler automatiquement IE 11 pour ces sites. 

- CastSrv.exe – Casting protocol connection listener
	- Editeur : Microsoft Corporation
	- Signature : Oui
	- Localisation : indéterminée
	- Description : fichier exécutable associé à l’écouteur de connexion de protocole de casting, qui est un service qui écoute les connexions de protocole de diffusion à partir d’autres périphériques.

- Cmd.exe – invite de commandes, ==sensible car envoie des commandes directement à Windows==
	- Editeur : Microsoft Corporation
	- Localisation : `C:\Windows\System32` ou parfois dans un sous-dossier de `C:\Windows`
	- ==Attention à WMIC== qui peut interroger le système pour obtenir des informations mais aussi changer certains paramètres.
	
- Ctfmon.exe – Chargeur CTF
	- Editeur :
	- Localisation

- [Conhost.exe](https://www.malekal.com/conhost-exe/), localisé dans `C:\Windows\system32\conhost.exe`, est un processus légitime depuis W7, visant à palier la vulnérabilité de `Csrss.exe` gérant l'affichage du CMD. 
	- Editeur :
	- Localisation
	==Si vous avez plus d’une dizaine de conhost.exe ou si vous constatez des allés et venus de conhost, cela peut être lié à une activité d’un logiciel malveillant.==
	
- [Csrss.exe – Processus d’exécution client-serveur](https://www.malekal.com/csrss-exe/),  localisé dans `C:\Windows\system32\csrss.exe`,  est un fichier système Windows critique. Ce dernier gère notamment le mode utilisateur de Windows.
	- Editeur :
	- Localisation

- [dasHost.exe (_Device Association Framework Provider Host_)](https://www.malekal.com/dashost-exe/)est un processus central officiel de Microsoft qui s’exécute sous le compte ==LOCAL SERVICE==. Ce processus sert de cadre à la connexion et à l’association de périphériques filaires et sans fil avec Windows. Un processus Device Association Framework Provider Host distinct s’affiche dans le Gestionnaire des tâches pour chaque appareil connecté. L’emplacement est : `C:\Windows\system32\dashost.exe`
	- Editeur :
	- Localisation

- CompPkgSrv.exe – Component Package Support Server
	- Editeur :
	- Localisation

- [Dllhost.exe](https://www.malekal.com/dllhost-exe/) – COM Surrogate, localisé dans `C:\Windows\system32\dllhost.exe` pèse environ 20 Ko. DLLHost (*Dynamic Link Library Host*) permet d’exécuter un objet DCOM au moyen d’un fichier DLL. Il constitue un processus de lancement d’applications et de services d’exploitation. COM est l’abréviation de *Component Object Model*. Des développeurs utilisent des objets COM pour étendre les applications. Windows l’utilise aussi en interne pour différentes fonctionnalités.
	- Editeur :
	- Localisation

- [dwm.exe – Gestionnaire de bureau de Windows](https://www.malekal.com/dwm-exe/) (Desktop Windows Manager), localisé dans `C:\Windows\system32\dwm.exe`, gère les affichages des fenêtres, il est notamment lié aux effets Aero. 
	==Des virus ou adwares peuvent générer des problèmes d’utilisation anormale du CPU par dwm.exe==
	- Editeur :
	- Localisation

- [Explorer.exe – Explorateur Windows](https://www.malekal.com/explorateur-windows-explorer-exe/), localisé dans `C:\Windows\explorer.exe`, régit le bureau de Windows. Il est le processus parent de tous les processus lancés par l’utilisateur.
	- Editeur :
	- Localisation

- Fontdrvhost.exe – Usermode Font Driver Host
	- Editeur :
	- Localisation

- HelpPane.exe (dans C:\Windows)
	- Editeur :
	- Localisation

- LsaIso.exe – Credential Guard & Key Guard
	- Editeur :
	- Localisation

- [Lsass.exe – Local Security Authority Process](https://www.malekal.com/lsass-exe/)(*Local Security Authority Process*), localisé dans `C:\Windows\system32\lsass.exe`, gère l’identification des utilisateurs Windows, que ce soit des utilisateurs locaux d’après les informations de la SAM ou des utilisateurs d’un domaine.
	- Editeur :
	- Localisation

- LockApp.exe
	- Editeur :
	- Localisation

- [LogonUI.exe](https://www.malekal.com/logonui-exe/), localisé dans `C:\Windows\System32` (14Ko), est responsable de **la création de l’interface utilisateur de connexion** à partir duquel vous vous connectez à votre système. De ce fait, le processus LogonUI.exe n’est pas en permanence en cours d’exécution.  Il ne doit donc pas être visible dans le gestionnaire de tâches de Windows.
	- Editeur :
	- Localisation

- [svchost.exe (Hôte de services)](https://www.malekal.com/svchost-exe/), gère des groupes de [services Windows](https://www.malekal.com/processus-service-windows/) qui permettent le fonctionnement de composants Windows. 
	==Un malware ou virus peut sans problème s’enregistrer dans un groupe de service svchost.exe afin de pouvoir se lancer au démarrage de Windows.==
	- Editeur :
	- Localisation

- [msiexec.exe](https://www.malekal.com/msiexec-exe/), localisé dans `C:\Windows\System32\msiexec.exe`, est associé à l’installation, la désinstallation et la gestion des packages Windows Installer (.msi).
	- Editeur :
	- Localisation

- NisSrv.exe – Microsoft Network Realtime Inspection Service
	- Editeur :
	- Localisation

- MoUsoCoreWorker.exe – MoUSO Core Worker Process
	- Editeur :
	- Localisation

- MSEdgeRedirect.exe – Widget Edge
	- Editeur :
	- Localisation

- [MsMpEng.exe](https://www.malekal.com/msmpeng-exe/) – [Antimalware Service Executable](https://www.malekal.com/antimalware-execution-service-et-forte-utilisation-cpu-disque-ou-ram-sur-windows-10/) est l'un des processus principaux de l’antivirus Windows Defender.
	- Editeur :
	- Localisation

- MsMpEngCP.exe – Antimalware Service Executable Content Process
	- Editeur :
	- Localisation

- [Ntoskrnl.exe – NT Kernel & System](https://www.malekal.com/ntoskrnl-exe/), localisé dans `C:\Windows\system32\ntoskrnl.exe`, est lié au **[Kernel Windows](https://www.malekal.com/noyau-kernel-windows/)** et est responsable de l’abstraction matérielle, de la gestion des processus et de la gestion de la mémoire.
	- Editeur :
	- Localisation

- [Ntoskrnl.exe, Ntkrnlpa.exe, Win32k.sys, hal.dll sur Windows 10](https://www.malekal.com/ntoskrnl-ntkrnlpa-exe-win32k-sys-hal-dll-windows-10/)
	- Editeur :
	- Localisation

- [RegAsm.exe](https://www.malekal.com/regasm-exe/), localisé dans `C:\Windows\Microsoft.NET\Framework\v4.0.30319\RegAsm.exe`, peut apparaître lorsqu’une application écrite en langage .NET est utilisée. 
	- Editeur :
	- Localisation
	==Beaucoup de trojan et chevaux de troie sont écrits dans ce langage et notamment les trojan RAT. En général, cela se caractérise par des plantages incessants de regAsm.exe.==

- [Regedit.exe](https://www.malekal.com/faire-recherche-base-de-registre-windows/), est l’éditeur du registre de Windows inclut par défaut.
	- Editeur :
	- Localisation

- [Regsvr32.exe](https://www.malekal.com/regsvr32-exe/), localisé dans `C:\Windows\system32\regsvr32.exe`, est un utilitaire de Windows qui permet des contrôles OLE, tels que des contrôles ActiveX, OCX et des DLL dans le Registre Windows.
	- Editeur :
	- Localisation
	- La version 64 bits est `%systemroot%\System32\regsvr32.exe`
	- La version 32 bits est `%systemroot%\SysWoW64\regsvr32.exe`
	==Regsvr32 servant à enregistrer des fichiers DLL, ce dernier peut-être utilisé pour enregistrer ou exécuter des fichiers malveillants.== La commande se voit alors souvent associer le paramètre `/s` . Les malwares fileless peuvent aussi utiliser regsvr32, on peut alors voir des processus regsvr32 tourner. Enfin les familles Emotet, Quakbot, Icedid et bien d’autres utilisent aussi regsvr32 pour charger une DLL malveillante à travers [les LNK malware](https://www.malekal.com/lnk-malware/) .

- [Rundll32.exe](https://www.malekal.com/rundll32-exe/), localisé dans `C:\Windows\System32` ou `C:\Windows\syswow64\rundll32.exe`, permet, en utilisant un argument, de lancer un fichier DLL. 
	- Editeur :
	- Localisation
	==S’il se trouve dans un autre emplacement, le fichier rundll32.exe probablement malveillant. Mais surtout, un malware peut utiliser rundll32.exe pour charger un fichier DLL malveillant. Il convient alors de vérifier la ligne de commandes et la cible.==

- [Runtimebroker.exe – Runtime Broker](https://www.malekal.com/runtime-broker-exe/), localisé dans `C:\Windows\system32`, est responsable des autorisations des applications Windows Store
	- Editeur :
	- Localisation

- [SearchIndexer.exe](https://www.malekal.com/searchindexer-exe/), localisé dans `C:\Windows\system32\SearchIndexer.exe`, est lié à l'indexation des fichiers et à Windows Search
	- Editeur :
	- Localisation

- SearchUI.exe
	- Editeur :
	- Localisation

- SecurityHealthService.exe
	- Editeur :
	- Localisation

- [SecurityHealthSystray.exe](https://www.malekal.com/securityhealthsystray-exe/), localisé dans `C:\Windows\system32`, est un composant de Windows Defender. Il est en charge de l’affichage de l’icône bouclier en bas à droite de Windows qui donne accès au Sécurité Windows.  
  Son utilisation mémoire est donc relativement baisse, à peine 944 ko, et la taille du fichier est de 260 Ko.
	- Editeur :
	- Localisation
  
- [Sedsvc.exe et sedlauncher.exe sur Windows 10](https://www.malekal.com/sedsvc-exe-sedlauncher-exe-windows-10/), localisés dans `C:\Program Files\rempl`, sont des processus systèmes de Windows apparus lors de la mise à jour KB4023057. Ils sont liés aux MAJ de Win10 et ont pour objectif de prévenir des problèmes d’installation de mises à jour de Windows 10.
	- Editeur :
	- Localisation

- Services.exe – Applications Services et contrôleur
	- Editeur :
	- Localisation

- SettingSyncHost.exe – Host Process for Setting Synchronization
	- Editeur :
	- Localisation

- SgrmBroker.exe – Service Broker du moniteur d’exécution System Guard
	- Editeur :
	- Localisation

- [Sihost.exe – Shell Infrastructure Host](https://www.malekal.com/sihost-exe/), localisé dans `C:\Windows\system32\sihost.exe`, un fichier système exécutable qui s’exécute en arrière-plan et constitue l’un des fichiers essentiels de Windows 11/10.
  Sihost.exe exécute divers processus dans Windows 11/10, notamment le démarrage et le lancement du menu contextuel, du centre d’action, etc.
	- Editeur :
	- Localisation
  
- ShellExperienceHost.exe – Windows Shell Experience Host
	- Editeur :
	- Localisation

- [Smartscreen.exe – Windows Defender SmartScreen](https://www.malekal.com/smartscreen/)est un composant de Defender et aide à protéger votre PC contre du contenu malveillants et attaques WEB. Il se présente globalement de deux manières :
	- **un filtre de réputation URL (Web Reputation)** qui protège l’accès des navigateurs WEB à des sites malveillants (virus, phishing) etc)
	- un filtre de réputation de fichiers qui bloque **l’exécution de fichiers malveillants** ou non sûrs.
	- Editeur :
	- Localisation
	
- Smss.exe – Gestionnaire de sessions Windows
	- Editeur :
	- Localisation

- SpeechRuntime.exe (dans C:\Windows\System32\Speech_OneCore\Common)
	- Editeur :
	- Localisation

- StartMenuExperienceHost.exe
	- Editeur :
	- Localisation

- [Spoolsv.exe – Application sous-système spouleur](https://www.malekal.com/le-service-spouler-spooler-impression-de-windows-comment-le-vider-reparer/), localisé dans `c:\windows\system32\spool`, met en spoule les travaux d’impression et gère l’interaction avec l’imprimante.
	- Editeur :
	- Localisation

- [Svchost.exe](https://www.malekal.com/svchost-exe/) gère des groupes de [services Windows](https://www.malekal.com/processus-service-windows/) qui permettent le fonctionnement de composants Windows.  
  Par exemple le Client DNS, le partage de fichiers, le pare-feu de Windows.
	- Editeur :
	- Localisation
  ==Un malware ou virus peut sans problème s’enregistrer dans un groupe de service svchost.exe afin de pouvoir se lancer au démarrage de Windows.==
  
- SystemSettingsBroker.exe
	- Editeur :
	- Localisation

- SystemSettings.exe (dans C:\Windows\ImmersiveControlPanel)
	- Editeur :
	- Localisation

- [Taskeng.exe – Moteur du planificateur de tâches](https://www.malekal.com/taskeng-exe/), localisé dans `C:\Windows\system32\taskeng.exe`, est le moteur de tâches plannifiées de Windows.
	- Editeur :
	- Localisation
	==Des malwares peuvent installer des tâches planifiées afin de se lancer dans Windows.  Parfois, cela peut provoquer l’ouverture de fenêtre noire CMD.==

- Taskhost.exe (Windows 7)
	- Editeur :
	- Localisation

- [Taskhostw.exe – Processus hôte pour Tâches Windows](https://www.malekal.com/taskhostw-exe-processus-hote-pour-taches-windows/), localisé dans `C:\Windows\System32`, est un processus Windows qui s’exécute en tant que protocole hôte dans le gestionnaire des tâches. Les services qui se chargent à partir de fichiers Dynamic Linked Library (DLL) plutôt qu’à partir de fichiers EXE ne peuvent pas se constituer en processus à part entière. Au lieu de cela, le processus hôte pour les tâches Windows doit servir d’hôte pour ce service.  
  La taille du fichier est d’environ 80 ko à 111 ko selon la version de Windows.
	- Editeur :
	- Localisation
  
- [Taskmgr.exe – Gestionnaire de tâches de Windows](https://www.malekal.com/le-gestionnaire-de-taches-de-windows/)est le gestionnaire de tâches de Windows
	- Editeur :
	- Localisation

- [TextInputHost.exe](https://www.malekal.com/textinputhost-exe/), localisé dans `C:\Windows\SystemApps\MicrosoftWindows.Client.CBS_cw5n1h2txyewy`, est utilisé par le clavier virtuel de Windows (osk), le clavier virtuel d’émoticons, ou encore lors de la recherche dans le menu Démarrer de Windows.
  La taille du fichier est de 40 ko environ et il utilise environ 40 Mo de mémoire.
	- Editeur :
	- Localisation
  
- [Tiworker.exe ou Windows Module Installer Worker](https://www.malekal.com/tiworker-exe/), localisé dans `C:\Windows\system32\tiworker.exe`,  est un fichier système lié à Windows Modules Installer ou programme d’installation pour les modules Windows en français.
	- Editeur :
	- Localisation

- [Trustedinstaller.exe – TrustedInstaller](https://www.malekal.com/trustedinstaller-suppression-fonctionnement/), localisé dans `C:\Windows\servicing\trustedInstaller.exe`, permet l’installation, la modification et la suppression de mises à jour Windows et de composants facultatifs.
	- Editeur :
	- Localisation

- UserOOBEBroker.exe – User OOBE Broker
	- Editeur :
	- Localisation

- [Userinit.exe – Userinit](https://www.malekal.com/userinit-exe/),  localisé dans` C:\Windows\system32\userinit.exe`, est responsable de l’exécution des scripts de connexion, du rétablissement de la connexion réseau et du démarrage d’Explorer.exe.
	- Editeur :
	- Localisation

- [waaSMedicAgent.exe](https://www.malekal.com/waasmedicagent-exe/), localisé dans `C:\Windows\System32`, également connu sous le nom de Windows as a **Service Medic Agent (WaaSMedicSvc**), est un processus intégré qui répare les composants de Windows Update lorsqu’ils tombent en panne. La taille du fichier sur le disque est d’environ 110 ko et il utilise environ 11 Mo en mémoire.
	- Editeur :
	- Localisation

- [Wermgr.exe (Windows Error Reporting)](https://www.malekal.com/wermgr-exe/), localisé dans `C:\Windows\system32\wermgr.exe`, (ou Windows Error Reporting) est un processus qui gère les erreurs de programmes et applications afin d’envoyer des informations à Microsoft.
	- Editeur :
	- Localisation

- Wininit.exe -Application de démarrage de Windows
	- Editeur :
	- Localisation

- [Winlogon.exe – Application d’ouverture de session Windows](https://www.malekal.com/winlogon-exe/), localisé dans `C:\Windows\system32\winlogon.exe`, est en charge de l’ouverture de session utilisateurs Windows 
	- il charge le profil d’un utilisateur après son authentification (userinit.exe) ;
	- il gère l’écran de veille : sur le retour au mode normal, on peut paramétrer Winlogon pour obliger l’utilisateur à s’authentifier une nouvelle fois.
	Winlogon charge des clés du registre Windows présentes  :
	- _HKEY_Current_User\Software\Microsoft\Windows NT\CurrentVersion\Winlogon_
	- _HKEY_Local_Machine\Software\Microsoft\Windows NT\CurrentVersion\Winlogon_
	et notamment, la clé Shell qui définit le bureau Windows, par défaut, il s’agit d’explorer.exe, et la clé userinit qui charge le processus userinit.exe. Par défaut, le contenu de userinit se trouve dans : `C:\Windows\system32\userinit.exe,`.  La virgule à la fin est importante.
	- Editeur :
	- Localisation
	==Les clés Winlogon peuvent servir d’autorun, c’est à dire pour charger des logiciels  malveillants sur l’ordinateur. La seconde clé userinit est aussi modifiée par des virus, ce qui permet de charger une charge malveillante assez tôt lors de l’ouverture du bureau Windows. En outre, cette clé est aussi lue en mode sans échec, ce qui permet de rendre la charge active en mode sans échec.==
	
- wlanext.exe – Infrastructure d’extensibilité pour les services réseau Windows sans fil 802.11
	- Editeur :
	- Localisation

- [WmiPrvSE.exe (WMI Provider Host)](https://www.malekal.com/wmiprvse-exe/), localisé dans `C:\Windows\System32\wbem\wmiprvse.exe`, est lié à WMI. Il ‘agit d’une interface qui permet à Windows, des applications ou des scripts d’interroger le système pour obtenir des informations ou modifier le système.
	- Editeur :
	- Localisation

- [Wscript.exe](https://www.malekal.com/wscript-exe/), localisé dans `C:\Windows\system32\wscript.exe`, est le processus lié à Windows Script Host qui permet l’exécution d’un script VBS ou JavaScript depuis Windows 2000. La taille du fichier est d’environ 196 ko.
	- Editeur :
	- Localisation
	==Les scripts malveillants sont très utilisés pour infecter les ordinateurs, il peut s’agir :==
	- de scripts pour télécharger et installer des chevaux de troie, par exemple envoyé par mail sous la forme d’un zip. Des campagnes de ransomwares ont aussi été effectuées à travers des scripts VBS et JavaScript.
	- des infections amovibles, virus par clé USB utilisent aussi wscript
	Si vous apercevez wscript.exe qui s’exécute en permanence ou tenter de s’exécuter au démarrage de Windows, cela peut indiquer l’activité d’un logiciel malveillant.
	*/(Il est possible de **désactiver Windows Script Host**, ce qui permet d’améliorer sensiblement la protection de l’ordinateur contre les logiciels malveillants.)/*
	
- WUDFHost.exe – Windows Driver Foundation – Processus hôte de l’infrastructure de pilotes en mode utilisateur
	- Editeur :
	- Localisation

- XtuService.exe – XtuService
	- Editeur :
	- Localisation

# Services Windows

Un service Windows est un programme qui peut démarrer automatiquement lors du lancement du système d’exploitation, sans nécessiter l’intervention d’un utilisateur ou la connexion à un compte du serveur.
La gestion des services Windows se fait avec la console de service.

```Bash 
windows + R
services.msc

msconfig # Permet via l'onglet services, d'activer ou désactiver des services
gestionnaire de tâches # Onglet services
```

Certains services ont besoin d’autres services pour fonctionner.  
On appel cela les dépendances. Pour visualiser les dépendances d’un Service, sélectionnez un service en double-cliquant dessus et choisissez l’onglet “Dépendances”.
## Utilisateur AUTORITE/NT

Les services Windows utilisateurs démarrent avec le compte local.  
Mais certains services peuvent démarrer avec un compte plus élevés du type AUTORITE/NT.  
(Il s’agit d’un utilisateur système avec les permissions les plus importantes.) => ==à reformuler== car n'est pas réellement un compte utilisateur

## Les états des services

- **Nom** : vous indique le nom du service
- **État** : si ce dernier est démarré ou arrêté (modifiable)
- **Type de Démarrage** : vous indique le type démarrage du service (modifiable) :  
    - **Automatique** : Le service va alors démarré avec Windows.
    - **Manuel** : Le service ne sera pas démarrer avec Windows. Cependant l’utilisateur ou Windows peut le démarrer au besoin.
    - **Désactivé** : Comme son nom l’indique, le service est désactivé. Cela peut être intéressant de désactiver des services qui sont sensibles au niveau de la sécurité de la machine.
    - ==Il existe un quatrième==

## La commande SC

permet de gérer les services depuis le CMD
```bash
sc start <nom du service> # démarre un service
sc stop <nom du service> # arrête un service
sc delete <nom du service> # Supprime un service
sc query # Affiche les informations sur les services
```

## Service et base de registre

Un service est chargé au démarrage de Windows par le processus `svchost.exe`.  
==Voir pour trouver un exemple plus simple (Dhcp ou Wuauserv)==
Si un service est démarré directement par Windows, il se trouve dans la clef suivante :
```Bash
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\servicename

# Exemple de clé de registre de service :
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ip6fw]
 "Description"="Fournit un service de prévention d'intrusion pour un réseau domestique ou de petite entreprise."
 "DisplayName"="Pilote du pare-feu Windows IPv6"
 "ErrorControl"=dword:00000001
 "ImagePath"=hex(2):73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,64,00,\
 72,00,69,00,76,00,65,00,72,00,73,00,5c,00,69,00,70,00,36,00,66,00,77,00,2e,\
 00,73,00,79,00,73,00,00,00
 "Start"=dword:00000003
 "Type"=dword:00000001
```
### La clé start
==A compléter== 
permet de définir le type de démarrage :
- 2 = automatique
- 3 = manuel
- 4 = désactivé

## Le processus svchost.exe

Pour rappel, svchost.exe (gère des groupes de [services Windows](https://www.malekal.com/processus-service-windows/) ) instancie des services qui permettent le fonctionnement de composants Windows. Quand un service est lancé par svchost.exe, il est alors placé dans un groupe de service particulier qui est ensuite lancé par [svchost.exe](https://www.malekal.com/svchost-exe/). Une liste de ces groupes et services peut être trouvé dans la base de registre à la clef suivante :
`HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\Svchost`.
Attention, c'est service.exe qui gère à proprement parler les services, svchost est une sous-gestion
### Tasklist

Depuis le CMD, affiche une liste des processus actuellement en cours sur un ordinateur local ou un ordinateur distant.
La commande `tasklist /svc` permet de lister les services démarrés par svchost.
Lorsqu’un service est lancé par svchost, le fichier du service peut-être trouvé à la clef : `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\servicename\Parameters\ServiceDll`

------
# Sources
[malekal.com ](https://www.malekal.com/)
[File.net](https://www.file.net/)
[ProcessLibrary.com - The Online Resource For Process Information!](https://www.processlibrary.com/en/)


