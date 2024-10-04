---------------------------------------
# Création d'utilisateur en détail : 
---------------------------------------

1. Assurez-vous que les outils de gestion Active Directory sont installés sur votre serveur ou machine, et que vous disposez des droits nécessaires pour ajouter des utilisateurs dans Active Directory.
2. Pour ajouter un utilisateur dans Active Directory à l'aide de PowerShell,je vous présente une commande typique et complète que vous pouvez utiliser (tous les attributs ne sont pas obligatoires):

```powershell
New-ADUser -Name "NomCompletUtilisateur" -GivenName "Prénom" -Surname "Nom" -SamAccountName "NomUtilisateur" -UserPrincipalName "NomUtilisateur@domaine.local" -Path "OU=Utilisateurs,DC=domaine,DC=local" -AccountPassword (ConvertTo-SecureString "MotDePasseComplexe" -AsPlainText -Force) -Enabled $true
```

Voici une explication détaillée des paramètres utilisés dans la commande :

- **-Name** : Le nom complet de l'utilisateur.
- **-GivenName** : Le prénom de l'utilisateur.
- **-Surname** : Le nom de famille de l'utilisateur.
- **-SamAccountName** : Le nom d'utilisateur pour la connexion.
- **-UserPrincipalName** : L'adresse e-mail ou le nom principal de l'utilisateur.
- **-Path** : Le chemin de l'OU (Organizational Unit) où vous voulez créer l'utilisateur.
- **-AccountPassword** : Définit le mot de passe de l'utilisateur. Utilisez la commande `ConvertTo-SecureString` pour entrer le mot de passe en toute sécurité.
- **-Enabled** : Active le compte utilisateur dès sa création.

### Exemple
Pour créer un utilisateur "Jean Dupont" avec le nom de connexion "jdupont", voici un exemple :

```powershell
New-ADUser -Name "Jean Dupont" -GivenName "Jean" -Surname "Dupont" -SamAccountName "jdupont" -UserPrincipalName "jdupont@exemple.local" -Path "OU=Utilisateurs,DC=exemple,DC=local" -AccountPassword (ConvertTo-SecureString "MotDePasseSecurise2024!" -AsPlainText -Force) -Enabled $true
```

