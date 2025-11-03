# Formation Ansible

## Un Labo pour Ansible

Capture 1 - Premier cluster de 4 machines virtuelles Alpine Linux

<img width="1920" height="1080" alt="001CaptureRocky" src="https://github.com/user-attachments/assets/b52c9e60-53bc-45c5-af09-e642d0a3f431" />

Capture 2 - Cluster de 4 machines virtuelles : Rocky Linux, Debian, OpenSUSE Leap et Ubuntu.

<img width="1920" height="1080" alt="002CaptureDebian" src="https://github.com/user-attachments/assets/4049b4b8-c973-4b88-9540-7bdb8b801d5c" />


## Installer Ansible

### Challenge n°1

1. ***Démarrez la VM `ubuntu` depuis le répertoire `atelier-01`***    
J'utilise la commande `$ vagrant up ubuntu`.

2. ***Connectez-vous à cette VM***  
J'utilise la commande `$ vagrant ssh ubuntu`.

3. ***Rafraîchissez les informatrions sur les paquets***  
J'utilise les commande `$ sudo apt update` et `$ sudo apt upgrade`.

4. ***Recherchez le paquet `ansible` avec les options qui vont bien***  
Je peux voir le paquet à installer avec la commande `$ sudo apt search ansible`.

5. ***Installer le paquet officiel fourni par la distribution***  
J'utilise la commande `$ sudo apt install -y ansible`.

6. ***Vérifiez si l'installation s'est bien déroulé***  
La commande `$ ansible --version` permet de voir que l'installation s'est bien déroulé.

7. ***Notez la version d'Ansible***  
La commande `$ ansible --version` nous indique qu'il s'agit de la version 2.10.8 d'Ansible.

<img width="1920" height="1080" alt="008InstallAnsible" src="https://github.com/user-attachments/assets/81f65a1c-bbfc-4f6e-87f9-4b682020f2c9" />


9. ***Déconnectez-vous et supprimez la VM***  
J'utilise les commandes `$ exit` pour sortir de la VM pui `$ vagrant destroy -f ubuntu` pour la supprimer.


### Challenge n°2

1. ***Répétez le challenge précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible***  
J'exécute exactement les mêmes commandes que pour le challenge 1, mais je termine l'installation avec la commande `$ sudo apt-add-repository ppa:ansible/ansible`

2. ***Notez la version fournie par ce dépôt tiers et comparez avec la version officielle du challenge précédent***  
Nous avions la version `2.10.8` pour le challenge précédent, la commande `ansible --version` nous indique cette fois-ci la version `2.17.14`. Nous avons une version plus récente sur ce deuxième challenge.


<img width="1920" height="1080" alt="009InstallAnsiblePPA" src="https://github.com/user-attachments/assets/8be84e2f-788a-4456-8a5c-c492a025e52e" />



### Challenge n°3

1. ***Lancez un VM Rocky linux et installer Ansible en utilisant PIP et Vitrualenv***

- On créer et se connecte à la VM avec les commandes `$ vagrant up rocky` et `$ vagrant ssh rocky`

- On met à jour les informations des paquets avec `$ sudo dnf update -y`

- On install PIP et Virtualenv : `$ sudo dnf install python3-pip'

- On Initialise l'environnement VirtualEnv avec la commande `$ python3 -m venv ~/.venv/ansible`

- On lance VirtualEnv avec la commande `$ source ~/.venv/ansible/bin/activate`

- Maintenant que nous somme dans un environnement virtuel, nous mettons à jour PIP pour la première utilisation : `$ pip install --upgrade pip`

- Nous installons finalement Ansible : `$ pip install ansible`

- Nous pouvons voir que l'installation s'est bien déroulé avec la commande `$ ansible --version`. Sa version est `2.15.13`

<img width="1920" height="1080" alt="011AnsibleChallenge3" src="https://github.com/user-attachments/assets/0e3275d8-9d93-4160-886f-37f9162c33e3" />



## Authentification
