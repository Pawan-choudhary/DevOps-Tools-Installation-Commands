# Jenkins Installation
1. Create a executable script using command 'vi jenkins_installation.sh' and paste below command
   
``` bash
sudo apt install openjdk-17-jre-headless -y
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```

2. give executble permission and run the script
   ``` bash
   chmod +x vi jenkins_installation.sh
   ./vi jenkins_installation.sh

3. Connet to Jenkins server using ec2-IP:8080

# Jenkins Master-Slave Setup
## Procedure: 
1. Goto Manage Nodes    
   - Manage Jenkins --> Manage Nodes and Clouds --> New Node  

2. Add the node name as Permanent Agent  
   
3. Provide below information to add Jenkins agent  
   **Name:** uniquely identifies an agent within this Jenkins installation  
   **Description:** <Description>  
   **Number of executors:** 2  
   **Remote root directory:** /home/ubuntu/jenkins-agent   
   **Labels:** Labels (or tags) are used to group multiple agents into one logical group.  
   **Usage:**  
   - Use this node as much as possible  
   - Only build jobs with label expressions matching this node   
 
   **Launch method:**   
   	  - Launch agent by SSH
   	  - Enter HOST IP : Get HOST IP by running below command on agent machine
   	    ``` bash
        hostname -i
   	    ```
      - Add credentials : Get pulbic key and private key by running below commands on agent machine
        ``` bash
        ssh-keygen
        ```
      - copy the private key< .pub>
  
      - Go to credentials--> Jenkins--> Add credentials--> Domain=Global--> Kind--> SSH username with key--> Scop=Global --> ID=devopstest --> usernmae=ubuntu
      - Privatekey --> Enter Directly --> Paste private key here --> Add and select credentials
      - Host Key Verification Stategy = Non verifying
      - Save
      - Go to agent machine--> Copy paste the public key at the end in authorized_keys
           ``` bash
           cat public_key # Copy
           vi authorized_keys # Paste at the end in new line 
           ```

      - Go to jenkins UI --> Bring this node back online --> Lanuch Agent
