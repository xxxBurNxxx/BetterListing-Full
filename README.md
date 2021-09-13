BetterListing-Full
-------------

Olá, neste artigo irei descrever como configurar um indexador de arquivos simples para servidores usando o BetterListing de autoria do [devCoster](https://gitlab.com/devCoster/BetterListing/) e o nginx.

Segue abaixo alguns pontos importantes:
*	Não é difícil e nem complicado montar este serviço, porem tem certos pontos que atenção tem que ser dobrada parra não errar.
*	Não existe uma única maneira de fazer tudo o que está descrito aqui e provavelmente se você achou este artigo, você já deve ter visto algumas formas espalhadas pela internet.
*	Ao iniciar um tópico deste artigo, siga todos os passos até o final pois se faltar alguma etapa provavelmente lá na frente alguma coisa não vai funcionar.
*	Tudo que estiver marcado com *** tem que ter uma certa atenção.

Softwares necessários:
-------------
*	Distribuição Linux - Debian, Ubuntu, Raspbian e outras.
*	BetterListing - [Oficial](https://gitlab.com/devCoster/BetterListing/-/archive/master/BetterListing-master.zip) ou [BetterListing-Full](https://github.com/xxxBurNxxx/BetterListing-Full/raw/main/betterlisting.zip)
*	nginx

# Sumario

1. [Instalando e configurando o nginx](#1)
2. [Baixando o BetterListing-Full](#2)
3. [Testando o serviço](#3)

-----------------------------------------------------------------------------------------------------------------------------------

## 1. Instalando e configurando o nginx <a name="1"></a>


A instalação do nginx é bem simples, basta você executar o comando abaixo no terminal da sua distribuição Linux:

 	sudo apt-get install nginx -y

Execute o comando abaixo para verificar se o nginx esta instalado, o terminal deve te retornar a versão instalada.
	
		sudo nginx -v

Agora vamos alterar o arquivo de configuração do nginx para o nosso site, para isso iremos editar o arquivo de configuração do nginx com algum editor, no caso estarei utilizando o Nano.

	sudo nano /etc/nginx/sites-available/default

Com o arquivo aberto iremos alterar apenas algumas linhas.
Na linha "root ..........." iremos adicionar um # no inicio da linha para comentar ela e abaixo iremos colocar o diretório da pasta onde o site ira ficar, exemplo:

	#root /var/www/html;
	root /home/SEU_USUARIO/Documents/site-exemplo;

*** E por ultimo iremos colocar as configurações abaixo no bloco **location /**
Atenção a configuração deve ficar igual abaixo, caso tenha algum problema na hora de abrir o site pode ser este que esta diferente,.

        location / {
                add_before_body /betterlisting/top.html;
                add_after_body /betterlisting/bot.html;
                autoindex on;
                autoindex_localtime on;
                autoindex_exact_size off;
                #
                sub_filter '<html>' '';
                sub_filter '<head><title>Index of $uri</title></head>' '';
                sub_filter '<body bgcolor="white">' '';
                sub_filter '</body>' '';
                sub_filter '</html>' '';
                sub_filter_once on;
                #
                charset   utf-8;
        }
        
Feito todas as alterações no arquivo, você estiver usando o Nano pode pressionar **CTRL + o** para salvar, **Enter** para gravar no mesmo arquivo e **CTRL + x** para sair. Caso queira abra novamente este arquivo e verifique se salvou corretamente.

***Irei deixar no diretório desse arquivo um arquivo completo de exemplo.

[arquivo de exemplo](https://github.com/xxxBurNxxx/BetterListing-Full/blob/main/arquivo-default)

## 2. Baixando o BetterListing-Full <a name="2"></a>

Para configurar o BetterListing, você pode baixar os arquivos oficiais ou baixar os que eu deixei no diretório desse artigo [Clicando aqui](https://github.com/xxxBurNxxx/BetterListing-Full/raw/main/betterlisting.zip), extrair os arquivos e deixar a pasta **betterlisting** na raiz da pasta do seu site.

***Por padrão o BetterListing não exebe icones para mostrar a extensão do arquivo, o BetterListing-Full que eu disponibilizei ja esta com o pack de icones configurado. Caso você queira baixar baixar a versão oficial e configurar os icones manualmente, você pode baixar o icones [Clicando aqui](https://github.com/xxxBurNxxx/BetterListing-Full/raw/main/icons.zip) e colocar dentro da pasta **betterlisting**.

Basicamente a estrutura de pastas deve ficar conforme abaixo:

	site-exemplo
		     ┖-- betterlisting
		             ┖-- icons
		     ┖-- Documentos
		     ┖-- Fotos

## 3. Testando o serviço <a name="3"></a>

Para testar se tudo funcionou conforme primeiro precisamos descobrir qual o ip do computador e reiniciar o serviço do nginx para que ele pegue as configurações realizadas.

Para descobrir o ip do computador digite:
	
	ip address show
	      ou
	ifconfig

Depois digite os comandos abaixo para verificar o status no nginx, caso não retorne nenhum possivelmente esta tudo certo com as configurações.

	sudo nginx -t

Digite o comando abaixo para reiniciar o serviço do nginx:

	sudo systemctl restart nginx.service
	
E digite novamente o comando abaixo para verificar se tem algum erro:

	sudo nginx -t

Feito isso e caso não tenha aparecido nenhuma mensagem de erro, basta você abrir o seu navegador e digitar o ip do seu computador ou digitar localhost.
