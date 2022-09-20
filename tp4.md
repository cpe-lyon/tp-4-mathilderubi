# Compte-rendu TP4

## Exercice 1. Commandes de base

1. Pour mettre à jour le système on exécute les commandes
```
sudo apt update
sudo apt upgrade
```
2. Pour créer un alias des commandes précédentes, on exécute :
```
alias maj='sudo apt update | sudo apt upgrade'
```
Pour que cet alias ne soit pas perdu au prochain redémarrage il faut l'enregistrer dans le .bashrc.

3. Pour obtenir les 5 derniers paquets installés, on exécute la commande 'cat /var/log/dpkg.log'. On peut voir que les 5 derniers paquets installés sont : 
initramfs-tools, dbus:amd64, man-db, libc-bin, cryptsetup-initramfs

4. Avec la commande 'grep " install " /var/log/dpkg.log, on obtient les logiciels installés explicitement avec la commande apt install : usbmuxd, upower, thermald, libevdev2, os-prober.

5. Avec la commande `dpkg --list | wc --lines` on obtient un nombre de paquets de 611. Avec la commande `apt list --installed |grep -c 'installé' ` on obtient un résultat de 606. Cette différence s'explique par le fait que dpkg compte les lignes d'informations. Pour les exclure on exécute la commande `dpkg -l |grep "^ii"` (le chapeau veut dire "commence par un i".



7. Le paquet glances est une application qui permet d'afficher l'état des principales ressources d'un système.
Le paquet tldr convertit les pages man en explications concises.
Le paquet hollywood simule une fenêtre de hacking.

8. Les paquets gnome-sudoku, nudoku et ksudoku permettent de jouer au sudoku.

## Exercice 2. 
Grâce à la commande `dpkg -S /bin/ls`, on sait que la commande ls a été installée à partir du paquet coreutils.
On crée le fichier origine-commande et on insère les lignes suivantes :

```
#!/bin/bash

commande=$1
which -a $commande | xargs dpkg -S 2> /dev/null | cut -d":" -f1

```

Ensuite on exécute le script :
`
tp@serveur:/bin$ sudo chmod 755 origine-commande 
tp@serveur:/bin$ sudo origine-commande ls
coreutils
tp@serveur:/bin$ 
`

## Exercice 3
On crée un fichier script et on entre les lignes suivantes :
```
#!/bin/bash

function verif()
{
        verif=$(apt list -a $1)
        if [[ $verif == *"installé"* ]]; then
                echo "INSTALLE"
        else
                echo "NON INSTALLE"
        fi
}
verif $1
```
Puis on vérifie si ça fonctionne
`
tp@serveur:/bin$ isinstalle sudo

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

NON INSTALLE

`
Pour une ligne de commande seule on exécute la commande dpkg -l coreutils 2>/dev/null | grep "^ii" > /dev/null && echo "INSTALLE" || echo "NON INSTALLE" 


## Exercice 4

Pour lister les programmes qui ont été installés avec coreutils, on exécute la commande `dpkg -L coreutils`. Le programme nommé "[" est un le programme qui permet de faire des tests.

## Exercice 5

