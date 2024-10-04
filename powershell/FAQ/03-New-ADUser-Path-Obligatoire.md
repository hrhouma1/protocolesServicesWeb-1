# **Question :**

*Lors de l'utilisation de la commande PowerShell `New-ADUser` pour créer un utilisateur, si je spécifie une OU dans le paramètre `-Path` (par exemple, `"OU=Utilisateurs,DC=votredomaine,DC=com"`), celle-ci doit-elle déjà exister dans Active Directory ? Si l'OU n'existe pas, est-elle automatiquement créée par la commande ou cela provoque-t-il une erreur ?*

# Réponse

Non, la commande PowerShell `New-ADUser` **ne créera pas** automatiquement l'Unité Organisationnelle (OU) ou les conteneurs spécifiés dans le paramètre `-Path`. Si l'OU que vous spécifiez (par exemple, `"OU=Utilisateurs,DC=votredomaine,DC=com"`) **n'existe pas déjà**, la commande échouera avec une erreur indiquant que l'OU n'a pas été trouvée.

### Ce qui se passe si l'OU n'existe pas :
Si vous tentez de créer un utilisateur avec un chemin (`-Path`) vers une OU ou un conteneur qui n'existe pas encore, PowerShell renverra une erreur du type :
```plaintext
New-ADUser : Cannot find an object with identity: 'OU=Utilisateurs,DC=votredomaine,DC=com'
```

### Solution :
Vous devez vous assurer que l'OU (Unité Organisationnelle) ou le conteneur spécifié existe **avant** d'exécuter la commande pour créer l'utilisateur. Si l'OU n'existe pas, vous pouvez la créer au préalable avec la commande `New-ADOrganizationalUnit` :

#### Exemple pour créer une OU avant d'ajouter l'utilisateur :
```powershell
New-ADOrganizationalUnit -Name "Utilisateurs" -Path "DC=votredomaine,DC=com"
```

Ensuite, vous pouvez exécuter la commande `New-ADUser` pour ajouter l'utilisateur dans cette OU nouvellement créée.

#### Exemple complet :
1. Créer l'OU :
   ```powershell
   New-ADOrganizationalUnit -Name "Utilisateurs" -Path "DC=votredomaine,DC=com"
   ```

2. Créer l'utilisateur dans cette OU :
   ```powershell
   New-ADUser -Name "John Doe" -SamAccountName "jdoe" -UserPrincipalName "jdoe@votredomaine.com" -Path "OU=Utilisateurs,DC=votredomaine,DC=com" -AccountPassword (ConvertTo-SecureString "MotDePasseComplexe!" -AsPlainText -Force) -Enabled $true
   ```

En résumé, le chemin `-Path` doit déjà exister avant l'exécution de la commande `New-ADUser`. Si l'OU n'existe pas, vous devez la créer manuellement avec `New-ADOrganizationalUnit`.
