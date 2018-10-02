---
layout: default
title: "Installer une FreeBSD 11.2 avec Mate"
tags: freebsd
comments: true
---

## Installer une FreeBSD 11.2 avec Mate

Pour ce premier article de blog ainsi que la premiere video, je me suis posé la question suivant: <<Est-t'il possible d'installer une FreeBSD bureautique avec Mate en quelque minutes?>>. La réponse est actuallement simple, oui c'est possible. Voici comment j'ai procédé.

## InstallATION
1. J'ai commencé par récuper l'iso sur le [la page de téléchargement de FreeBSD](https://www.freebsd.org/where.html)
2. J'ai créer une machine virtuelle et j'ai démarré sur l'ISO.
3. À la première question, j'ai sélectionner <<Install>>
4. Par la suite j'ai selectionner ma disposition de clavier (Ici: <<Canadian Biligual>>)
5. J'ai mis une nom d'hôte simple: <<freebsd-mate>>
6. Par la suite on nous demande de selectionner des composantes optionel, je laisse tout par défaut.
7. Pour le formatage j'ai décider d'y aller avec ZFS (Mon choix personnel)
8. L'installation débute, ça ne prend que quelque instant.
9. Àpres l'installation ça nous demande de configurer le réseau, de configurer la zone horaire, d'activer des services au démmarrage et de configurer des options de sécuriter.
10. Ont peut par la suite créer un utilisateur, le mettre dans des groupes (n'oubliez pas de mettre <<wheel>> si vous voulez votre utiliseur d'accéder a sudo.)
11. Àpres la creation de l'utilisateur nous pouvons redémarrer le systeme.

## Mise à jour du systeme et premiere configuration
1. Au redémarrage je me suis connecter en root pour faire les première configuration du système.
2. Ont commence par mettre a jour le gestionnaire de paquet ainsi que le systeme.
    > - pkg update
    > - freebsd-update fetch install
    > - pkg upgrade
3. Par la suite j'ai installer <<bash-completion>> afin d'avoir une completion des commandes
    > - pkg install bash-completion
4. J'ai changer pour bash l'utilisateur qu'ont a créer plutot ainsi que root
    > - chsh -s /usr/local/bin/bash
    > - chsh -s /usr/local/bin/bash freebsd <-- À changer pour votre utilisateur

## La vrai grosse partie commence ici:
1. Je commence par installer Xorg ainsi que les additions invité pour Virtualbox
   > - pkg install xorg virtualbox-ose-additions
2. J'installe quelque premier outils
   > - pkg install sudo nano xdg-user-dirs zip unzip p7zip neofetch alsa-utils wget curl
3. Le support pour les imprimantes
   > - pkg install cups hplip system-config-printer
4. J'installer network-manager pour avoir une partie en interface pour le réseau
   > - pkg install networkmgr
5. Installation de Mate(Bureau) ainsi de Slim(Gestionnaire de fenetre)
   > - pkg install mate-desktop mate slim slim-themes

## La configuration des applications installé auparavant commence:
1. Nous commencons par configurer sudo
   > - visudo
   
   ont décommente cette partie:
   > %wheel ALL=(ALL) ALL
2. Ont créer le fichier .xinitrc dans le dossier de notre utilisateur (n'oubliez pas de remplacer les valeurs pour les votres)
   > - nano /home/freebsd/.xinitrc

   ont y ajoute cette ligne:
   > exec mate-session
3. Ont donne la permission a l'utilisateur
   > - chown freebsd:freebsd /home/freebsd/.xinitrc
4. Ont modifie le fichier /etc/fstab
   > - nano /etc/fstab
   
   et ajoutons cette ligne
   > proc	/proc	procfs	rw	0	0
5. Passons ensuite au fichier /usr/local/etc/doas.conf
   > - nano /usr/local/etc/doas.conf

   ajoutons c'est ligne:
   > permit nopass keepenv root
   
   > permit :wheel
   
   > permit nopass keepenv :wheel cmd netcardmgr
   
   > permit nopass keepenv :wheel cmd ifconfig
   
   > permit nopass keepenv :wheel cmd service
6. Actions maintenant tout les services nécessaires.
   > vboxguest_enable="YES"
	
   > vboxservice_enable="YES"
    
   > cupsd_enable="YES"
	
   > devfs_system_ruleset="system"
   
   > moused_enable="YES"
	
   > dbus_enable="YES"
	
   > hald_enable="YES"
	
   > slim_enable="YES"

## Dernière étape
1. Installons nos applications
   > - pkg install firefox thunderbird gimp libreoffice rhythmbox vlc
2. Nous pouvons enfin profiter de notre installation.

Pour voir le résulat final pour pouvez voir la vidéo ci-dessous.











## La vidéo reliée.

[![Box](http://img.youtube.com/vi/FB0k63bx1Sg/0.jpg)](http://www.youtube.com/watch?v=FB0k63bx1Sg)