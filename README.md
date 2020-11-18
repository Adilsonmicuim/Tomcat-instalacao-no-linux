# Como instalar o Apache Tomcat 9 no Ubuntu 18*

## Passo 1 — Instalar o Java
### Primeiramente, atualize seu índice de pacotes da ferramenta apt:
```bash
sudo apt update
```
### Então, instale o pacote Java Development Kit com a apt:
```bash
sudo apt install openjdk-8-jre-headless
```
### Em seguida vereficar a versão instalada.
```bash
java -version
```

## Passo 2 — Criar o usuário do Tomcat
### Primeiro, crie um grupo tomcat:
```bash
sudo groupadd tomcat
```
### Em seguida, crie um usuário tomcat:
```bash
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```

## Passo 3 — Instalar o Tomcat
### Mude para o diretório /tmp:
```bash
cd /tmp
```
### Use o curl para baixar o link que copiou do site do Tomcat:
```bash
curl -O https://downloads.apache.org/tomcat/tomcat-9/v9.0.39/bin/apache-tomcat-9.0.39.tar.gz
```
### Crie o diretório tomcat:
```bash
sudo mkdir /opt/tomcat
```
### Em seguida, extraia o arquivo nele com esses comandos:
```bash
sudo tar xzvf apache-tomcat-*tar.gz -C /opt/tomcat --strip-components=1
```

## Passo 4 — Atualizar permissões


## Fonte
[Digitalocean](https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804-pt)
