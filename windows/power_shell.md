# Setting up PowerShell
_the way I like it_

Get PowerShell v4.0:

    https://www.microsoft.com/en-us/download/details.aspx?id=40855
    
Install Git (if its not already installed)

    http://git-scm.org
    http://www.imtraum.com/blog/streamline-git-with-powershell/

Install PsGet
```PowerShell
(new-object Net.WebClient).DownloadString("http://psget.net/GetPsGet.ps1") | iex
```
Install Posh-Git
```PowerShell
Install-Module Posh-Git –force
```
Setup SSH keys

    https://help.github.com/articles/generating-ssh-keys/
    
Setup git global config

```
git config --global user.name "Kevin Coffey"
git config --global user.email "kevdougful@gmail.com"
git config --global color.ui auto
```

Black background, white text, Lucida Console 14pt
