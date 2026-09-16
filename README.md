# synparc-installer

> ðŸ“¦ Scripts PowerShell de dÃ©ploiement et logique d'auto-update pour la plateforme Synparc (agent + serveur).

## Stack

- **Langage** : PowerShell 5.1+ (Windows PowerShell et PowerShell Core)
- **Distribution** : GitHub Releases (assets binaires signÃ©s)
- **Cible** : Windows Server 2016+ et Windows 10/11

## Scripts

| Script | Description |
|--------|-------------|
| `Install-SynparcAgent.ps1` | Installe l'agent sur un poste ou serveur Windows |
| `Update-SynparcAgent.ps1` | VÃ©rifie et applique les mises Ã  jour depuis GitHub Releases |
| `Uninstall-SynparcAgent.ps1` | DÃ©sinstalle proprement l'agent et son service Windows |
| `Deploy-SynparcServer.ps1` | DÃ©ploiement du serveur central (Node.js + PostgreSQL + TimescaleDB) |
| `New-EnrollmentToken.ps1` | GÃ©nÃ¨re et enregistre un token d'enrÃ´lement pour une machine |

## Installation rapide de l'agent

```powershell
$enrollmentToken = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
$serverUrl       = "https://synparc.example.com"

Invoke-Expression (Invoke-WebRequest `
    -Uri "https://raw.githubusercontent.com/Synparc/synparc-installer/main/Install-SynparcAgent.ps1").Content
```

## Logique d'auto-update

```
1. GET  https://api.github.com/repos/Synparc/synparc-agent/releases/latest
2. Comparer le tag avec la version locale installÃ©e
3. Si nouvelle version : tÃ©lÃ©charger le binaire signÃ© (.exe)
4. VÃ©rifier la signature (hash SHA256)
5. ArrÃªter le service Windows â†’ remplacer le binaire â†’ redÃ©marrer
```

## PrÃ©requis

- PowerShell 5.1+ ou PowerShell 7+
- Droits administrateur locaux
- AccÃ¨s HTTPS sortant vers GitHub et le serveur Synparc

## DÃ©ploiement en masse (GPO / SCCM)

```powershell
.\Install-SynparcAgent.ps1 `
    -ServerUrl       "https://synparc.example.com" `
    -EnrollmentToken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
    -Silent
```