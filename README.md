# ICS344 Project: Offensive Security, Cyber Deception, and SIEM

This project demonstrates offensive security techniques, cyber deception using honeypots, and Security Information and Event Management (SIEM) analysis. The targeted service is **HTTP**, the honeypots are **StrutsHoneypot** and **Cowrie**, and the SIEM platform used is **Wazuh**.

---

## Table of Contents
1. [Phase 1: Setup and Compromise the Service](#phase-1-setup-and-compromise-the-service)
2. [Phase 2: Honeypot Setup and Realism Evaluation](#phase-2-honeypot-setup-and-realism-evaluation)
3. [Phase 3: Visual Analysis with SIEM Dashboard](#phase-3-visual-analysis-with-siem-dashboard)

---

## Phase 1: Setup and Compromise the Service

### Victim Environment (Target: `192.168.56.105`)
1. **Install Apache HTTP Server**:
    ```bash
    sudo apt update && sudo apt install apache2 -y
    sudo systemctl start apache2
    sudo systemctl enable apache2
    ```
2. **Verify Apache Installation**:
    ```bash
    curl http://192.168.56.105
    ```

---

### Attacker Environment (my Machine: `192.168.56.103`)

#### Install and Configure Caldera
1. Clone the repository:
    ```bash
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash –
    sudo apt install -y nodejs
    node -v
    npm -v
    wget https://go.dev/dl/go1.19.13.linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf go1.19.13.linux-amd64.tar.gz
    echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
    source ~/.profile
    go version
    sudo apt install python3 python3-pip -y
    git clone https://github.com/mitre/caldera.git --recursive
    cd caldera
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    python3 server.py --insecure --build
    ```
    
2. Access Caldera: [http://192.168.56.103:8888](http://192.168.56.103:8888).

#### Use Kali Linux Tools
1. **Nmap (Service Scanning)**:
    ```bash
    nmap -sV -p 80 192.168.56.105
    ```
2. **Nikto (HTTP Vulnerability Scanning)**:
    ```bash
    nikto -h http://192.168.56.105
    ```
3. **Metasploit Framework**:
    ```bash
    msfconsole
    use auxiliary/scanner/http/http_version
    set RHOSTS 192.168.56.105
    run
    ```

#### Custom Exploitation Script
- Example Python Script:
    ```python
    import requests

    url = "http://192.168.56.105/vulnerable-endpoint"
    payload = {"command": "malicious_code"}
    response = requests.post(url, data=payload)

    print(response.text)
    ```

---

## Phase 2: Honeypot Setup and Realism Evaluation

### Deploy Honeypots

#### StrutsHoneypot
1. Clone the repository:
    ```bash
    git clone https://github.com/strutshoneypot/strutshoneypot.git
    cd strutshoneypot
    docker-compose up
    ```

#### Cowrie Honeypot
1. Clone the repository:
    ```bash
    git clone https://github.com/cowrie/cowrie.git
    cd cowrie
    virtualenv cowrie-env
    source cowrie-env/bin/activate
    pip install -r requirements.txt
    ```
2. Start Cowrie:
    ```bash
    ./bin/cowrie start
    ```

---

### Replicate Attacks
1. Repeat all tools and scripts used in Phase 1 on both honeypots.
2. Compare results for realism and accuracy.

---

### Evaluate Realism
1. Monitor resource usage:
    ```bash
    top
    ```
2. Analyze honeypot logs:
    ```bash
    tail -f /path/to/honeypot/logs
    ```

---

## Phase 3: Visual Analysis with SIEM Dashboard

### Install Wazuh SIEM

#### Install Wazuh Manager (My Machine: `192.168.56.103`)
1. Add Wazuh repository and install:
    ```bash
    curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
    echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee /etc/apt/sources.list.d/wazuh.list
    apt update && apt install wazuh-manager -y
    ```
2. Start Wazuh Manager:
    ```bash
    systemctl start wazuh-manager
    ```

#### Install Wazuh Agent (Victim & Honeypots: `192.168.56.105`, etc.)
1. Add the repository:
    ```bash
    curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
    echo "deb https://packages.wazuh.com/4.x/apt/ stable main" | tee /etc/apt/sources.list.d/wazuh.list
    apt update && apt install wazuh-agent -y
    ```
2. Configure the agent:
    Edit `/var/ossec/etc/ossec.conf` and set:
    ```xml
    <server>
      <address>192.168.56.103</address>
    </server>
    ```
3. Restart the agent:
    ```bash
    systemctl restart wazuh-agent
    ```

---

### Visualize Logs and Insights
1. Access Wazuh dashboard: [http://192.168.56.103:5601](http://192.168.56.103:5601).
2. Compare logs and visualize key metrics for victim and honeypot activity.


---

## Authors
**Abdulaziz Alghamdi**, **Abdulrahman Alghamdi**, **Ibrahim Almojel**

King Fahd University of Petroleum & Minerals (KFUPM)  
ICS344: Information Security Project, Semester 241  
