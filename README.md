# Formation Ansible

## Un Labo pour Ansible

Capture 1 - Premier cluster de 4 machines virtuelles Alpine Linux

Capture 2 - Cluster de 4 machines virtuelles : Rocky Linux, Debian, OpenSUSE Leap et Ubuntu.


## Installer Ansible

### Challenge n°1

1. ***Démarrez la VM `ubuntu` depuis le répertoire `atelier-01`***    
J'utilise la commande `vagrant up ubuntu`.

2. ***Connectez-vous à cette VM***  
J'utilise la commande `vagrant ssh ubuntu`.

3. ***Rafraîchissez les informatrions sur les paquets***  
J'utilise les commande `sudo apt update' et 'sudo apt upgrade`.

4. ***Recherchez le paquet `ansible` avec les options qui vont bien***  
Je peux voir le paquet à installer avec la commande `sudo apt search ansible`.

5. ***Installer le paquet officiel fourni par la distribution***  
J'utilise la commande `sudo apt install -y ansible`.

6. ***Vérifiez si l'installation s'est bien déroulé***  
La commande `ansible --version` permet de voir que l'installation s'est bien déroulé.

7. ***Notez la version d'Ansible***  
La commande `ansible --version` nous indique qu'il s'agit de la version 2.10.8 d'Ansible.

8. ***Déconnectez-vous et supprimez la VM***  
J'utilise les commandes `exit` pour sortir de la VM pui `vagrant destroy -f ubuntu` pour la supprimer.


### Challenge n°2

1. ***Répétez le challenge précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible***  
J'exécute exactement les mêmes commandes que pour le challenge 1, mais je termine l'installation avec la commande `sudo apt-add-repository ppa:ansible/ansible`

2. ***Notez la version fournie par ce dépôt tiers et comparez avec la version officielle du challenge précédent***  
Nous avions la version `2.10.8` pour le challenge précédent, la commande `ansible --version` nous indique cette fois-ci la version `2.17.14`. Nous avons une version plus récente sur ce deuxième challenge.


### Challenge n°3

