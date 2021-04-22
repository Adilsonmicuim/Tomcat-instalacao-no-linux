# Como instalar o Apache Tomcat 9 no Ubuntu 18.* 

--------------------------------------------------------------------
## Passo 1 — Instalar o Java
### Primeiramente, atualize seu índice de pacotes da ferramenta apt:
```bash
sudo apt update
```
### Então, instale o pacote Java Development Kit com a apt:
```bash
sudo apt install openjdk-8-jre
```
### Em seguida vereficar a versão instalada.
```bash
java -version
```

--------------------------------------------------------------------
## Passo 2 — Criar o usuário do Tomcat
### Primeiro, crie um grupo tomcat:
```bash
sudo groupadd tomcat
```
### Em seguida, crie um usuário tomcat:
```bash
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```

--------------------------------------------------------------------
## Passo 3 — Instalar o Tomcat
### Mude para o diretório /tmp:
```bash
cd /tmp
```
### Use o curl para baixar o link que copiou do site do Tomcat:
```bash
curl -O https://downloads.apache.org/tomcat/tomcat-9/v9.0.41/bin/apache-tomcat-9.0.41.tar.gz

OU

curl -O https://downloads.apache.org/tomcat/tomcat-8/v8.5.61/bin/apache-tomcat-8.5.61.tar.gz
```
### Crie o diretório tomcat:
```bash
sudo mkdir /opt/tomcat
```
### Em seguida, extraia o arquivo nele com esses comandos:
```bash
sudo tar -vzxf apache-tomcat-*tar.gz -C /opt/tomcat --strip-components=1
```

--------------------------------------------------------------------
## Passo 4 — Atualizar permissões
### Vá até o diretório onde extraímos a instalação do Tomcat:
```bash
cd /opt/tomcat
```
### Dê a propriedade de todo o diretório de instalação ao grupo tomcat:
```bash
sudo chgrp -R tomcat /opt/tomcat
```
### Em seguida, dê ao grupo tomcat acesso de leitura ao diretório conf e todo o seu conteúdo e execute o acesso ao diretório, propriamente dito:
```bash
sudo chmod -R g+r conf
sudo chmod g+x conf
```
### Faça do usuário tomcat o proprietário dos diretórios webapps, work, temp e logs:
```bash
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

--------------------------------------------------------------------
## Passo 5 — Criar um arquivo de serviço systemd
### Queremos executar o Tomcat como um serviço. Dessa forma, vamos configurar o arquivo de serviço systemd.
```bash
vim /etc/systemd/system/tomcat.service
```
### Cole o conteúdo a seguir no seu arquivo de serviço. Modifique o valor de JAVA_HOME se necessário:
```python
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
### Após salvar e fechar, recarregue o daemon systemd para que ele saiba sobre nosso arquivo de serviço:
```bash
sudo systemctl daemon-reload
```
### Inicie o serviço Tomcat digitando:
```bash
sudo systemctl start tomcat
```
### Verifique novamente se ele foi iniciado sem erros, digitando:
```bash
sudo systemctl status tomcat
```
### Colocando o Tomcat para subir no boot
```bash
sudo systemctl enable tomcat
```
### Pausar o tomcat
```bash
sudo systemctl stop tomcat
```

--------------------------------------------------------------------
## Fonte
[DigitalOcean](https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804-pt)
