#TP Virtualisation type 2 avec qemu
##Xingshuo CHA et Andi WANG INFO5
##1.1 Installation de Qemu
###1.1.1 Sur une distribution MAC OS
Pour faire ce TP de virtualisation, nous avon utilisé la machine Mac.
D'abord on installe le `QEMU` sur la machine.
![install qemu](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/brew-install-qemu.png)
On peut voir que le QEMU a déjà installé sur la machine.
###1.1.2 Premiers tests 
####1.1.2.1 Construction d’une image avec qemu-img
Pour créer un image en format de qcow2, on utilise la commande `qemu-img create -f qcow2 16g.qcow2 16G`.
Et puis on peut utiliser la commande `qwmu-img info 16g.qcow2` pour voir les informations sur cet image.

![install qemu](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/premiere-test.png)
On trouve que la taille du 16g.qcow2 est 196kb, c'est a dire que le 16G est une taille virtuelle.
####1.1.2.2 Cas pratique : construction d’une image de démarrage Debian Buster
On commence par créer un répertoire dans lequel on installera une image de base, et puis télécharger le code d’amorçage.
![wget](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/wget-netboot.png)
Voir le résultat de la commande.
![tftpboot](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/tftpboot.png)

Maintenant on peut démarrer une machine virtuelle Qemu au moyen de la commande `qemu-system-x86_64` avec les options `-machine -cpu -m -drive -boot -device`.

![qemu-system-x86_64](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step16.png)
Il ensuite entrer les démarches de configuration.
Il y a des images enregistrer quelque étape de la configuration.
![step1](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step1.png)
![step2](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step2.png)
![step3](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step3.png)
![step4](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step4.png)
![step5](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step5.png)
![step6](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step6.png)
![step7](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step7.png)
![step8](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step8.png)
![step9](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step9.png)
![step10](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step10.png)
![step11](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step11.png)
![step12](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step12.png)
![step13](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step13.png)
Justqu'à ici, tout est bon.
Mais il tousjour échoue pendant les installations des logicielles. 
Pour trouver la solution du problème, on entre le shell et voir le contenu de `/var/log/syslog`.
![step14](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step14.png)
On trouve que c'est parce que il n'y a pas assez de espace disque. Donc on changer la taille virtuelle de la fichier 16g.qcow2.
![step15](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step15.png)

Et puis refaire la commande `qemu-system-x86_64`.

![step16](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step16.png)
![step17](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step17.png)
![step18](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step18.png)
![step19](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step19.png)
Cet fois il a réussit à installer l'image qcow2.
Après l'installation de l'image du Debian, on revoir la taille du fichier 16g.qcow2.
![step191](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step191.png)
Maintenant il est de 1.6G, c'est la taille de l'image qu'on a configuré et installé depuis internet.

Pour lancer la machine vituelle, on utilise aussi la commande `qemu-system-x86_64 -machine q35,accel=hvf -cpu host -m 512m -drive file=buster00.qcow2,if=virtio,index=0,snapshot=on` mais pas de option `-device`.

![step20](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step20.png)
On peut voir que la machie vituelle a réussit à démarrer.
![step21](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step21.png)
![step22](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step22.png)
On peut voir que la taille de disque est de 3G et la taille de mémoire est de 512M.
La taille de disque est défini dans le fichier 16.qcow2, ce que on ne peut changer plus.
Mais on peut configurer la taille de mémoire par l'option -m de la commande `qemu-system-x86_64`.

Cet fois on met l'option `-m 256m` et revoir le résultat de la commande `df -h` et `free -h`.

![step23](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step23.png)
On voir bien que la taille de mémoire peut être configurer comme on a besoins. Il est donc plus flexible que VirtuelBox.

Pour mettre en place une console sur port série, on changer le contenu du fichier `/etc/default/grub`.
![step24](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step24.png)

Et puis `update-grub`.

![step25](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step25.png)
###1.1.3 Benchmarks
####1.1.3.1 Mesures de performances d’entrées-sorties
Après l'installation du `Bonnie++`, on fait un mesure de performances d’entrées-sorties
![step26](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/step26.png)
