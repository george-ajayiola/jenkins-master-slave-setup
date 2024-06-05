# Jenkins Controller Agent Setup on GCP: Ubuntu VM


## Introduction
In this part of the series, we will create a new Ubuntu VM instance in GCP to serve as our agent node machine. We will then register this new worker node instance in Jenkins for further use. This guide includes detailed steps to set up and configure the agent node in Jenkins.

## Step-by-Step Guide

### Step 1: Create a New Ubuntu VM Instance for Agent Node
1. **Configuration:**
    - Machine Type: E2 Series, e2-medium
    - Boot Disk: Ubuntu 22.04 LTS, Size: 10 GB

2. **Static IP:**
    - Reserve a static IP address for the agent node instance.

3. **Create Instance:**
    - Click on the Create button to set up the instance.

### Step 2: Connect to Your Worker Agent Instance
- Use SSH to connect: "Open in Browser window".
- Run an update in your VM instance:
    ```sh
    sudo apt-get update -y
    ```

### Step 3: Install Java, Maven, Git, Docker in Agent Instance
1. **Install Java JDK 17:**
    ```sh
    sudo apt-get install openjdk-17-jdk -y
    java --version
    ```

2. **Install Maven:**
    ```sh
    sudo apt install maven -y
    ```

3. **Install Git:**
    ```sh
    sudo apt install git -y
    ```

4. **Install Docker:**
    - Follow the [official Docker documentation for Ubuntu](https://docs.docker.com/engine/install/ubuntu/).
    - Verify the Docker installation:
        ```sh
        sudo docker run hello-world
        ```

### Step 4: Setup Additional Details in Agent Node Instance
1. **Create User and SSH Directory:**
    ```sh
    sudo useradd -m jenkins
    sudo -u jenkins mkdir /home/jenkins/.ssh
    ```

2. **Add Jenkins User to Docker Group:**
    ```sh
    sudo usermod -a -G docker jenkins
    ```

### Step 5: Create SSH Public and Private Keys in Controller Node
1. **Generate SSH Key:**
    ```sh
    ssh-keygen
    ```

2. **Get Public Key:**
    ```sh
    sudo cat ~/.ssh/id_rsa.pub
    ```

### Step 6: Add Public Key Details in Agent Machine
1. **Create Authorized Keys File:**
    ```sh
    sudo -u jenkins vi /home/jenkins/.ssh/authorized_keys
    ```

2. **Paste Public Key:**
    - Copy the public key from the controller node and paste it into the `authorized_keys` file.

3. **Verify SSH Connection:**
    ```sh
    ssh jenkins@agent-publicip
    ```

### Step 7: Register Agent Node Details in Jenkins Configuration from UI
1. **Navigate to Jenkins:**
    - `Manage Jenkins` > `Manage nodes and clouds` > `New Node`

2. **Enter Node Details:**
    - Name: `worker`
    - Remote root directory: `/home/jenkins`
    - Description: `jenkins private key`
    - Labels: `worker`
    - Usage: `Use this node as much as possible`
    - Launch method: `Launch agents via SSH`

3. **Add Credentials:**
    - Username: `jenkins`
    - Private Key: Copy from the controller instance:
        ```sh
        sudo cat ~/.ssh/id_rsa
        ```

4. **Select Host Key Verification Strategy:**
    - `Manually trusted key Verification strategy`
    - Click `Save`.

### Conclusion
Your agent node should now be added, online, and connected for usage in Jenkins.

### References
- [Test Automation: Jenkins Controller Agent setup in GCP: Ubuntu VM](https://medium.com/@nairgirish100/test-automation-jenkins-controller-agent-setup-in-gcp-ubuntu-vm-part-3-ee0d1d874dcc)


