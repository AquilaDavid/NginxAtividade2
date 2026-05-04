# Introdução

Esta documentação trata-se do passo a passo de como instalar e configurar o  (**Nginx**) em um sistema Linux, utilizando o **WSL**, que será exemplificado na versão **Ubuntu 24.04**.  
Recomenda-se utilizar uma máquina container do Ubuntu 24.04 (**Docker**).

## Requisitos básicos

1. Ter Docker instalado  
2. Ter WSL instalado  
3. 8 GB de RAM
4. SystemCtl

## Primeiros passos

Para iniciar, devemos abrir terminal via WSL como administrador, com isso vamos dar os seguintes comandos.

`docker ps`

Com isso vamos ver se tem alguma maquina rodando, caso tenha alguma, pare ela e depois inicie para seguir os proximos passos.

Idealmente esteja assim:

![1](/1.png)


Caso você ainda não tenha o docker com um conteiner e maquina configurada: [Acesse aqui para configurar](https://github.com/AquilaDavid/GerenciaConfiguracao)

Como estamos como administrador no WSL, onde iniciarmos nossa maquina, já entraremos como usuario root.
Apos isso, vamos dar continuidade aos passos a passos.

### Instalando o Nginx

Vamos utilizar os seguintes comandos:

` apt update & apt install -y nginx`

Com isso, atualizamos todas as dependências do ubuntu e instalamos o nginx.

Para verificamos se o nginx foi corretamente instalado, usamos esse comando:

`nginx -v `

Deve aparecer algo assim:

![5](/5.png)

Com a isntalação pronta, vamos usar o **SystemCtl** para dar um  start no nosso nginx e configurar para iniciar junto com o SO.

Usaremos esses comandos para isso:

`systemctl status nginx`

Deve aparecer algo como:

![2](/2.png)

Com isso verificamos que o nosso nginx não estar ativo.
Para dar inicio ao nginx usaremos esse comando:

`systemctl start nginx`

Com isso o nosso nginx que estava como "Inativo" comece a ficar Ativo.

Para verificamos se estar ativo, usamos o mesmo comando:

`systemctl status nginx`.

Deve aparecer algo como:

![3](/3.png)


Com o nginx ativo, vamos agora para o segundo passo:

# Configurando o Nginx

## 1° passo (Instalar o Git)

User esse comando para instalar o git na sua maquina.

`apt update & apt install -y git`

Apos isso, vamos entrar no diretorio do ubuntu e fazer um clone de um repositorio com ReactJS.

Segue a sequencia de comandos necessarios para isso:

`cd /home/ubuntu`

Apos isso usaremos esse repositorio como exemplo: 

`git clone https://github.com/rhavymaia/minhaappweb20252.git`

Use esse comando apos a clonagem do repositorio:

`cd /home/ubuntu/minhaappweb20252`

## 2° passo (instalar o Node)

Usaremos o node para dar build na nossa aplicação.

Para isso, vamos usar esses comandos:

`apt update`

Isso server para atualizar todas as dependências do ubuntu.

Apos isso, vamos dar essa seguincia de comandos:

1. `apt install -y ca-certificates curl gnupg`
2. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash`
3. `source ~/.bashrc`
4. `nvm install 24`
5.`node -v`

Apos a instalação do node, vamos agora configura o Nginx.

## Configurando o nginx

Para o Nninx n ser barrado pelo UFW, é necessario dar um privilegio quando subir a maquina.

User o `exit para sair da maquina` e o `docker stop ubuntu2404_3`

Após isso use esse comando para subir o docker com privilegios:

`docker run --privileged -d --name ubuntu2404_3 ubuntu:24.04 bash -c "tail -f /dev/null"
` 

Com isso sua mquina subirar com privilegios.

Depois de subir a maquina, certificasse que o nginx e o node estão funcionado utilozando o systemctl como mostrado anteriormente.

Após isso, vamos ao diretorio onde estar nosso clone e vamos fazer os seguintes comandos:

Para instalar todas as dependencias do projeto.

`npm install`

Para dar um build na nossa aplicação

` npm run build`

Agora vamos para esse diretorio 

`cd /etc/nginx`

Use esse comando: `ln -s /etc/nginx/sites-available/minhaappweb20252.conf /etc/nginx/sites-enable/`

Apos isso, faça isso:

`/etc/nginx/sites-available# nano minhaappweb20252.conf`

Dentro do arquivo cole isso:

``` 
	server {
    listen 80;                          # Listen on port 80 >    server_name _; # Domain names to handle
    root /home/ubuntu/minhaappweb20252/dist;
    index index.html;

    location / {

        try_files $uri $uri/ =404;      # Handle 404s if fil>    }
	}	


```

Com isso fazemos com que o nginx escute na porta 80.

Mesmo configurado, como o docker não deu run com a porta 80, não tem como acessar via http pelo navegador, mesmo estando todo configurado.

vamos gerar uma clone do nosso docker e iniciarmos ele já startando a porta 80.

siga esses comandos abaixo:

```
	1. docker commit ubuntu2404_3 minha-imagem-react
	2. docker images
	3. docker run -d -p 8080:80 --name ubuntu2404_3_web minha-imagem-react
	4. http://localhost:8080
```

O 1° comando é responsavel por fazer a copia do docker junto com tudo que tem dentro dele.

O 2° comando é para verificar se o docker foi clonado com sucesso, deve aparecer algo como desse tipo `minha-imagem-react   latest`

O 3° comando é responsavel por iniciar o docker já com a porta 80.

O 4° é uma URL onde com isso você colocarar no navegador para ter acesso a sua aplicação ReactJs.

Com isso deve aparecer sua aplicação, como desse jeito:


![4](/4.png)
