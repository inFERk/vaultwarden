# Instalación VaultWarden
  

# **<--- Pre-requisitos --->**  
## Actualizar paquetes
````   
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release nano git
 
````  
## Abrir puertos  
```` 
sudo nano /etc/iptables/rules.v4
```` 
Agregar arriba de: -A INPUT -j REJECT
```` 
-A INPUT -p tcp --dport 8080 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
-A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
-A INPUT -p tcp --dport 81 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
-A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
````  
  
```` 
sudo iptables-restore < /etc/iptables/rules.v4
````  

# **<--- Docker --->**

  
## Instalar llaves GPG de docker para Debian Y Ubuntu  
````  
sudo mkdir -p /etc/apt/keyrings
```` 
  
```` 
curl -fsSL https://download.docker.com/linux/$(lsb_release -is | awk '{print tolower($0)}')/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
````  
  
## Agregando Repositorios  
Debian  o Ubuntu
```` 
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/$(lsb_release -is | \
  awk '{print tolower($0)}')   $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```` 

  
## Instalando Docker
```` 
sudo apt update 
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```` 
## Agregando el grupo Docker  
```` 
sudo groupadd docker  
```` 
  
## Agregar el usuario actual al grupo Docker
```` 
sudo usermod -aG docker $USER  
```` 
## Instalar Docker Compose
```` 
sudo curl -SL https://github.com/docker/compose/releases/download/v2.11.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose  
```` 
  
## Hacer Ejecutable docker-compose
```` 
sudo chmod +x /usr/local/bin/docker-compose
```` 
## Paso Final
  Salga del sistema y vuelva a entrar. Luego ejecute
```` 
docker-compose -v
docker -v
docker ps
```` 
  
  
 # **<--- NGINX Proxy Manager, MariaDB y Vaultwarden --->**  
  
## Descargar el repositorio en el que viene el archivo para docker
```` 
sudo mkdir /opt/vw
git clone https://github.com/inFERk/vaultwarden.git
cd vaultwarden
```` 

## Instalar todo
```` 
docker-compose up -d
```` 
  
abrir el administrador desde la dirección http://localhost:81
```` 
Email: admin@example.com
Password: changeme
```` 
