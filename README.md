# Ansible-Windows

# Create HTTPS listener
By default when you run winrm quickconfig command WinRM is only configured for HTTP (port 5985). You can check already registered listeners by running following command.

$   WinRM e winrm/config/listener
 
```
PS C:\Users\Administrator> WinRM e winrm/config/listener
Listener
    Address = *
    Transport = HTTP
    Port = 5985
    Hostname
    Enabled = true
    URLPrefix = wsman
    CertificateThumbprint
    ListeningOn = 127.0.0.1, 192.168.122.230, ::1
```
We can enable to WinRM HTTPS listener by running  powershell script :   ConfigureRemotingForAnsible.ps1


After running the powershell script we can verify:

```
PS C:\Users\Administrator> WinRM e winrm/config/listener
Listener
    Address = *
    Transport = HTTP
    Port = 5985
    Hostname
    Enabled = true
    URLPrefix = wsman
    CertificateThumbprint
    ListeningOn = 127.0.0.1, 192.168.122.230, ::1

Listener
    Address = *
    Transport = HTTPS
    Port = 5986
    Hostname = WINAD
    Enabled = true
    URLPrefix = wsman
    CertificateThumbprint = B984DBFDDAB89B2E822CA585CF15FEE243514EF2
    ListeningOn = 127.0.0.1, 192.168.122.230, ::1

```

#next on ansible controller node we have to install pywinrm package:

pip install pywinrm 

#next we have to create hosts file with these variables:

```
$ cat hosts
[win]
192.168.122.230 

[win:vars]
ansible_user=Administrator
ansible_password=password
ansible_connection=winrm
ansible_winrm_server_cert_validation=ignore
```

We can verfiy the connection by running ping 

```
$ ansible -m win_ping -i hosts all 
192.168.122.230 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

```



