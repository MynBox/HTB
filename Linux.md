`apropos` 

`Kulina@htb[/htb]$ apropos <keyword>`

```
Kulina@htb[/htb]$ apropos sudo
sudo (8)             - execute a command as another user
sudo.conf (5)        - configuration for sudo front end
sudo_plugin (8)      - Sudo Plugin API
sudo_root (8)        - How to run administrative commands
sudoedit (8)         - execute a command as another user
sudoers (5)          - default sudo security policy plugin
sudoreplay (8)       - replay sudo session logs
visudo (8)           - edit the sudoers file
```

### Information système

***uname***

La commande `uname` permet d’obtenir des infos sur le système. Quelques options basiques :

- **`uname`** : Affiche juste le nom du noyau (par exemple "Linux").
- **`uname -a`** : Affiche toutes les infos disponibles : nom du noyau, nom de l'ordinateur, version du noyau, date de compilation, architecture matérielle, etc.
- **`uname -s`** : Affiche le nom du noyau.
- **`uname -n`** : Affiche le nom de l'hôte (nom de l’ordinateur sur le réseau).
- **`uname -r`** : Affiche la version du noyau.
- **`uname -v`** : Affiche la version du noyau avec la date de compilation.
- **`uname -m`** : Affiche l'architecture matérielle (par exemple x86_64).
- **`uname -p`** : Affiche le type de processeur.
- **`uname -i`** : Affiche la plateforme matérielle.
- **`uname -o`** : Affiche le système d'exploitation.

Par exemple, en tapant `uname -a`, on peut obtenir une sortie comme celle-ci :

```
Linux Dell 5.8.0-53-generic #60~20.04.1-Ubuntu SMP Wed May 12 20:40:08 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

```

- **Linux** : Le noyau.
- **monordi** : Le nom de ta machine.
- **5.8.0-53-generic** : La version du noyau.
- **#60~20.04.1-Ubuntu SMP Wed May 12 20:40:08 UTC 2021** : La version du noyau avec la date de compilation.
- **x86_64** (trois fois) : L'architecture matérielle (64 bits ici).
- **GNU/Linux** : Le système d'exploitation.

***env***

Une autre commande pour obtenir des informations sur son système est la commande `env` qui permet d'afficher les variables d'environnement actuelles.

- **`env`** : Affiche toutes les variables d'environnement et leurs valeurs actuelles.

En tapant  `env`, on peut obtenir une sortie comme celle-ci :

```
SHELL=/bin/bash
USER=W@ffl3
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/home/W@ffl3
LANG=en_US.UTF-8
HOME=/home/W@ffl3

```

Cela donne des infos sur l'environnement :

- **SHELL** : Le shell utilisé, ici `/bin/bash`.
- **USER** : Le nom de l'utilisateur, ici `W@ffl3`.
- **PATH** : Les chemins où le système cherche les exécutables.
- **PWD** : Le répertoire de travail actuel.
- **LANG** : La langue et la codification utilisée, ici `en_US.UTF-8`.
- **HOME** : Le répertoire personnel de l'utilisateur.

### Travail sur fichiers et répertoires

***touch***

La commande `touch` sur Linux est utilisée principalement pour créer des fichiers vides ou pour mettre à jour la date et l’heure des fichiers existants.

**Créer un fichier vide** :

- `touch nomdufichier` : Si `nomdufichier` n'existe pas, cette commande crée un fichier vide avec ce nom.
- Exemple : `touch fichier.txt` crée un fichier vide nommé `fichier.txt`.

**Mettre à jour les horodatages d'un fichier existant** :

- `touch nomdufichier` : Si le fichier existe déjà, cette commande met à jour la date et l'heure d'accès et de modification à l'heure actuelle.
- Exemple : `touch fichier.txt` met à jour les horodatages de `fichier.txt` à l'heure actuelle.

**Définir une date et une heure spécifiques** :

- `touch -t [[CC]YY]MMDDhhmm[.ss] nomdufichier` : Met à jour les horodatages à une date et heure spécifiques.
- Exemple : `touch -t 202406211200.00 fichier.txt` définit l'horodatage de `fichier.txt` au 21 juin 2024 à 12:00:00.

***ls***

Je connaissais déjà la commande `ls` pour afficher le contenu d’un répertoire mais je ne connaissais pas l’option `-i`  qui nous permet de d’indexer le contenu.

ls -i for index 

### Trouver des fichiers et des répertoires

La commande `find` nous permet de rechercher des fichiers ou répertoires sur notre système. C’est les différentes options que j’ai apprises lors de cette section qui m’ont marqués.

```bash

find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null
```

### Décomposition de la commande

- **`find /`** : Commence la recherche à partir de la racine du système de fichiers.
- **`type f`** : Limite la recherche aux fichiers (et non aux répertoires, liens symboliques, etc.).
    - type f ⇒ files et type d ⇒ directories
- **`name *.conf`** : Rechercher des fichiers dont le nom correspond au motif `.conf`
- **`user root`** : Limite la recherche aux fichiers appartenant à l'utilisateur `root`.
- **`size +20k`** : Limite la recherche aux fichiers dont la taille est supérieure à 20 kilooctets.
- **`newermt 2020-03-03`** : Limite la recherche aux fichiers modifiés après le 3 mars 2020.
- **`exec ls -al {} \;`** : Pour chaque fichier trouvé, exécute la commande `ls -al` pour afficher les détails du fichier (permissions, nombre de liens, propriétaire, groupe, taille, date de modification, nom). Les accolades `{}` sont remplacées par le chemin du fichier trouvé. Le point-virgule `\;` termine la commande `exec`.
- **`2>/dev/null`** : Redirige les messages d'erreur vers `/dev/null` pour ne pas afficher les erreurs (comme les permissions refusées) dans la sortie standard.

### **File Descriptors et Redirections**

Un descripteur de fichier est un identifiant non négatif (généralement un petit entier) utilisé par le système d'exploitation pour faire référence à un fichier ou à une autre ressource d'entrée/sortie.

Les descripteurs de fichier sont utilisés par les processus pour accéder à ces ressources de manière efficace et uniforme.

***Les différents types***

- **FD 0 : Standard Input (stdin)** :
    - Utilisé pour lire les données d'entrée.
    - Par défaut, il est associé au terminal (clavier).
    - Exemple : lire des données tapées par l'utilisateur.
- **FD 1 : Standard Output (stdout)** :
    - Utilisé pour écrire les données de sortie.
    - Par défaut, il est associé au terminal (écran).
    - Exemple : afficher des résultats ou des messages à l'utilisateur.
- **FD 2 : Standard Error (stderr)** :
    - Utilisé pour écrire les messages d'erreur.
    - Par défaut, il est associé au terminal (écran).
    - Exemple : afficher des messages d'erreur ou de diagnostic.

- **`2>/dev/null` : On avait utilisé cette option dans le `find` dans la section précédente. Donc enfaite le 2 fait référence au stderr.**

**exemple d’utilisation :**

```bash

find /etc/ -name shadow 2> stderr.txt 1> stdout.txt
```

- En faisant cat stderr.txt, on verra que le fichier contient toute les erreurs de sorties comme les Permision denied.
- En faisant cat stdout.txt, on verra que le fichier contient les ressources trouvées nommées “shadow” dans le répertoire /etc/. Dans notre cas ce sera /etc/shadow

```bash

cat < stdout.txt
```

- Avec < on utilise le fichier stdout.txt (Le fichier on a stocké les sorties/output) comme entrée/input.

***EOF*** 

Ce délimiteur (End Of File) permet de spécifier la fin d’une entrée.

```bash

cat << EOF > stream.txt
```

Dans l’invite de commande je vais rentrer :

Hack

The

Box

EOF

```bash
cat stream.txt
```

Le fichier contient :

Hack

The

Box

Je pourrais très bien remplacer le EOF par FIN.

`dpkg -l | grep -c '^ii'`

La commande `dpkg -l | grep -c '^ii'` est utilisée pour compter le nombre de paquets installés sur un système Debian ou basé sur Debian (comme Ubuntu).

### Explication de chaque composant de la commande :

1. **`dpkg -l`** :
    - Cette commande liste tous les paquets installés ou dont l'installation a été tentée. La sortie contient plusieurs colonnes, dont le statut du paquet.
2. **`|` (pipe)** :
    - Ce symbole est utilisé pour prendre la sortie de la commande `dpkg -l` et la passer comme entrée à la commande suivante (`grep`).
3. **`grep -c '^ii'`** :
    - `grep` est utilisé pour filtrer les lignes qui correspondent à un motif spécifique.
    - `c` option de `grep` compte le nombre de lignes qui correspondent au motif.
    - `'^ii'` est un motif qui signifie "ligne commençant par 'ii'". Dans la sortie de `dpkg -l`, une ligne commençant par `ii` indique que le paquet est correctement installé.

### Que fait cette commande dans son ensemble ?

La commande entière :

```bash
dpkg -l | grep -c '^ii'

```

Liste tous les paquets sur le système, filtre ceux qui sont installés correctement (`^ii`), et compte le nombre de ces paquets.

### Exemple de sortie

Supposons que vous ayez 1234 paquets installés correctement. La commande retournerait simplement :

```
1234

```

Cette commande est très utile pour obtenir rapidement le nombre total de paquets installés sur votre système.

### Commandes liées

Voici quelques autres commandes utiles pour la gestion des paquets sur un système Debian ou basé sur Debian :

- **Lister tous les paquets installés** :
    
    ```bash
    dpkg -l
    
    ```
    
- **Rechercher un paquet spécifique** :
    
    ```bash
    dpkg -l | grep <nom_du_paquet>
    
    ```
    
- **Afficher des informations sur un paquet spécifique** :
    
    ```bash
    dpkg -s <nom_du_paquet>
    
    ```
    
- **Lister les fichiers installés par un paquet** :
    
    ```bash
    dpkg -L <nom_du_paquet>
    
    ```
    

Ces commandes peuvent vous aider à mieux gérer et comprendre l'état des paquets sur votre système.

### Filtrer le contenu

Différence entre `more` et `less`:

Lorsque je lis le contenu d’un fichier avec more. Le contenu reste dans le terminal lorsque je quitte tandis qu’avec less le contenu ne reste pas sur le teminale.

```bash
netstat -ln4 | grep LISTEN | grep -v 127 | wc -l
```

- netstat -ln4 - services that are listening, with numeric addresses, and using the ipv4 protocol as opposed to ipv6 or unspecified
- grep LISTEN - find results containing the word “LISTEN”
- grep -v 127 - exclude any results that contain the number “127”
- wc -l - count the number of lines

```bash

grep -E "(my.*false)" /etc/passwd
grep -E "my" /etc/passwd | grep -E "false"
```

Ces deux commandes sont équivalentes.

### **Package Management**

Un paquet est un composant fondamental de la gestion des logiciels dans les systèmes Linux. Il simplifie l'installation, la mise à jour et la gestion des applications, tout en assurant que toutes les dépendances et configurations nécessaires sont en place pour que le logiciel fonctionne correctement.

```bash
apt-cache search \paquet recherché\
```

Cette commande permet d’obtenir des informations sur les paquets sur notre machine.
Quelle paquet est sur notre machine et une description brève de celle-ci.

```bash
apt-cache show impacket-scripts
```

Cette commande nous  permet d’avoir plus d’informations sur un paquet précis.

```bash
apt list --installed
```

Pour voir tous les paquets installés

**Les différentes façons d’installer des paquets.**

**Méthode avec `git`**

```bash
mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang
```

- Aller sur la page GitHub du package voulu.
- Prendre l’URL pour copier.
- (Créer le répertoire où on va stocker le dépôt git. Qui contiendra des scripts, modules, documentations, etc.) Optionnel
- Utilisation de la commande git clone avec URL
- Emplacement où on va stocker le dépôt

**Méthode avec `apt-get` install**

```bash
sudo apt-get install tree 
```

### **Service and Process Management**

La plupart des distributions Linux ont maintenant adopté systemd. Ce démon est un processus Init démarré en premier et possède donc l'identifiant de processus (PID) 1. Ce démon surveille et s'occupe du démarrage et de l'arrêt ordonné des autres services. Tous les processus ont un PID assigné qui peut être consulté sous /proc/ avec le numéro correspondant. Un tel processus peut avoir un identifiant de processus parent (PPID), et s'il en a un, il est connu comme processus enfant.

En plus de systemctl, nous pouvons également utiliser update-rc.d pour gérer les liens des scripts init SysV.

### Commandes de Base associées à systemd

1. **Vérifier le statut d'un service** :
    
    ```bash
    systemctl status nom_du_service
    ```
    
2. **Démarrer un service** :
    
    ```bash
    sudo systemctl start nom_du_service
    ```
    
3. **Arrêter un service** :
    
    ```bash
    sudo systemctl stop nom_du_service
    ```
    
4. **Redémarrer un service** :
    
    ```bash
    sudo systemctl restart nom_du_service
    ```
    
5. **Activer un service (démarrage automatique au démarrage)** :
    
    ```bash
    sudo systemctl enable nom_du_service
    ```
    
6. **Désactiver un service (ne pas démarrer au démarrage)** :
    
    ```bash
    sudo systemctl disable nom_du_service
    ```
    

**Lister les processus en cours**

```bash
ps -aux | grep ssh
```

- **ps** : Affiche les informations sur les processus en cours.
- **a** : Inclut tous les utilisateurs, pas seulement les processus de l'utilisateur actuel.
- **u** : Affiche des informations détaillées sur les processus.
- **x** : Inclut les processus qui ne sont pas liés à un terminal de contrôle.
- Le pipe grep ssh, nous permet de filtrer pour obtenir que des réponses avec ssh

Autre méthode avec `systemctl`

```bash
systemctl list-units --type=service
```

Les deux commandes sont complémentaires et souvent utilisées ensemble pour obtenir une vue complète de l'état du système. `systemctl list-units` est plus adapté à la gestion des services systemd, tandis que `ps -aux` est plus adapté à l'analyse des processus système individuels.
