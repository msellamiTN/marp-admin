Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Atelier 1 : Atelier d'Installation et
d'Utilisation de Vagrant
Objectif de l'atelier : Apprendre à installer Vagrant et à créer une machine virtuelle avec Vagrant
pour un environnement de développement.

Prérequis :

Un ordinateur fonctionnant sous Windows, macOS ou Linux.
Oracle VirtualBox (ou un autre hyperviseur) déjà installé sur votre système.
Étape 1 : Installation de Vagrant

Rendez-vous sur le site officiel de Vagrant à l'adresse https://www.vagrantup.com/.
Téléchargez la dernière version de Vagrant compatible avec votre système d'exploitation.
Installez Vagrant en suivant les instructions fournies pour votre système d'exploitation.
Étape 2 : Configuration de l'environnement de travail

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Ouvrez un terminal (invite de commande) sur votre ordinateur.
Étape 3 : Création d'un dossier de projet

Créez un nouveau dossier pour votre projet Vagrant. Par exemple : mkdir
mon_projet_vagrant.
Déplacez-vous dans le dossier de projet avec la commande cd mon_projet_vagrant.
Étape 4 : Initialisation de la configuration Vagrant

Exécutez la commande suivante pour initialiser une configuration Vagrant dans le dossier
de projet :
vagrant init

Cette commande crée un fichier Vagrantfile dans le dossier de projet, qui contient la
configuration de la machine virtuelle que vous allez créer.
Étape 5 : Choix de la box Vagrant

Choisissez une "box" Vagrant à utiliser. Une box est une image de base de machine virtuelle.
Vous pouvez parcourir les box disponibles sur le site Vagrant Cloud à l'adresse
https://app.vagrantup.com/boxes/search.
Pour ajouter une box à votre configuration, exécutez la commande suivante en remplaçant
nom_de_la_box par le nom de la box que vous avez choisie :
vagrant box add nom_de_la_box

Étape 6 : Configuration de la machine virtuelle

Ouvrez le fichier Vagrantfile dans un éditeur de texte.
Modifiez les paramètres de configuration selon vos besoins, tels que la quantité de
mémoire, le nombre de CPU, etc. Par exemple :
Vagrant.configure("2") do |config|
config.vm.box = "nom_de_la_box"
config.vm.network "private_network", type: "dhcp"
config.vm.provider "virtualbox" do |vb|
vb.memory = "1024"
vb.cpus = 2
end
end
Étape 7 : Démarrage de la machine virtuelle

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Pour démarrer la machine virtuelle, exécutez la commande :
vagrant up

Vagrant téléchargera la box si elle n'a pas encore été téléchargée et créera la machine
virtuelle en fonction de la configuration spécifiée.
Étape 8 : Connexion à la machine virtuelle

Pour vous connecter à la machine virtuelle, exécutez la commande :
vagrant ssh

Vous serez connecté à la machine virtuelle en tant qu'utilisateur par défaut.
Étape 9 : Arrêt et destruction de la machine virtuelle

Pour arrêter la machine virtuelle, exécutez la commande :
vagrant halt

Pour détruire complètement la machine virtuelle, exécutez la commande :
vagrant destroy

C'est tout pour cet atelier d'installation et d'utilisation de Vagrant! Vous avez maintenant appris à
configurer et à gérer des environnements de développement avec Vagrant. N'hésitez pas à explorer
davantage la documentation de Vagrant pour en savoir plus sur ses fonctionnalités avancées.

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Atelier 2 : Configuration avancée des
VMs avec Vagrant
Objectif : Apprendre à créer et à configurer une machine virtuelle avec Vagrant en utilisant une
configuration personnalisée.

Prérequis :

Un ordinateur fonctionnant sous Windows, macOS ou Linux.
VirtualBox déjà installé sur votre système.
Vagrant installé sur votre système.
Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Étape 1 : installation des plugins et Désactivation de la mise à jour automatique des
VirtualBox Guest Additions

Dans cette étape, nous allons installer le plugin VAGRANT_EXPERIMENTAL, qui est
nécessaire pour activer la fonctionnalité de disques expérimentale dans Vagrant.

installation des plugins :

Ouvrez un terminal sur votre ordinateur et créer un dossier mapr-cluster
mkdir mapr-cluster

Utilisez la commande suivante pour installer le plugin VAGRANT_EXPERIMENTAL :
vagrant plugin install vagrant-experimental

Assurez-vous d'avoir une version de Vagrant égale ou supérieure à 2.2.8, car cette
fonctionnalité nécessite une version compatible.

Une fois l'installation terminée, vous pouvez activer la fonctionnalité de disques
expérimentale en définissant la variable d'environnement
VAGRANT_EXPERIMENTAL dans votre Vagrantfile comme suit :
Activer la fonctionnalité de disques expérimentale
ENV["VAGRANT_EXPERIMENTAL"] = "disks"

Assurez-vous d'ajouter cette ligne au début de votre Vagrantfile.

Maintenant, vous avez installé avec succès le plugin VAGRANT_EXPERIMENTAL et activé la
fonctionnalité de disques expérimentale dans votre environnement Vagrant.

Cette étape est nécessaire pour utiliser la fonctionnalité de disques dans votre atelier

Désactivation de la mise à jour automatique :

Cette étape désactive la mise à jour automatique des VirtualBox Guest Additions, ce qui
peut être nécessaire pour éviter les problèmes de compatibilité potentiels.

Vagrant.configure("2") do |config|

Disable the automatic update of VirtualBox Guest Additions
if Vagrant.has_plugin?("vagrant-vbguest")
config.vbguest.auto_update = false
end

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Le code utilise config.vbguest.auto_update = false pour désactiver la mise à jour
automatique des Guest Additions si le plugin "vagrant-vbguest" est installé.
Étape 2 : Configuration de la machine virtuelle

Objectif : Ici, nous configurons les paramètres de base de la machine virtuelle, tels que
l'image de base et le nom d'hôte.

Script :

Edge Node
config.vm.define "mapr-edge" do |edge|
edge.vm.box = "ubuntu/focal64"
edge.vm.hostname = "mapr-edge-data2-ai.com"

config.vm.define crée une machine virtuelle nommée "mapr-edge".
edge.vm.box spécifie l'image de base de la machine virtuelle (Ubuntu 20.04).
edge.vm.hostname définit le nom d'hôte de la machine virtuelle.
Étape 3 : Configuration des ressources de la machine virtuelle

Objectif : Dans cette étape, nous définissons les ressources allouées à la machine
virtuelle, y compris la mémoire, le nombre de CPU et la taille du disque.

Script :

Set memory, CPU, and disk resources
edge.vm.disk :disk, size: "30GB", primary: true
edge.vm.provider "virtualbox" do |vb|
vb.memory = 8000 # 24 GB RAM
vb.cpus = 8 # 8 CPUs

edge.vm.disk ajoute un disque virtuel de 30 Go à la machine virtuelle.
edge.vm.provider "virtualbox" configure la mémoire (8 Go) et le nombre de CPU (8)
pour la machine virtuelle.
Étape 4 : Configuration des ports redirigés

Objectif : Nous configurons les ports redirigés pour permettre la communication entre la
machine virtuelle et l'hôte.

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Script :

edge.vm.network "forwarded_port", guest: 22 , host: 2220 , id: "ssh",
auto_correct: true

La ligne edge.vm.network "forwarded_port", guest: 22, host: 2220 , id: "ssh",
auto_correct: true redirige le port SSH de la machine virtuelle (22) vers le port 2220
de l'hôte.
Étape 5 : Configuration de l'adresse IP

Objectif : Nous configurons les adresses IP de la machine virtuelle.

Script :

NAT adapter for internet access
edge.vm.network "public_network", type: "static", ip:
"192.168.56.10", bridge: "ens160"

Internal/private network for communication between VMs
edge.vm.network "private_network", type: "static", ip:
"192.168.56.10", virtualbox__intnet: true , name: "mapr-cluster-net"

edge.vm.network "public_network" configure une interface réseau avec une
adresse IP statique (192.168.56.10) et un pont vers l'interface réseau de l'hôte
(ens160).
edge.vm.network "private_network" configure une interface réseau privée avec
une adresse IP statique (192.168.56.10) pour la communication entre les machines
virtuelles.
Vous pouvez exécuter la machine virtuelle avec vagrant up depuis le terminal. Cette étape
démarre la machine virtuelle et la provisionne en fonction de la configuration spécifiée.

Étape 6 : Gestion de la machine virtuelle

Objectif :

Pour accéder à la machine avec ssh
ssh vagrant@localhost -p 222 2
Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Pour arrêter la machine virtuelle, utilisez vagrant halt. Cela mettra la machine
virtuelle hors tension.
Pour détruire complètement la machine virtuelle, utilisez vagrant destroy. Cela
supprimera la machine virtuelle et tous ses fichiers associés.
Étape 7 : Installations des packages sous les VMs avec SSH et Vagrant
Objectif : Dans cette etape, nous allons installer les packages requis individuellement sur
la machine virtuelle en utilisant SSH et la commande apt-get.

Instructions :

Assurez-vous que votre machine virtuelle est en cours d'exécution à l'aide de la
commande vagrant up.
Utilisez SSH pour vous connecter à votre machine virtuelle en exécutant la
commande suivante :
vagrant ssh

Installation d'OpenJDK 11

Pour installer OpenJDK 11, utilisez la commande suivante :
sudo apt-get update

sudo apt-get install -y openjdk- 11 - jdk

Cette commande installe OpenJDK 11.
Installation de wget

Pour installer l'outil de téléchargement wget, utilisez la commande suivante :
sudo apt-get install -y wget

Cette commande installe wget.
Installation de tar

Pour installer l'utilitaire de manipulation d'archives tar, utilisez la commande
suivante :
sudo apt-get install -y tar

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Cette commande installe tar.
Installation de Python 3

Pour installer Python 3, utilisez la commande suivante :
sudo apt-get install -y python

Cette commande installe Python 3.
Installation de python3-selinux

Pour installer le package python3-selinux, utilisez la commande suivante :
sudo apt-get install -y python3-selinux

Cette commande installe python3-selinux.
Vous avez maintenant installé chaque package requis individuellement sur la
machine virtuelle.
Pour vous déconnecter de la machine virtuelle, utilisez la commande exit.
Ces étapes vous permettront d'installer chaque package requis individuellement sur la
machine virtuelle en utilisant SSH et la commande apt-get. Vous pouvez personnaliser ces
étapes en fonction de vos besoins spécifiques pour la configuration de votre machine
virtuelle.

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Atelier 3 : Approvisionnement avec un script
shell avec Vagrant
Objectif : Dans cette étape, nous allons approvisionner la machine virtuelle en exécutant
un script shell qui installe les packages requis, notamment OpenJDK 11, wget, tar, Python 3
et python3-selinux.

Etape 1 : Installation des packages avec un shell
Assurez-vous que votre machine virtuelle est en cours d'exécution à l'aide de la
commande vagrant up.
Créer un fichier Shell contenant les étapes d’installations ‘packages
install_packages.sh
#!/bin/bash

Update the package manager
sudo apt-get update

Install OpenJDK 11
sudo apt-get install - y openjdk- 11 - jdk

Install wget
sudo apt-get install - y wget

Install tar
sudo apt-get install - y tar

Install Python 3
sudo apt-get install - y python

Install python3-selinux
sudo apt-get install - y python3-selinux

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Dans votre Vagrantfile , ajoutez une section de provisionnement pour spécifier le
script shell à exécuter.
Provisioning with a shell script
edge.vm.provision "shell", path: "install_packages.sh"

Ce code utilise config.vm.provision pour spécifier un script shell en ligne
qui installe les packages requis un par un en utilisant apt-get.
Après avoir ajouté cette section de provisionnement, enregistrez votre Vagrantfile.
Pour appliquer le provisionnement à votre machine virtuelle, utilisez la commande
suivante :
vagrant provision

Cette commande exécutera le script shell sur la machine virtuelle pour installer les
packages requis.
Une fois le provisionnement terminé, vous avez maintenant installé les packages
requis sur la machine virtuelle.
Cela vous permettra d'approvisionner la machine virtuelle en utilisant un script shell
pour l'installation des packages requis, ce qui peut être pratique pour automatiser le
processus d'installation.
Étape 2 : Génération des clés SSH et du fichier hosts
Objectif : Dans cette étape, nous allons générer des clés SSH et un fichier hosts pour
faciliter la communication sécurisée entre les machines virtuelles.

Instructions :

Ouvrez un terminal sur votre ordinateur.
Génération des clés SSH

Pour générer une paire de clés SSH (une clé privée et une clé publique), utilisez la
commande suivante :
ssh-keygen -t rsa -b 4096 - f ~/.ssh/id_rsa

t rsa : Spécifie le type de clé RSA.
b 4096 : Spécifie la taille de la clé (4096 bits est recommandé pour la
sécurité).
Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
f ~/.ssh/id_rsa : Spécifie le nom du fichier pour la clé privée (id_rsa) et la clé
publique (id_rsa.pub).
Suivez les invites pour configurer une phrase secrète pour votre clé SSH. Cette
phrase secrète est un mot de passe supplémentaire pour protéger votre clé privée.
Une fois la génération terminée, vos clés SSH seront stockées dans le répertoire
~/.ssh/. Vous pouvez maintenant les utiliser pour vous connecter en toute sécurité à
vos machines virtuelles.
Création du fichier hosts

Créez un fichier external_hosts (ou utilisez un nom de fichier de votre choix) pour
spécifier les adresses IP et les noms d'hôte de vos machines virtuelles. Voici un
exemple de contenu :
192.168.56.10 mapr-edge-data2-ai.com

Assurez-vous de remplacer l'adresse IP et le nom d'hôte par ceux de votre configuration.

6. Enregistrez ce fichier dans le répertoire de votre projet.

Étape 3 : Copie de fichiers vers la machine virtuelle

Objectif : Nous copions des fichiers depuis l'hôte vers la machine virtuelle.

Script :

Copy hosts file
edge.vm.provision "file", source: "external_hosts", destination:
"/tmp/hosts"
edge.vm.provision "shell", inline: "sudo mv /tmp/hosts
/etc/hosts"
edge.vm.provision "file", source: "/home/user2/mapr-
cluster/.ssh/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
edge.vm.provision "file", source: "/home/user2/mapr-
cluster/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
edge.vm.provision "file", source: "/home/user2/mapr-
cluster/.ssh/id_rsa.pub", destination:
"/home/vagrant/.ssh/authorized_keys"

Les commandes edge.vm.provision "file" copient des fichiers de l'hôte vers des
emplacements spécifiques de la machine virtuelle, notamment le fichier
"/tmp/hosts" et les clés SSH.
Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Après avoir ajouté cette section de provisionnement, enregistrez votre Vagrantfile.
Pour appliquer le provisionnement à votre machine virtuelle, utilisez la commande
suivante :
vagrant provision

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Atelier 4 : Préparation des VMs pour un
cluster MapR avec Vagrant
Objectif : L'objectif de cet atelier est de créer et de configurer des VMs pour un cluster MapR en
utilisant Vagrant.

Prérequis :

Un ordinateur fonctionnant sous Windows, macOS ou Linux.
VirtualBox déjà installé sur votre système.
Vagrant installé sur votre système.
Introduction : Dans cet atelier, vous apprendrez à créer et à configurer des machines virtuelles
(VMs) pour simuler un environnement de cluster MapR à l'aide de Vagrant. Chaque VM
représentera un nœud spécifique dans le cluster MapR.

Configuration des VMs : Voici la configuration de base de chaque VM que vous allez créer :

Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
Nœud Hostname RAM CPUs
Disque
Principal
Disque
Secondaire Ports Forwarded IP Publique IP Privée
Edge

mapr-edge-data2-
ai.com 8GB 8 30GB 120GB SSH: 2220 192.168.56.10 192.168.56.
Master

mapr-master-
data2-ai.com 24GB 8 30GB 120GB
CLDB: 7222, ZooKeeper: 5181,
etc. 192.168.56.11 192.168.56.
Worker

mapr-worker1-
data2-ai.com 8GB 4 20GB 150GB
NodeManager: 8042,
DataNode: 50010, etc. 192.168.56.12 192.168.56.
Worker

mapr-worker2-
data2-ai.com 8GB 4 20GB 150GB
NodeManager: 8043,
DataNode: 50011, etc. 192.168.56.13 192.168.56.
Worker

mapr-worker3-
data2-ai.com 8GB 4 20GB 150GB
NodeManager: 8044,
DataNode: 50012, etc. 192.168.56.14 192.168.56.
Chaque VM est configurée avec des ressources spécifiques, des ports forwardés pour la
communication, et des adresses IP dédiées pour simuler un environnement de
Étapes de l'atelier :
Étape 1 : Création du répertoire de travail
Ouvrez un terminal ou une invite de commande.
Créez un répertoire de travail pour cet atelier et accédez-y :
mkdir cluster-mapr cd cluster-mapr
Étape 2 : Configuration du Vagrantfile
Créez un fichier Vagrantfile dans le répertoire de travail en utilisant un éditeur de
texte.
Ajoutez la configuration de base pour chaque nœud du cluster en utilisant la
structure fournie dans le tableau. Personnalisez les valeurs en fonction des
spécifications données.
Vagrant.configure("2") do |config|
config.vm.define "node-name" do |node|
node.vm.box = "ubuntu/focal64"
node.vm.hostname = "hostname"
node.vm.memory = RAM
node.vm.cpus = CPUs
node.vm.disk :disk, size: "30GB", primary: true # Disque Principal
node.vm.disk :disk, size: "120GB" # Disque Secondaire
Formateur : Sellami Mokhtar
mokhtar.sellami@gmail.com
node.vm.network "forwarded_port", guest: guest-port, host: host-port,
auto_correct: true # Ports Forwarded
node.vm.network "public_network", type: "static", ip: "public-IP" # IP
Publique
node.vm.network "private_network", type: "static", ip: "private-IP" # IP
Privée
end
end

Assurez-vous de personnaliser chaque nœud (edge, master, workers) avec les
spécifications données.