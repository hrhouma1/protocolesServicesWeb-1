
----------------------------
# Partie 1
----------------------------

- Le cmdlet PowerShell **`New-ADOrganizationalUnit`** permet de créer une nouvelle unité organisationnelle (OU) dans Active Directory. 
- Dans l'expression **`DC=votredomaine,DC=com`**, il y a deux composants **DC** car ils représentent respectivement les deux parties du nom de domaine complet **votredomaine.com**.

- Je vous présente une explication détaillée :

1. **`DC=votredomaine`** : Ce premier **DC** correspond à la partie "votredomaine" du nom de domaine. Dans ce cas, **DC** signifie **Domain Component**, et "votredomaine" représente le nom du domaine que vous utilisez dans votre infrastructure Active Directory.

2. **`DC=com`** : Le second **DC** correspond à la partie **".com"** du nom de domaine. Le **com** est la composante finale du domaine, ce qu'on appelle un **TLD** (Top-Level Domain), comme `.com`, `.net`, ou `.org`. 

### Pourquoi deux **DC** ?

Active Directory utilise un système de dénomination basé sur les noms de domaine complets (**Fully Qualified Domain Name**, ou **FQDN**). Chaque partie du nom de domaine est spécifiée par un **DC** :

- **votredomaine.com** se décompose en deux parties : 
   - **votredomaine** (le nom du domaine),
   - **com** (le TLD, ou extension du domaine).

Ainsi, pour spécifier un domaine complet dans Active Directory, il est nécessaire d'utiliser les deux composantes sous la forme `DC=votredomaine,DC=com`.

### Exemple :
Pour un domaine appelé **monentreprise.org**, cela se représenterait comme :

```powershell
-Path "DC=monentreprise,DC=org"
```

En résumé, chaque **DC** permet de séparer les différentes parties du nom de domaine, et le fait d'en avoir deux est simplement dû à la structure hiérarchique du nom de domaine.

-------------------
# Partie 2
----------------------------

- Je vous présente plusieurs exemples de chemins LDAP pour des domaines avec différentes extensions (**TLDs**) et noms de domaine spécifiques. 
- Chaque **DC** représente une partie du nom de domaine.

### 1. **Domaine avec .org :**
   Pour un domaine comme **"association.org"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=association,DC=org"
   ```

### 2. **Domaine avec .fr :**
   Pour un domaine en France comme **"entreprise.fr"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=entreprise,DC=fr"
   ```



### 3.1. **Domaine avec .de (Allemagne) :**
   Pour un domaine en Allemagne comme **"institut.de"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=institut,DC=de"
   ```

### 3.2 **Domaine avec .jp (Japon) :**
   Pour un domaine au Japon comme **"universite.jp"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=universite,DC=jp"
   ``` 

###  3.3 **Domaine avec .tn (Tunisie) :**
   Pour un domaine en Tunisie comme **"institut.tn"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=institut,DC=tn"
   ```

###  3.4  - Pour un domaine au Sénégal, l'extension de domaine est **.sn**. Je vous présente un exemple de chemin LDAP pour un domaine comme **"universite.sn"** :

```powershell
-Path "DC=universite,DC=sn"
```

- Dans ce cas, **universite** est le nom du domaine, et **sn** représente l'extension pour le Sénégal.



### 4. **Domaine avec plusieurs sous-domaines :**
   Pour un domaine plus complexe comme **"cmaisonneuve.qc.ca"**, où **"qc"** représente le code de la région (Québec) et **"ca"** le pays (Canada), voici comment se décompose le chemin LDAP :

   ```powershell
   -Path "DC=cmaisonneuve,DC=qc,DC=ca"
   ```

   Chaque partie du nom de domaine est séparée par un **DC**, y compris les sous-domaines (dans cet exemple, **qc**).

### Autres exemples pour des domaines spécifiques :

5. **Domaine avec sous-domaine dans .com :**
   Pour un sous-domaine comme **"blog.entreprise.com"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=blog,DC=entreprise,DC=com"
   ```

### 6. **Domaine avec .gov :**
   Pour un domaine gouvernemental comme **"etat.gov"**, le chemin LDAP serait :

   ```powershell
   -Path "DC=etat,DC=gov"
   ```

### Principe général :
Le principe reste le même, quel que soit le TLD utilisé : chaque composant du nom de domaine est séparé par un **DC** dans le chemin LDAP, en suivant l'ordre hiérarchique du nom de domaine complet (FQDN).




---------------------
# Partie 3
----------------------------

Le cmdlet PowerShell **`New-ADOrganizationalUnit`** permet de créer une nouvelle unité organisationnelle (OU) dans Active Directory. 

Je vous présente une explication des deux paramètres mentionnés :

1. **`-Name "Utilisateurs"`** : 
   Ce paramètre définit le nom de l'unité organisationnelle que vous voulez créer. Dans cet exemple, le nom de l'OU sera "Utilisateurs". C’est le libellé qui apparaîtra dans la hiérarchie d’Active Directory et servira de conteneur pour organiser des objets tels que des utilisateurs, des groupes ou des ordinateurs.

2. **`-Path "DC=votredomaine,DC=com"`** : 
   Ce paramètre indique l'emplacement (ou chemin LDAP) dans la hiérarchie d'Active Directory où l'unité organisationnelle sera créée. Le `DC=votredomaine,DC=com` spécifie le domaine racine où l'OU sera ajoutée. Le `DC` signifie **"Domain Component"**, et chaque `DC` correspond à une partie du nom de domaine. Par exemple, dans un domaine nommé "votredomaine.com", `DC=votredomaine,DC=com` fait référence à ce domaine.

### Exemple complet :
```powershell
New-ADOrganizationalUnit -Name "Utilisateurs" -Path "DC=votredomaine,DC=com"
```

Cela crée une OU nommée **"Utilisateurs"** à la racine du domaine **votredomaine.com**.
