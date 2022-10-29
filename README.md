# 4640-pod1

Bash:  
To add a user:  
```adduser [username]```

To check that bash is their login shell:  
```sudo nano /etc/passwd```

To give the user sudo privileges:  
```sudo nano /etc/sudoers```  
Make sure %wheel ALL=(ALL) ALL or %sudo ALL=(ALL:ALL) ALL are uncommented (remove #)

Add the user to the wheel or sudo group:  
```usermod -aG wheel [username]```  
```usermod -aG sudo [username]```

Copy the .ssh directory to the /home directory of the new user:  
```sudo cp -r .ssh /home/[username]```

Change ownership of the copied .ssh directory to the new user:  
```sudo chown [username]:[username] .ssh```

Change ownership of the authorized_keys file in the .ssh directory to the new user:  
```sudo chown [username]:[username] authorized_keys```

Remove ssh privileges for root:  
```sudo nano /etc/ssh/sshd_config```  
Change [PermitRootLogin yes] to [PermitRootLogin no]

Restart the ssh daemon:  
```sudo systemctl restart sshd```

