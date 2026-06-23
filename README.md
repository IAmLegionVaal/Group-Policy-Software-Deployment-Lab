# Group Policy Software Deployment Lab

![Status](https://img.shields.io/badge/Project-Completed-success)
![Group Policy](https://img.shields.io/badge/Management-Group%20Policy-0078D4)
![Deployment](https://img.shields.io/badge/Focus-MSI%20Deployment-2F6FED)

A completed Windows domain lab in which I deployed an MSI package to domain computers through Group Policy and validated installation, targeting and troubleshooting behavior.

> **Status:** Completed and validated. This repository documents work already performed in a private lab. Real software names, share paths, domain names, computer names and credentials have been generalized.

## Objectives completed

- Prepared a software-distribution share
- Configured share and NTFS permissions for domain computers
- Created a computer-targeted software deployment GPO
- Assigned an MSI package using a UNC path
- Linked the GPO to the target computer OU
- Restarted clients and validated installation
- Reviewed policy and installer event logs
- Tested scope and troubleshooting scenarios
- Documented findings

## Implementation summary

1. Placed the MSI package in a dedicated network share.
2. Granted the required read access to the target computer accounts.
3. Created a Group Policy Object for software installation.
4. Added the package under Computer Configuration using its UNC path.
5. Linked the GPO to the OU containing the target computers.
6. Refreshed policy and restarted the domain endpoint.
7. Confirmed that the application installed before or during computer startup processing.
8. Reviewed `gpresult`, Group Policy logs and Windows Installer events.
9. Corrected share-access and targeting problems identified during testing.

## Validation commands

```powershell
gpupdate /force
gpresult /h "$env:USERPROFILE\Desktop\SoftwareDeployment-GPResult.html"
Get-Package
Get-WinEvent -LogName Application -MaxEvents 50 |
    Where-Object ProviderName -eq 'MsiInstaller'
```

```cmd
wmic product get name,version
```

## Outcome

The selected MSI package installed successfully on the targeted domain computer through Group Policy. Computers outside the linked OU did not receive the deployment, confirming that the scope was controlled correctly.

## Findings

- The MSI had to be referenced through a UNC path; using a local drive path made the package unavailable to remote computer accounts.
- Domain Computers required read access to both the share and the underlying NTFS folder.
- Computer-assigned software normally depended on startup processing, so `gpupdate` alone did not always complete installation immediately.
- A successful GPO link did not prove package accessibility; testing the distribution share from the target security context was essential.
- Windows Installer and Group Policy operational logs provided clearer evidence than waiting for the application to appear visually.
- Narrow OU targeting reduced the risk of accidentally deploying software to every domain computer.

## Skills demonstrated

- Group Policy software installation
- MSI deployment
- SMB and NTFS permission configuration
- Computer-based GPO targeting
- Windows Installer event analysis
- Domain client validation
- Deployment troubleshooting
- Technical documentation

## Security notes

- No proprietary installer or licensed software is stored in this repository.
- All domain and share details are generalized.
- The lab was completed in a controlled non-production environment.

## Author

**Dewald Pretorius** — L2 IT Support Engineer
