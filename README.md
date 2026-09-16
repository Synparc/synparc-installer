# synparc-installer

> 📦 Scripts PowerShell de déploiement et logique d'auto-update pour la plateforme Synparc (agent + serveur).

## Stack

- **Langage** : PowerShell 5.1+ (Windows PowerShell et PowerShell Core)
- **Distribution** : GitHub Releases (assets binaires signés)
- **Cible** : Windows Server 2016+ et Windows 10/11

## Scripts

| Script | Description |
|--------|-------------|
| `Install-SynparcAgent.ps1` | Installe l'agent sur un poste ou serveur Windows |
| `Update-SynparcAgent.ps1` | Vérifie et applique les mises à jour depuis GitHub Releases |
| `Uninstall-SynparcAgent.ps1` | Désinstalle proprement l'agent et son service Windows |
| `Deploy-SynparcServer.ps1` | Déploiement du serveur central (Node.js + PostgreSQL + TimescaleDB) |
| `New-EnrollmentToken.ps1` | Génère et enregistre un token d'enrôlement pour une machine |

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
2. Comparer le tag avec la version locale installée
3. Si nouvelle version : télécharger le binaire signé (.exe)
4. Vérifier la signature (hash SHA256)
5. Arrêter le service Windows -> remplacer le binaire -> redémarrer
```

## Prérequis

- PowerShell 5.1+ ou PowerShell 7+
- Droits administrateur locaux
- Accès HTTPS sortant vers GitHub et le serveur Synparc

## Déploiement en masse (GPO / SCCM)

```powershell
.\Install-SynparcAgent.ps1 `
    -ServerUrl       "https://synparc.example.com" `
    -EnrollmentToken "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" `
    -Silent
```
