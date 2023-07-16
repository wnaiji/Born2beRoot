# Born2beRoot
Administration Système

Mise en place d’une VM avec des règles stricte….

Les plus et les moins de Rocky(ex CentOS) et Debian

Rocky:

- Rocky est un clone de CentOS 100% stable
- Distributeur Linux pour les entreprises
- Compatible avec tout les paquet Red Hat
-          - Red Hat (Red Hat Package Manager) RPM est un système de gestion de paquet de logiciels utilisé sur certaines distributions Linux
- Les mises à jour prennent du temps ce qui les rends stables
- Un bon remplacent de CentOS et est par une  croissante et dynamique
-   - Jeune par rapport aux autres distributions stables... 

Debian:

- Debian a toujours au moins trois versions activement entretenues (nous allons rester sur la versions stable)
- La versions stables:
-    - La versions 11 (BULLSEYE) la dernière mise à jour est la 11.6, publiée le 17 décembre 2022
- Le cycle de vie des distributions 
-    - Les utilisateurs peuvent s’attendre à une prise en charge complète pendant trois années pour chaque publication et deux années supplémentaires avec LTS (Long Term Support - Prise en charge à long terme)

Différence entre SELinux et AppArmor

Bon à savoir:

 ——DAC:
		Contrôle d’accès discrétionnaire (Discretionary Access Control)
	Est un moyens de limiter l’accès aux objets basés sur l’identité des sujets ou groupes auxquels ils appartiennent.

——MAC:
		Contrôle d’accès obligatoire (Mantatory access control)
	Est une méthode de gestion des droits des utilisateurs pour l’usage de systèmes d’information.

SELinux:

- SELinux (Security-Enhanced Linux) est une architecture de sécurité pour systèmes Linux
- Permet aux administrateurs de mieux contrôler les accès au système 
- Permet aussi de Définir une politique de contrôle d’accès obligatoire aux éléments d’un système issu de linux

    —Fonctionnement:

SELinux fonctionne comme un système d’étiquetage : chaque fichier, chaque processus et chaque port du système possède une étiquette SELinux.
Les étiquettes constituent un moyen logique de regrouper plusieurs éléments.

Les étiquettes sont rédigées au format suivant : user:role:type:level
Les éléments “user”,”role” et “level” sont utilisés pour des mises en oeuvre de SELinux plus complexes. Le type d’etiquette est essentiel pour la politique ciblée.

SELinux utilise l’application de types pour appliquer un politique définie sur le système. L’application de types est l’élément de la politique SELinux qui détermine si un processus qui est exécuté avec un type donné est autorisé à accéder à un fichier étiqueté avec un type donné.

Conclusion:

Les politiques SELinux permettent de gérer les accès de manière spécifique et couvrent un grand nombre de processus. Vous pouvez apporter des modifications avec SELinux pour limiter l’accès entre utilisateurs, fichiers, répertoires et plus encore.

AppArmor:

- AppArmor est un logiciel de sécurité pour Linux
- Permet à l’administrateur système d’associer à chaque programme un profil de sécurité qui restreint ses accès au système d’exploitation.
- Il complète le DAC en permettant d’utiliser le MAC
- Mode d’apprentissage: toute les transgressions au profil sont enregistrées, mais pas empêchées.
- Identifie les objets du système par leur chemin 

Conclusion SELinux AppArmor:

AppArmor a été créé en partie comme une alternative à SELinux, critiqué pour être difficile à paramétrer et à maintenir par les administrateurs.
A la différence de SELinux, qui s’appuie sur l’application d’indicateurs aux fichiers, AppArmor travaille avec les chemins. 
Il est donc moins complexe et plus facile pour l’utilisateur moyen que d’apprendre SELinux.
AppArmor demande moins de modifications pour fonctionner avec les systèmes existants

LVM:

Introduction:

LVM (Logical Volume Manager) permet la création et la gestion de volumes logiques sous Linux. L’utilisation de volumes logiques remplace en quelque sorte le partitionment des disques. C’est un système beaucoup plus souple, qui permet par exemple de diminuer la taille d’un système de fichier pour pouvoir en agrandir un autre, sans se préoccuper de leur emplacement sur le disque.

Les plus:

- Il n’y a pas de limitations
- On ne se préoccupe plus de l’emplacement exact des données.
- On peut conserver quelques giga-octets de libres pour pouvoir leas ajouter n’importe où n’importe quand
- Les operations de redimensionnement deviennent quasiment sans risques

Les moins:

- Si un des volumes physiques devient HS, alors c’est l’ensemble des volumes logiques qui utilisent ce volume physique qui sont perdus


Qu’est ce que “Sudo”:

Sudo (Super User DO) “Faire en tant que super-utilisateur”
Cette commande permet à un administrateur système d’accorder à certains utilisateurs (ou groupes d’utilisateurs) la possibilité de lancer une commande en tant qu’administrateur, ou en tant qu’autre utilisateur, tout en conservant une trace des commandes saisies et des arguments.

Utilisation:

Sert à exécuter, en mode super utilisateur, des commandes ou des applications en console
	- $ sudo <commande>
Un mot de passe est demandé (celui de l’utilisateur courant)
Il est mémorisé pour une durée de 15 minutes, au terme de ce laps de temps, il faut entrer de nouveau le mot de passe.
Pour terminer sudo avant la fin des 15 minutes
	- $ sudo -k
La commande suivante permet d’être connecté en tant que root
	- $ sudo -I

Configuration:

Les autorisations sudo sont définies dans le fichier /etc/sudoers
Ce fichier doit être modifié à l’aide de la commande “visudo”
Car elle vérifie, avant écriture du fichier, que sa syntaxe est correcte.
Ps: une erreur dans le fichier peut bloquer définitivement l’accès à la machine.

Ce fichier précise quelles commandes peuvent être utilisées par quels utilisateurs avec les privilèges de quel utilisateur et s’il peut s’abstenir de taper son mot de passe.
	- $ man sudoers


Se connecter en SSH sur Debian 11:

Installation SSH:

1er mise à jour:
	- $ sudo apt update
	- $ sudo apt upgrade -y

Installation du paquet Openssh
	- $ sudo apt install opens-server

Activation et démarrage du service ssh:
	- $ sudo systemctl enable ssh
	- $ sudo systemctl start ssh

Depuis une autre machine
	- $ ssh <nomd’utilisateur>@<ip-machine>

Autoriser la connection avec l’utilisateur root:

Pour activer l’authentification avec une clé publique dans SSH,
Il faut éditer le fichier de configuration du serveur SSH.
	- $ sudo vim /etc/ssh/sshd_config
Il faut modifier la ligne PubkeyAuthentication, en retirant le commentaire et en mettant comme valeur: “yes”
	- PubkeyAuthentication yes
Puis redémarrer le service SSH:
	- $ sudi systemctl restart ssh


Installation et Utilisation de UFW:

1er mise à jour:
	- $ sudo apt update
	- $ sudo apt upgrade -y

Installation UFW:
	- $ sudo apt install ufw

Ajouter les protocoles que vous souhaitez autoriser:
	- $ sudo ufw allow ssh
	- $ sudo ufw allow http
	- $ sudo ufw allow https
	…
	- $ sudo ufw allow 80
Pour ne pas bloquer l’utilisation de SSH pour un contrôle à distance, ne surtout pas oublier de l’ajouter.

Une fois votre configuration prête vous pouvez saisir la commande suivante pour activer UFW:
	- $ sudo ufw enable

Puis vous pouvez consulter votre configuration
	- $ sudo ufw status

Vous pouvez aussi autoriser les accès à la machine uniquement depuis une machine avec cette commande
	- $ sudo ufw allow from 192.168.0.201

Il est aussi possible d’autoriser une seul machine à utiliser qu’un seul port avec cette commande:
	- $ sudo ufw allow from 192.168.0.201 to any port 4242

APT et Aptitude, quelle est la vraie différence entre eux?:

Introduction:

	Aptitude et apt-get sont deux des outils populaires de gestion de packages. Les deux sont capables de gérer toutes sortes d’activités sur les paquets, y compris l’installation, la suppression, la recherche, etc. Cependant, il existe toujours des différences entre les deux outils, qui font que les utilisateurs préfèrent l’un ou l’autre.

Qu’est ce que Apt?:

Apt (Advanced Packaging Tool) best un logiciel libre et à code source ouvert qui gère gracieusement l’installation et la suppression du logiciel.

Apt est une ligne de commande complète sans interface graphique. Chaque fois qu’il est appelé à partir de la ligne de commande en spécifiant le nom du paquet à installer, il le trouve dans la liste configurée des sources spécifiée dans ‘/etc/apt/sources.list’ ainsi que dans la liste des dépendances pour ce paquet et les trie et les installe automatiquement avec le paquet actuel.

Il est très flexible et permet à l’utilisateur de contrôler facilement diverse configurations, telles que: ajouter toute nouvelle source pour rechercher des paquets, apt-pinning, c’est à dire mettre à niveau les packages les plus importants et laisser les moins importants.

Qu’est ce que Aptitude?:

Aptitude est un outil de packaging avancé qui ajoute une interface utilisateur à la fonctionnalité, permettant ainsi à l’utilisateur de rechercher un package de manière interactive, puis de l’installer ou de le supprimer. 

Son interface utilisateur est basée sur la bibliothèque “nurses”, qui lui ajoute divers éléments, couramment vus dans les interfaces graphiques.
L’un des points forts est qu’il peut émuler la plupart des arguments de ligne de commande d’apt-get.

Au total, Aptitude est un gestionnaire de packages de niveau supérieur qui résume les détails de bas niveau. Il peut fonctionner en mode interface utilisateur interactive à base de texte et même en mode non interactif en ligne de commande.

En conclusion:

Outre la différence principale, Aptitude est un gestionnaire de packages de haut niveau, tandis que Apt est un gestionnaire de packages de niveau inférieur.
Apt-get, est limité à la ligne de commande, tandis qu’Aptitude, possède une interface interactive.

PAM (Pluggable Authentication Modules):

Définition:

PAM est un mécanisme permettant d’intégrer différents schémas d’authentification de bas niveau dans une API de haut niveau, permettant de ce fait de rendre indépendants du schéma les logiciels réclamant une authentification.

En Général:

La mise en place d’une nouvelle méthode d’authentification ne doit pas nécessiter de modifications dans la configuration ou dans le code source d’un programme ou d’un service.

C’est pourquoi les applications s’appuient sur PAM, qui va leur fournir les primitives nécessaires à l’authentification de leurs utilisateurs.

L’administrateur d’un système peut choisir exactement sa politique d’authentification pour une seule application (par exemple pour durcir le service SSH) indépendamment de celle-ci.

A chaque application ou service prenant en charge PAM correspondra un fichier de configuration dans le repertoire “/etc/pam.d”
Par exemple, le processus “login” attribue le nom “/etc/pam.d/login” à son fichier de configuration.

Warning:

Une mauvaise configuration de PAM peut compromettre toute la sécurité de votre système.
PAM est un système d’authentification (gestion des mots de passe). Si PAM est vulnérable alors l’ensemble du système est vulnérable.

Pam_cracklib:

Le module pam_cracklib permet de tester les mots de passe.

Utilize la bibliothèque crackle pour vérifier la solidité d’un nouveau mot de passe. Il peut également vérifier que le nouveau mot de passe n’est pas construit à partir de l’ancien. Il ne concerne que le mécanisme password.

Arguments:

Retry=n		nombre de test possible
Minlen=n		longer minima du MDP
Difok=n		nombre de caractères différents de l’ancien MDP
Dcredit=-n	impose un MDP contenant au moins n chiffres
Ucredit=-n	impose un MDP contenant au moins n majuscules
Lcredit=-n	impose un MDP contenant au moins n minuscules
Ocredit=-n	impose un MDP contenant au moins n caractères spéciaux
Maxrepeat=n	hombre max de répétition
Usercheck=	check is le MDP contient le nom d’utilisateur
Reject_username	rejet ke nom d’utilisateur
enfore_for_root	même règle pour root.


Qu’est ce que Crontab:

Introduction:

Crontab est un outil qui permet de lancer des applications de façon régulière.

Installation:

Apt-get install cron

Configuration:

Pour être autorisé à utiliser la commande crontab, il faut que l’utilisateur soit présent dans le groupe cron.

Les fichier /etc/cron.allow et /etc/cron.deny permettent de définir les droits d’utilisation sur crontab.

Commande crontab:

Afficher le contenu du fichier crontab:
-$ crontab -l

Supprimer toutes les actions du fichier crontab:
-$ crontab -r

Editer les actions du fichier crontab:
-$ crontab -e

Syntaxes:

Code bash:
mm hh jj mmm JJJ [user] tache > log

mm:		minutes (00-59)
hh:		heures (00-23)
jj:		jours du mois (01-31)
MMM:	mois (01-12 ou jan, feb, mar, apr,…)
JJJ:		jour de la semaine (1-7 ou mon, tue, wed,…)
user: 	nom d’utilisateur avec lequel exécuter la tâche.
> log:	redirection de la sortie vers un fichier de log

Utilisations des unités:

- 1-5: Les unités de temps de 1 à 5
- */6: toutes les 6 unités de temps (toutes les 6 heures par exemple)
- 2,7: les unités de temps 2 et 7
