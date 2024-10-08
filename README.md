
# GitLab Vagrant Setup

Ce projet Vagrant permet de configurer une machine virtuelle Ubuntu 22.04 avec GitLab Community Edition (CE)  accessible localement via HTTPS sur `localhost:8080`. Ce guide détaille les étapes pour démarrer la VM, accéder à GitLab, ainsi que les différentes configurations incluses dans le fichier `Vagrantfile`.

## Prérequis

- [Vagrant](https://www.vagrantup.com/downloads) installé
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) installé

## Description des Actions Réalisées

Le `Vagrantfile` fourni effectue les actions suivantes :

1. **Configuration de la Machine Virtuelle :**
   - Utilise l'image `generic/ubuntu2204` pour Ubuntu 22.04.
   - Assigne **4 Go de mémoire** et **4 CPU** à la VM pour garantir que GitLab fonctionne correctement.

2. **Configuration Réseau :**
   - Le port **443** (HTTPS) de la VM est redirigé vers le **port 8080** de la machine hôte, permettant d'accéder à GitLab en utilisant `https://localhost:8080`.

3. **Provisionnement de la Machine :**
   - Le provisionnement est effectué via un script Shell exécuté après la création de la VM, incluant les étapes suivantes :
     - **Mise à jour du système** : Télécharge les dernières mises à jour du système pour s'assurer que les paquets sont à jour.
     - **Installation des Dépendances** : Installe les outils nécessaires `curl`, `openssh-server`, `ca-certificates`, `tzdata`, et `perl`.
     - **Ajout du Dépôt GitLab** : Télécharge et ajoute le dépôt officiel GitLab pour permettre son installation.
     - **Installation de GitLab CE** : Installe GitLab Community Edition sur la machine.

4. **Configuration SSL :**
   - **Certificat Auto-Signé** : Crée un certificat SSL auto-signé pour `localhost`.
     - Un certificat et une clé privée sont générés dans `/etc/gitlab/ssl/`.
     - Les permissions sont définies pour protéger les fichiers de certificat.
   - **Configuration de GitLab pour HTTPS** : Le fichier de configuration de GitLab (`/etc/gitlab/gitlab.rb`) est modifié pour :
     - Utiliser `https://localhost` comme URL externe.
     - Utiliser les certificats SSL créés pour sécuriser la connexion.
     - La redirection automatique de HTTP vers HTTPS est désactivée.

5. **Reconfiguration de GitLab :**
   - **Reconfiguration Finale** : Après la modification des fichiers de configuration, `gitlab-ctl reconfigure` est exécuté pour appliquer les changements et démarrer GitLab avec les nouvelles configurations.

## Instructions d'Utilisation

Suivez ces étapes pour démarrer la machine virtuelle et accéder à GitLab :

### 1. Cloner le Projet

Clonez ce dépôt sur votre machine locale :

```sh
git clone <URL_DU_DEPOT>
cd <NOM_DU_DOSSIER>
```

### 2. Démarrer la Machine Virtuelle

Pour démarrer la machine virtuelle, exécutez :

```sh
vagrant up
```

Ce processus peut prendre quelques minutes, car il installe toutes les dépendances et configure GitLab.

### 3. Accéder à GitLab

Une fois la machine virtuelle démarrée, vous pouvez accéder à GitLab via votre navigateur en allant sur :

```
localhost:8080
```

Note : Le certificat SSL est auto-signé, ce qui signifie que votre navigateur affichera probablement un avertissement de sécurité. Vous pouvez contourner cet avertissement en ajoutant une exception pour accéder à GitLab.

### 4. Gérer la Machine Virtuelle

- **Arrêter la VM** : Pour arrêter la machine virtuelle proprement :

  ```sh
  vagrant halt
  ```

- **Redémarrer la VM** : Pour redémarrer la machine après des modifications :

  ```sh
  vagrant reload
  ```

- **Détruire la VM** : Si vous n'avez plus besoin de la machine virtuelle :

  ```sh
  vagrant destroy
  ```

## Informations Complémentaires

- **Login Administrateur GitLab** : Lors du premier accès, vous devrez configurer le mot de passe de l'utilisateur `root`.
Pour accéder au premier mot de passe de la session root :

- **Pour accéder au premier mot de passe root ( n'est actif que 24 h )**:

  ```sh
  vagrant ssh
  sudo cat /etc/gitlab/initial_root_password
  ```

## Dépannage

- **GitLab n'est pas accessible** : Vérifiez si la machine virtuelle est bien en cours d'exécution en exécutant :

  ```sh
  vagrant status
  ```

- **Voir les journaux GitLab** : Pour voir les journaux et comprendre les erreurs éventuelles :

  ```sh
  vagrant ssh
  sudo gitlab-ctl tail
  ```

## Auteur

ROSTOLL Jean-Michel
