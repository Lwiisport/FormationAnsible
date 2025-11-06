# Formation Ansible

## Un Labo pour Ansible

Capture 1 - Premier cluster de 4 machines virtuelles Alpine Linux

<img width="1920" height="1080" alt="001CaptureRocky" src="https://github.com/user-attachments/assets/b52c9e60-53bc-45c5-af09-e642d0a3f431" />

Capture 2 - Cluster de 4 machines virtuelles : Rocky Linux, Debian, OpenSUSE Leap et Ubuntu.

<img width="1920" height="1080" alt="002CaptureDebian" src="https://github.com/user-attachments/assets/4049b4b8-c973-4b88-9540-7bdb8b801d5c" />


## Installer Ansible

### Challenge n°1

1. ***Démarrez la VM `ubuntu` depuis le répertoire `atelier-01`***    
`$ vagrant up ubuntu`

2. ***Connectez-vous à cette VM***  
`$ vagrant ssh ubuntu`

3. ***Rafraîchissez les informatrions sur les paquets***  
`$ sudo apt update` et `$ sudo apt upgrade`.

4. ***Recherchez le paquet `ansible` avec les options qui vont bien***  
Je peux voir le paquet à installer avec la commande `$ sudo apt search ansible`.

5. ***Installer le paquet officiel fourni par la distribution***  
`$ sudo apt install -y ansible`

6. ***Vérifiez si l'installation s'est bien déroulé***  
`$ ansible --version` permet de voir que l'installation s'est bien déroulé.

7. ***Notez la version d'Ansible***  
`$ ansible --version` nous indique qu'il s'agit de la version `2.10.8` d'Ansible.

<img width="1920" height="1080" alt="008InstallAnsible" src="https://github.com/user-attachments/assets/81f65a1c-bbfc-4f6e-87f9-4b682020f2c9" />


9. ***Déconnectez-vous et supprimez la VM***  
`$ exit` pour sortir de la VM puis `$ vagrant destroy -f ubuntu` pour la supprimer.


### Challenge n°2

1. ***Répétez le challenge précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible***  
J'exécute exactement les mêmes commandes que pour le challenge 1, mais je termine l'installation avec la commande `$ sudo apt-add-repository ppa:ansible/ansible`

2. ***Notez la version fournie par ce dépôt tiers et comparez avec la version officielle du challenge précédent***  
Nous avions la version `2.10.8` pour le challenge précédent, la commande `$ ansible --version` nous indique cette fois-ci la version `2.17.14`. Nous avons une version plus récente sur ce deuxième challenge.


<img width="1920" height="1080" alt="009InstallAnsiblePPA" src="https://github.com/user-attachments/assets/8be84e2f-788a-4456-8a5c-c492a025e52e" />



### Challenge n°3

1. ***Lancez un VM Rocky linux et installer Ansible en utilisant PIP et Vitrualenv***  

- On créer et se connecte à la VM avec `$ vagrant up rocky` et `$ vagrant ssh rocky`

- On met à jour les informations des paquets avec `$ sudo dnf update -y`

- On install PIP et Virtualenv : `$ sudo dnf install python3-pip`

- On Initialise l'environnement VirtualEnv avec la commande `$ python3 -m venv ~/.venv/ansible`

- On lance VirtualEnv avec la commande `$ source ~/.venv/ansible/bin/activate`

- Maintenant que nous somme dans un environnement virtuel, nous mettons à jour PIP pour la première utilisation : `$ pip install --upgrade pip`

- Nous installons finalement Ansible : `$ pip install ansible`

- Nous pouvons voir que l'installation s'est bien déroulé avec la commande `$ ansible --version`. Sa version est `2.15.13`

<img width="1920" height="1080" alt="011AnsibleChallenge3" src="https://github.com/user-attachments/assets/0e3275d8-9d93-4160-886f-37f9162c33e3" />



## Authentification

1. ***Faites le nécessaire pour réussir un `ping` Ansible comme ceci***  

- Modification du document `/etc/hosts` :  
```
vagrant@control:~$ cat /etc/hosts
# /etc/hosts
127.0.0.1       localhost.localdomain   localhost
192.168.56.10   target01.sandbox.lan    target01
192.168.56.20   target02.sandbox.lan    target02
192.168.56.30   target03.sandbox.lan    target03
```

- Nous pouvons tester la connectivité de base avec nos VM :
```
vagrant@control:~$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
PING target01.sandbox.lan (192.168.56.10) 56(84) bytes of data.

--- target01.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.022/0.022/0.022/0.000 ms
PING target02.sandbox.lan (192.168.56.20) 56(84) bytes of data.

--- target02.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.151/1.151/1.151/0.000 ms
PING target03.sandbox.lan (192.168.56.30) 56(84) bytes of data.

--- target03.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.089/1.089/1.089/0.000 ms
```

- On collecte ensuite les clés SSH publiques des Target Hosts :
```
vagrant@control:~$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

- Génération d'une paire de clés SSH
```
vagrant@control:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:np/+i0VAfnKpVb8okYWh37oUIDru9AiEiBf3eOpaQjA vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|          ..o..  |
|         o..oo . |
|E . .  . o+o=   .|
|oo.o o. . oBo . .|
|o.o..oo S .+.o . |
| o. .o.. . .+    |
|  ..oo  o  o.    |
|   ++ o  ..+.    |
|  ...o . .=oo.   |
+----[SHA256]-----+
```

- Finalement, copie de la clé publique vers les Target Hosts
```
vagrant@control:~$ ssh-copy-id vagrant@target01
vagrant@control:~$ ssh-copy-id vagrant@target02
vagrant@control:~$ ssh-copy-id vagrant@target03
```

- On peut maintenant tester le bon fonctionnement de l'authentification sur les Target Hosts
```
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```


## Configuration de base

1. ***Editez `/etc/hosts` de manière à ce que les Target Hosts soient joignables par leut nom d'hôte simple.***  
On ajoute les adresses des Target Hosts dans le fichier `/etc/hosts`
```
# /etc/hosts
127.0.0.1       localhost.localdomain   localhost
192.168.56.10   control.sandbox.lan     control
192.168.56.20   target01.sandbox.lan    target01
192.168.56.30   target02.sandbox.lan    target02
192.168.56.40   target03.sandbox.lan    target03
```

Puis nous essayons de les pings simplement avec leur nom d'hôte simple.
```
vagrant@control:~$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.

--- target01.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.883/0.883/0.883/0.000 ms
PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.

--- target02.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.935/0.935/0.935/0.000 ms
PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.

--- target03.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.763/0.763/0.763/0.000 ms
```
Nous remarquons que les pings fonctionnent correctement.

3. ***Configurez l'authentification par clé SSH avec les trois Target Hosts.***  
On collecte tout d'abord les clés SSH publiques des Target Hosts
```
vagrant@control:~$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

On génère ensuite une paire de clés SSH sur le Control Host
```
vagrant@control:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:JQajEVnREWVgYaFGxptkhV1HdRoLIX2m/0DED1A1aWE vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|    o=B*XO=o**+E=|
|    .==*o. o..BB.|
|    .oooo .  =+o |
|     .o. o  . . .|
|        S    o   |
|              o  |
|               o |
|                .|
|                 |
+----[SHA256]-----+
```

Finalement on distribue la clé publique sur les Target Hosts 
```
vagrant@control:~$ ssh-copy-id vagrant@target01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.

vagrant@control:~$ ssh-copy-id vagrant@target02
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.

vagrant@control:~$ ssh-copy-id vagrant@target03
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added.

```

Je peux maintenant me connecter à chaque machine avec l'authentification par clés SSH.

<img width="1920" height="1080" alt="012SSHConfigBase" src="https://github.com/user-attachments/assets/2e32201c-aadf-40d7-ab7d-4ab1d2446683" />

5. ***Installez Ansible.***  
`sudo apt install -y ansible` permet d'installer Ansible sur le Control Host.

<img width="1920" height="1080" alt="013AnsibleConfigBase" src="https://github.com/user-attachments/assets/93500f4c-bb9e-4a93-a7fc-b7394175645e" />


6. ***Envoyer un premier `ping` Ansible sans configuration.***  
```
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

7. ***Créez un répertoire de projet `~/monprojet/`.***  
`$ mkdir ~/monprojet/`

8. ***Créez un fichier vide `ansible.cfg` dans ce répertoire.***  
`$ cd ~/monprojet/` puis `touch ansible.cfg`

10. ***Vérifiez si ce fichier est bien pris en compte par Ansible.***  
```
vagrant@control:~/monprojet$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```
La commande `ansible --version | head -n 2` nous montre bien que le fichier est bien pris en compte.

11. ***Spécifiez un inventaire nommé `hosts`.***  
12. ***Activer la journalisation dans `~/journal/ansible.log`.***  

<img width="1920" height="1080" alt="014AnsibleCFGConfigBase" src="https://github.com/user-attachments/assets/ab4aa3b9-0259-4d67-9862-937cf5e27c18" />


13. ***Testez la journalisation.***  
Afin de tester la journalisation, nous pouvons créer un dossier `$ mkdir -v ~/logs/` puis nous pouvons utiliser la commande `$ ansible all -i target01,targetà2,target03 -m ping`

```
vagrant@control:~/monprojet$ cat ~/logs/ansible.log 
2025-11-06 10:45:30,584 p=3361 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-11-06 10:45:30,603 p=3361 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-11-06 10:45:30,608 p=3361 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

14. ***Créez un groupe `[testlab]` avec vos trois Target Hosts.***  
15. ***Définissez explicitement l'utilisateur `vagrant` pour la connexion à vos cibles.***  
```
vagrant@control:~/monprojet$ cat hosts
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
```
Le groupe `[testlab]' est créé avec mes Target Hosts et on définit l'utilisateur `vagrant` pour la connexion.

16. ***Envoyez un `ping` Ansible vers le groupe de machines `[all]`.***  

<img width="1920" height="1080" alt="015PingConfigBase" src="https://github.com/user-attachments/assets/5c07fd08-7b9c-42e6-bf3a-ae842eb44999" />


17. ***Définissez l'élévation des droits pour l'utilisateur `vagrant` sur les Target Hosts.***  
```
vagrant@control:~/monprojet$ cat hosts 
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_become=yes
```
On ajoute la ligne `ansible_become=yes` pour l'élévation de droits pour l'utilisateur `vagrant` sur les Target Hosts.


18. ***Affichez la première ligne du fichier `/etc/shadow` sur tous les Target Hosts.***  

<img width="1920" height="1080" alt="016ShadowConfigBase" src="https://github.com/user-attachments/assets/e6205b57-d352-456d-af7d-4a49020991fb" />


19. ***Quittez le Control Host et supprimez touts les VM de l'atelier.***  

On utilise les commandes `exit` puis `vagrant destroy -f`.


## Idempotence

1. ***Installez successivement les paquets `tree`, `git` et `nmap` sur toutes les cibles.***  
J'exécute les commandes : `ansible all -m package -a "name=tree"`, `ansible all -m package -a "name=git"` et `ansible all -m package -a "name=nmap"`
Ceci permet d'installer les paquets `tree`, `git` et `nmap` sur toutes les cibles.

Finalement je m'assure que les paquets ce soit bien installer en testant les commandes depuis les Target Hosts.

<img width="1920" height="1080" alt="017PaquetIdemPotence" src="https://github.com/user-attachments/assets/23194e5c-1645-4720-b382-0c9a0a990800" />

Les paquets sont bien installé sur toutes les machines cibles.

2. ***Désinstallez successivement ces trois paquets en utilisant le pramamètre supplémentaire `state=absent`.***  
J'exécute maintenant les commandes : `ansible all -m package -a "name=tree state=absent"`, `ansible all -m package -a "name=git state=absent"` et `ansible all -m package -a "name=nmap state=absent"`
Ceci permet de désinstaller les paquets `tree`, `git` et `nmap` sur toutes les cibles.

Finalement je m'assur que les paquets soit bien désinstaller en testant les commandes depuis les Target Hosts.

<img width="1920" height="1080" alt="018UninstallidemPotence" src="https://github.com/user-attachments/assets/ee8e1872-8665-4f98-b24a-b92dc93d56ea" />

Les paquets sont bien désinstallé sur toutes les machines cibles

3. ***Copiez le fichier `/etc/fstab` du Control Host vers tous les Target Hosts sous forme d'un fichier `/tmp/test3.txt`.***  
J'utilise la commande `ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"`

<img width="1920" height="1080" alt="019CopieIdempotence" src="https://github.com/user-attachments/assets/7f724070-fbae-4d00-ba76-0d5c25266377" />

Le fichier est bien présent sur les Target Hosts.

4. ***Supprimez le fichier `/tmp/test3.txt` sur les Target Hosts en utilisant le module `file` avec le paramètre `state=absent`.***  
J'utilise la commande `ansible all -m file -a "dest=/tmp/test3.txt state=absent"`

<img width="1920" height="1080" alt="019CopieIdempotence" src="https://github.com/user-attachments/assets/08fb9990-db6d-41a9-b0c8-407d827d7e4f" />

Les fichiers `test3.txt` sont bien supprimer.

5. ***Enfin, affichez l'espace utilisé par la partition principale sur tous les Target Hosts. Que remarquez-vous ?***  
J'utilise la commande `ansible all -a "df -h /"`

<img width="1920" height="1080" alt="019CopieIdempotence" src="https://github.com/user-attachments/assets/19f405aa-8c49-4dc8-95eb-bf3be9bd7f5d" />

On remarque que la commande n'est pas idempotente. On voit l'annotation `CHANGED` à chaque fois que l'on exécute la commande.
