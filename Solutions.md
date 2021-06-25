Task -  PowerShell From Linux to Windows and Windows to Linux 

- Linux can now manage Windows or Windows can manage Linux.
- Create connection between Linux & Windows.


1. Ubuntu Settings

- you'll first need to import the keys and then download the package.

```
 curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
 curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/microsoft.list
```

- Now Update the cache

```
 sudo apt-get update
```

-  Install PowerShell

```
 sudo apt-get install -y powershell
```

- Start Powershell

```
pwsh
```

-  If you haven't already got ssh server running now's your chance to install it.

```
 sudo apt install openssh-client
  sudo apt install openssh-server
```

-  You have to now add some lines to the config

```
 sudo nano /etc/ssh/sshd_config
```

-  Make sure to enable the following lines:

```
PasswordAuthentication yes
RSAAuthentication yes
PubkeyAuthentication yes
```

-  Add the following line to the Subsystem section:

```
 Subsystem powershell powershell -sshs -NoLogo -NoProfile
```

-  Safe the changes and restart ssh server for changes to take effect.

```
 sudo service sshd restart
```


2. Windows Setting

-  First of all download both the Powershell and the Win32 or Win64 version of OpenSSH

```
 https://github.com/PowerShell/Win32-OpenSSH/releases
 https://github.com/PowerShell/PowerShell
```

- For the open-ssh extract to

```
C:\Program Files\OpenSSH or another directory remember the location as you'll need this later.
```

- Install the Powershell 6 by following the prompts.

```
Download windows (x64) .msi [stable binary]
```

-  Inside the extracted Open-ssh folder will be an install-sshd.ps1 script, run it by using the following PowerShell command from the same directory.

```
 powershell -executionpolicy bypass -file install-sshd.ps1
```

-  Next while still in the Open-SSH folder create a key.

```
  .\ssh-keygen.exe -A
  [You can also execute it through GUI & It will create your ssh keys]
```

-  Now you will need to allow SSH connection into your server.

```
 New-NetFirewallRule -Protocol TCP -LocalPort 22 -Direction Inbound -Action Allow -DisplayName SSH
```

-  Next we are setting the Open-SSH to start automatically

```
Set-Service sshd -StartupType Automatic
Set-Service ssh-agent -StartupType Automatic
```

-  Finally, we add some lines to the sshd_config file, open it in notepad and make sure the three following lines are unhashed.

```
PasswordAuthentication yes
RSAAuthentication yes
PubkeyAuthentication yes
[You have to add RSAAuthentication by yourself]
```

-  You will also need to add a line under the subsystem next to sftp, now please keep in mind that your powershell 6 path might change if the release is earlier or later than the one I was using so adapt it as needed.

```
 Subsystem powershell C:/Program Files/PowerShell/6.0.2/pwsh.exe -sshs -NoLogo -NoProfile
```

-  Safe the change and now set a user or system environment variable for Open-SSH

```
 setx PATH "C:\Program Files\OpenSSH" /M
```

-  Restart the Open-SSH for changes you've make to take affect

```
Stop-Service sshd
Start-Service sshd
```

-  Now open open Powershell 6 and final step set the remoting by running script

```
 .\Install-PowerShellRemoting.ps1
```



FINALLY YOU HAVE CONFIGURED BOTH THE MACHINE & NOW TRY TO CONNECT THEM THROUGH SSH.

ssh -i <key-name> user@IP-address












































 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
