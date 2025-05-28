
# 📦 Instalação do WPP via Docker

## ✅ 1º Passo: Instalar o Docker no PDV

Acesse a documentação oficial:

🔗 [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

### ➡️ Procedimento:

1. Desça até a seção **"Set up Docker's apt repository"**.
2. Copie **todo o código** que está dentro do bloco.
3. No terminal do PDV, entre como **root**:

```bash
sudo su - 123456
```

4. Cole e execute o código no terminal:

```bash
# Atualiza o repositório e instala dependências
sudo apt-get update
sudo apt-get install ca-certificates curl

# Cria diretório de chaves
sudo install -m 0755 -d /etc/apt/keyrings

# Baixa a chave GPG oficial do Docker
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Adiciona o repositório do Docker
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Atualiza novamente o apt
sudo apt-get update
```

## ✅ 2º Passo: Instalar pacotes do Docker

Execute o comando:

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## ✅ 3º Passo: Reiniciar a máquina

```bash
sudo reboot
```

## ✅ 4º Passo: Testar o Docker

Após o reinício, execute o comando abaixo para verificar se o Docker está funcionando:

```bash
sudo docker run hello-world
```

### ✅ Retorno esperado:

```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
e6590344b1a5: Pull complete 
Digest: sha256:dd01f97f252193ae3210da231b1dca0cffab4aadb3566692d6730bf93f123a48
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

🚀 **Docker instalado com sucesso!**

## ✅ 5º Passo: Baixar a imagem da API do WPP

Execute o comando:

```bash
docker run --name wpp -d -p 21465 --restart unless-stopped tipetcamp/wpplj101:1.0
```

## ✅ 6º Passo: Criar a pasta do número de telefone e alterar o SELECT da consulta de mensanges para a loja

1. Acesse o terminal do container:

```bash
docker exec -it wpp /bin/bash
```

2. Instalar o VIM para inspecionar arquivos

```bash
apt install -y vim
```

3. Crie a pasta correspondente ao número do telefone da loja:

```bash
mkdir /tokens/celulardaloja
```

4. Alterar o select no server.ts

```bash
vi server.ts
```

Procure o SELECT que realiza a consulta no banco de dados e altere o WHERE remetente in ('')

Coloque o celular da loja na consulta.

Para salvar a alteração

:x! 

6. Saia do container:

```bash
exit
```

## ✅ 7º Passo: Reiniciar o container e visualizar o QRCode

Execute:

```bash
docker restart wpp && docker logs -f wpp
```

➡️ **O QRCode será gerado no terminal.**  
⚠️ **Atenção:** O QRCode expira a cada **30 segundos**.  
Se expirar, **reinicie novamente o container** para gerar um novo QRCode.

## ✅ Finalização

📲 Após gerar o QRCode, peça para a loja escaneá-lo com o WhatsApp.  
Pronto! O WPP está instalado e configurado!
