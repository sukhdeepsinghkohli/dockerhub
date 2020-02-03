# ansible-practise-centos7-ssh-enabled

ansible-practise-centos7-ssh-enabled is a SSH Enabled container running on centos 7. It should be used for Test and Dev purposes ONLY!.
This is useful for practising ansible exercises. **SystemD enabled**.

Almost all the images I used would give me the following error 
```
failed to get D-Bus connection: Operation not permitted 
```
# Using the docker image
```
docker run -dP -v /sys/fs/cgroup:/sys/fs/cgroup:ro local/c7-systemd-httpd sukhdeepsinghkohli/centos7-sshd
```

# Identify the Internal IP
```
docker inspect <container-id-name>
```

# SSH
```
ssh <container-ip>

Username: root
Password: Passw0rd
```

# Create the docker inventory
```
cat ~/inventory-docker.txt
target1 ansible_host=172.17.0.2 ansible_ssh_pass=Passw0rd
target2 ansible_host=172.17.0.3 ansible_ssh_pass=Passw0rd
target3 ansible_host=172.17.0.4 ansible_ssh_pass=Passw0rd
```

# Sample ansible play
```
ansible all -i ~/inventory-docker.txt -m ping
```
Based on : https://docs.docker.com/engine/examples/running_ssh_service/
