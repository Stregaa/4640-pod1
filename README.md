# 4640-pod1

Setting up the 2 VMs (Droplets) on DigitalOcean:  
** We will be using DigitalOcean because the process for creating VMs is easier than in AWS. **  
1. Click Create -> Droplets  
2. Choose Ubuntu 22.04, Basic, Regular with SSD, the lowest pricing option (512 MB), and the San Fransisco 3 region.  
3. Either choose an existing SSH key you have or generate a new one.  
4. Give the droplet a name and click Create Droplet.  
5. Repeat the steps 1-4, except choose Rocky 9.0 instead for step 2.  
6. Generate a new API token by going to API on the sidebar and clicking Generate New Token  
7. Add ```export DO_API_TOKEN=[API token]``` to .profile.  
8. Run ```source .profile```.  
9. Edit the ansible config using ```sudo nano ansible.cfg```.  
```
[defaults]  
host_key_checking = False  
inventory = inventory

[inventory]
enabled_plugins = community.digitalocean.digitalocean
```

10. Inside an [inventory] directory, add a new file [hosts.digitalocean.yml]  
```
plugin: community.digitalocean.digitalocean
attributes:
  - id
  - name
  - memory
  - vcpus
  - disk
  - size
  - image
  - networks
  - volume_ids
  - tags
  - region
compose:
  ansible_host: do_networks.v4 | selectattr('type','eq','public')
    | map(attribute='ip_address') | first
```

11. Test the connection to the droplets using:  
```ansible -m ping -u root all```  
and  
```ansible-inventory --graph```  
![graph](https://user-images.githubusercontent.com/64290337/198861661-fa0ffa01-0f7a-4bfe-aaa1-e6c1307deeb7.png)

Bash commands for Task1:  
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

