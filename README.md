# Jenkins EC2 Orchestrator

Install Jenkins, configure Docker as an agent, set up CI/CD, deploy applications to Kubernetes, and much more.

---

## AWS EC2 Instance Setup

1. **Go to AWS Console**
2. **Navigate to:**
    - Instances (running)
    - Launch instances

![Launch EC2 Instance](https://github.com/user-attachments/assets/d1928875-62c9-413f-8d38-c9c6b86d189a)

---

## Install Jenkins

### Pre-Requisites

- Java (JDK)

### Install Java

```bash
sudo apt update
sudo apt install openjdk-17-jre
```

### Verify Java Installation

```bash
java -version
```

---

### Install Jenkins

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

> **Note:**  
> By default, Jenkins will not be accessible to the external world due to inbound traffic restrictions by AWS.  
> Open port `8080` in the inbound traffic rules as shown below.  
> You can just allow TCP 8080 as well (in my case, I allowed "All traffic").

1. EC2 > Instances > Click on your instance
2. In the bottom tabs -> Click on **Security**
3. Security groups
4. Add inbound traffic rules as shown in the image

<img width="1223" alt="Security Group Inbound Rule" src="https://github.com/user-attachments/assets/3d7aea51-e728-466d-92b3-c13c7a19ec27" />

---

## Login to Jenkins

Visit:

```
http://<ec2-instance-public-ip-address>:8080
```
_Get the public IP address from your AWS EC2 console._

> **Note:**  
> If you are not interested in allowing "All Traffic" to your EC2 instance:  
> 1. Delete the inbound traffic rule for your instance  
> 2. Edit the inbound traffic rule to only allow custom TCP port `8080`

---

### Initial Jenkins Setup

After logging in:

1. Run the command to copy the Jenkins Admin Password:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

2. Enter the Administrator password.

![Jenkins Initial Password](https://github.com/user-attachments/assets/48fadf33-12cb-4dae-b8b2-d7d578f4ebe9)

---

### Install Suggested Plugins

Click **Install suggested plugins** when prompted.

![Install Plugins](https://github.com/user-attachments/assets/0f441891-6a17-488f-998d-099008dca3dd)

Wait for Jenkins to install suggested plugins:

![Plugin Installation Progress](https://github.com/user-attachments/assets/6ccf1d49-7cb8-49cb-b1f2-8fb0af953f97)

---

### Create Admin User

Create your first admin user or skip the step.  
_If you want to use this Jenkins instance for future use-cases as well, it is better to create an admin user._

![Create Admin User](https://github.com/user-attachments/assets/75a0f0fe-ea41-4a3c-8eac-abc84966766a)

---

### Jenkins Installation is Successful

You can now start using Jenkins.

![Jenkins Ready](https://github.com/user-attachments/assets/7d8bd697-6928-4079-a967-5166fe82da23)

---

## Install the Docker Pipeline Plugin in Jenkins

1. Log in to Jenkins.
2. Go to **Manage Jenkins > Manage Plugins**.
3. In the **Available** tab, search for **Docker Pipeline**.
4. Select the plugin and click **Install**.
5. Restart Jenkins after the plugin is installed.

---

## Docker Slave Configuration

### Install Docker

```bash
sudo apt update
sudo apt install docker.io
```

### Grant Jenkins and Ubuntu Users Permission to Docker Daemon

```bash
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

---

Once you are done with the above steps, it is better to restart Jenkins:

```
http://<ec2-instance-public-ip-address>:8080/restart
```

---

The Docker agent configuration is now successful!
