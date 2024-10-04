# Connaître les options disponibles pour une commande spécifique

- Dans PowerShell avec Active Directory, pour connaître les options disponibles pour une commande spécifique, vous pouvez utiliser les méthodes suivantes :

### 1. Utilisation de `-help` ou `-?`
Vous pouvez obtenir une aide rapide pour une commande Active Directory en ajoutant `-help` ou `-?`.

Exemple pour la commande `New-ADUser` :
```powershell
New-ADUser -help
```
ou
```powershell
New-ADUser -?
```

### 2. Utilisation de `Get-Help`
PowerShell fournit également une commande appelée `Get-Help` pour obtenir des informations plus détaillées sur les cmdlets (commandes PowerShell).

Exemple pour `New-ADUser` :
```powershell
Get-Help New-ADUser
```

Pour des informations encore plus détaillées (comme des exemples), utilisez le paramètre `-Full` :
```powershell
Get-Help New-ADUser -Full
```

Ou, pour obtenir uniquement des exemples d'utilisation :
```powershell
Get-Help New-ADUser -Examples
```

### Exemple avec `Get-Help` sur la commande `Get-ADUser` :
```powershell
Get-Help Get-ADUser -Full
```

Cela affichera une explication complète de la commande, y compris les paramètres disponibles, leurs descriptions et des exemples d'utilisation.
