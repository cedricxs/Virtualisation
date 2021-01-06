# TP Virtualisation Container
# 1 Mise en place
Nous utilisons le Macbook comme le système hôte. Utilisons le VirtualBox et créer une machine virtuelle linux embarquée dans VirtualBox.
La machine virtuelle  Linux est Ubuntu 20.04.1 LTS.
Récupérer l'image du Linux sur Internet https://releases.ubuntu.com/20.04/ubuntu-20.04.1-live-server-amd64.iso.
Démarrer la machine virtuelle dans VirtualBox en utilisant l'image du Linux.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/creerhost.png "creer le VM host")
Et puis lancer la machine virtuelle.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/lancer_host.png "lancer host")

Enfin nous installons le lxd en utilisant la commande `sudo snap install lxd`

![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/installlxd.png "reussir installer lxd")
Nous avons réussir a finir les prérequis du TP.
# 2 Premiers pas avec LXD
## 2.1 LXD et nftables
Pour commencer, nous allons utiliser les iptables “à l’ancienne”.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/nftable.png "nftable")
## 2.2 Initialisation du service (daemon)
L’initialisation du service lxd se fait au moyen de la commande `sudo lxd init`
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/lxcinit.png "lxd init")
Il donc réussir à initialiser le service lxd avec des configurations nous avons choisis.
## 2.3 Lancement des instances
Pour lancer une instance de conteneur, nous utilisons la commande `lxc launch` avec la syntaxe `lxc template nom-conteneur`.
On d'abord lister toutes les templates en utilisant la commande `lxc image list`.

![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/imagelist.png "image list")

Et puis on peut lancer une instance de conteneur à pairtir l'image `ubuntu/18.04`.

![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/creercontainer.png "lancer conteneur")
## 2.4 État des instances
Pour savoir l'état d'un conteneur, on utilise la commande `lxc ls -c colonnes nom-conteneur` comme l'image ci-dessous.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/statusrun.png "etat conteneur")
## 2.5 Arrêt / redémarrage
On peut changer l'etat d'un conteneur en utilisant la commande `lxc start nom-conteneur` ou `lxc stop nom-conteneur` qui font démarrer ou arrêter un conteneur.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/stopcontainer.png "arreter conteneur")
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/startcontainer.png "demarrer conteneur")
## 2.6 Création de processus dans un container
La commande `lxc exec` permet d’exécuter une commande dans un container.
En utilisant la commande `lxc exec buster01 -- /bin/bash` on peut entrer dans la console du conteneur.

![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/bashcontainer.png "bash conteneur")
## 2.7 Exercices
### 2.7.1
On a déjà créer le conteneur buster01 à partir l'image Ubuntu/18.04.
### 2.7.2
- Les résultats des commandes `date` et `cat /proc/cpuinfo`:

 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/datecontainer.png "date conteneur")
On peur vois que la date est correct et les cpuinfos sont des informations des cpus hôst.

- Et les résultats des commandes `ls /dev` `free -h` et  `df -h`:

 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/devcontainer.png "dev conteneur")
On peut voir que la taille du mémoire est 1 GB et la taille du file-system est plus de 3 GB. Il peut-être pouvoir changer la taille du mémoire et la taille du file-system dans la configuration du lxc.

- Et les resultats des command `ip link ls`  `ip route ls`:

 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/ipcontainer.png "ip conteneur")

 Et en même temps on peut voir que la `ip route ls` du hôst (VM Ubuntu) a  `10.191.129.0/24 dev lxdbr01 proto kernel scope link src 10.191.129.1`. Donc la hôst VM est comme la gateway du conteneur.

- Enfin la command `systemctl status` s'affiche les services marchent dans le conteneur.

 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/systemctl.png "systemctl conteneur")
### 2.7.3
Entrer dans le conteneur et installer le ssh(ssh et sshd).

 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/installssh.png "install ssh")

 Revient au hôst (Ubuntu VM) et exécuter `ssh -l root 10.191.129.245` (ipv4 du conteneur).
 
 Il toujours demande la mot de passe et toujours `permission denied`.
#### Solution:
Entrer dans le conteneur et changer la configuration du sshd.
 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/viconfigurationsshd.png "vi configuration sshd")
 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/configurationsshd.png "configuration sshd")

 Changer la ligne à `PermitRootLogin yes`.
 Et puis relancer le sshd.
 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/restartsshd.png "relancer sshd")
 Réussir à se connecter le conteneur en ssh.
 ![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/connectssh.png "conncecter avec ssh")
### 2.7.4
- Créer le conteneur nginx01 en utilisant l'image ubuntu/18.04.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/creercontainernginx.png "lancer conteneur nginx")

- Installer ssh dans le conteneur et se connecter le conteneur en ssh.
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/connectsshnginx.png "conncecter avec ssh")

- Installer nginx dans le conteneur: `apt install nginx`

- Voir l'état du serveur nginx: `systemctl status nginx`
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/systemctlnginx.png "systemctl nginx")

- Tester le web serveur dans le hôst: `curl 10.191.129.98`
![](https://raw.githubusercontent.com/cedricxs/Virtualisation/main/images/curlnginx.png "curl nginx")


