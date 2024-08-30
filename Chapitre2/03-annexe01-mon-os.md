# Mon OS ?
Pour connaître la version du système d'exploitation utilisé sur une machine Linux, il existe plusieurs commandes que vous pouvez exécuter. 

### 1. Utilisation de `lsb_release`
C'est l'une des méthodes les plus courantes et les plus simples pour vérifier la version de la distribution Linux.

- **Commande :**
  ```bash
  lsb_release -a
  ```

  **Sortie attendue :**
  ```
  Distributor ID: Ubuntu
  Description:    Ubuntu 20.04.2 LTS
  Release:        20.04
  Codename:       focal
  ```

### 2. Lecture du fichier `/etc/os-release`
Ce fichier contient des informations détaillées sur la distribution Linux installée.

- **Commande :**
  ```bash
  cat /etc/os-release
  ```

  **Sortie attendue :**
  ```
  NAME="Ubuntu"
  VERSION="20.04.2 LTS (Focal Fossa)"
  ID=ubuntu
  ID_LIKE=debian
  PRETTY_NAME="Ubuntu 20.04.2 LTS"
  VERSION_ID="20.04"
  ```

### 3. Utilisation de `uname`
La commande `uname` fournit des informations sur le noyau, mais elle peut aussi donner une idée générale du système.

- **Commande pour la version du noyau :**
  ```bash
  uname -r
  ```

  **Sortie attendue :**
  ```
  5.4.0-74-generic
  ```

- **Commande pour des informations complètes :**
  ```bash
  uname -a
  ```

  **Sortie attendue :**
  ```
  Linux hostname 5.4.0-74-generic #83-Ubuntu SMP Tue Jun 8 11:15:29 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
  ```

### 4. Utilisation de `cat` sur `/etc/issue`
Le fichier `/etc/issue` contient également des informations sur la version de la distribution.

- **Commande :**
  ```bash
  cat /etc/issue
  ```

  **Sortie attendue :**
  ```
  Ubuntu 20.04.2 LTS \n \l
  ```

### 5. Utilisation de `hostnamectl`
Cette commande est utile pour afficher des informations sur l'hôte, y compris la version du système d'exploitation.

- **Commande :**
  ```bash
  hostnamectl
  ```

  **Sortie attendue :**
  ```
  Static hostname: hostname
  Icon name: computer-vm
  Chassis: vm
  Machine ID: xxxxxxx
  Boot ID: xxxxxxx
  Virtualization: kvm
  Operating System: Ubuntu 20.04.2 LTS
  Kernel: Linux 5.4.0-74-generic
  Architecture: x86-64
  ```

Ces commandes vous fourniront des informations détaillées sur la version du système d'exploitation que vous utilisez.
