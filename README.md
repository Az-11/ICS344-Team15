# ICS344-Team15
Project Code 

## Install HTTP Service
### Apache
sudo apt update && sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
curl http://192.168.56.105

## Install Caldera 
sudo apt install git -y
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
