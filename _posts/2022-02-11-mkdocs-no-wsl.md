---
title: Material Design para MkDocs no WSL 
author:
  name: Pedro Cândido
  link: https://github.com/plocandido
date: 2022-02-11 14:55:00 -0300
categories: [Documentação, Tutorial]
tags: [documentação, WSL, Python]
---

# Instalando o MkDocs no WSL

Iremos mostrar nesse Post como podemos instalar o Material Desing para MkDocs no WSL para criação de documentação.

## Requisitos:

- [x] Windows Subsystem for Linux (WSL)
- [x] Visual Studio Code
- [x] Python 3.10
- [x] Pip/Virtualenv

## WSL

Primeiramente precisamos realizar a instalação do WSL no Windows seguindo [este tutorial oficial](https://docs.microsoft.com/pt-br/windows/wsl/install).

Para acessar a arvore dos arquivos do **WSL**, basta abrir o Explorer do Windows e digita na barra de endereço **\\\wsl$**. Senão aparecer nada, nenhuma pasta, você terá que abrir (executar) o Linux escolhido durante a instalação do WSL para só então tentar acessar os arquivos. Aqui a distribuição escolhida foi o Debian.

## Visual Studio Code

Estou utilizando o VSCode para realizar a programação dessa documentação. Para instalar o VSCode, bastar acessar [este link](https://code.visualstudio.com/) e fazer o download do binário e realizar a instalação. Depois disso teremos que realizar a instalação de uma extenção para o VSCode conforme [esse artigo](https://docs.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode).

## Python 3.10

### Instalação do Python.

Podemos utilizar outras versões do Python, mas nesse tutorial estou utilizando a versão 3.10. Para fazer isso, primeiramente precisamos atualizar o repositório do SO conforme abaixo.

```terminal
$ sudo apt update && sudo apt upgrade
```

Precisamos também instalar as dependências necessárias para construir o Python 3.10 a partir do código-fonte.

```terminal
$ sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```

Download do Python 3.10.

```terminal
$ wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
```

Descompactando o arquivo.

```terminal
$ tar -xf Python-3.10.*.tgz
```

Acessando a pasta descompactada.

```terminal
$ cd Python-3.10.*/
```

Executando o comando ``configure`` para verificar se as dependências necessárias estão disponíveis

```terminal
$ ./configure --enable-optimizations
```

Criando o Python 3.10 a partir do código-fonte conforme abaixo. Lembre-se de acelerar o processo usando o sinalizador `-j`. Isso especifica o número de núcleos em seu sistema. O comando `nproc` mostra os núcleos do seu sistema.

```terminal
$ make -j 4
```

Quando o ``make`` estiver concluído, prossiga e instale o Python 3.10 no linux conforme abaixo. O sinalizador `altinstall` é usado para manter o caminho binário padrão do Python em `/usr/bin/python`.

```terminal
$ sudo make altinstall
```

### Pip/Virtualenv

A instalação do `PIP` e feita utilizando o comando abaixo:

```terminal
$ sudo apt install python3-pip
```

Instalando o Virtualenv


```terminal
$ sudo pip3 install virtualenv
```

### Configuração do ambiente

Precisamos criar uma pasta para nosso projeto:

```terminal
$ mkdir docinfrati
```

Acessando a pasta criada.

```terminal
$ cd docinfrati
```

Vamos abrir o VSCode nessa pasta com o comando:

```terminal
$ code .
```

Vamos agora definir o Python padrão para o WSL.

Descobrindo onde o Python 3.10 está instalado

```terminal
$ which python3.10
```

Alterando o python padrão do WSL para o Python 3.10:

```terminal
$ sudo ln -sf /usr/local/bin/python3.10 /usr/bin/python
```

Estando dentro da pasta do projeto, precisamos criar o ambiente virtual:

```terminal
$ python -m venv .venv
```

Feito a criação do ambiente vamos ativa-lo:

```terminal
$ source .venv/bin/activate
```

Sempre quando terminar-mos de fazer as alterações é importante que seja desativado o ambiente utilizando com o comando abaixo:

```terminal
$ deactivate
```

Com o ambiente ativado, e com o VSCode aberto, vamos criar a pasta da aplicação chamada ``docinfra``. Dentro desta pasta vamos realizar a instalação dos pacotes utilizando o comando ``pip``, para isso teremos que criar um arquivo com os pacotes que serão utilizados. Copie o código abaixo e salve em um arquivo chamado ``requirements.txt``.

```terminal
astroid==2.9.3
click==8.0.4
future==0.18.2
ghp-import==2.0.2
importlib-metadata==4.11.1
isort==5.10.1
Jinja2==3.0.3
joblib==1.1.0
lazy-object-proxy==1.7.1
livereload==2.6.3
lunr==0.6.1
Markdown==3.3.6
MarkupSafe==2.1.0
mccabe==0.6.1
mergedeep==1.3.4
mkdocs==1.2.3
mkdocs-material==8.2.1
mkdocs-material-extensions==1.0.3
nltk==3.7
packaging==21.3
platformdirs==2.5.1
Pygments==2.11.2
pylint==2.12.2
pymdown-extensions==9.2
pyparsing==3.0.7
python-dateutil==2.8.2
PyYAML==6.0
pyyaml_env_tag==0.1
regex==2022.1.18
six==1.16.0
toml==0.10.2
tornado==6.1
tqdm==4.62.3
typed-ast==1.5.2
watchdog==2.1.6
wrapt==1.13.3
zipp==3.7.0
```
{: file='requirements.txt'}

Se deixar só o nome do pacote, o PIP irá instalar os mais recentes disponível. Para realizar a instalação use o comando abaixo:

```terminal
$ pip install -r requirements.txt
```
Ainda dentro da pasta da Aplicação vamos criar um arquivos ``mkdocs.yml`` com as configurações do projeto.

```terminal
site_name: INFRAESTRUTURA DE TI
site_url: http://127.0.0.1:8000/docinfra/
site_description: Documentation the IT Infrastructure 
site_author: Pedro Luiz de Oliveira Cândido
nav:
    - HOME: index.md
    - INFRAESTRUTURA:
      - Sede: se/networks.md
      - Rio Branco: rb/networks.md
      - Zona Norte: zn/networks.md
      - Caicó: cai/networks.md
      - Mossoró: mos/networks.md
    - SERVIDORES: 
      - Sede: se/server.md
      - Rio Branco: rb/server.md
      - Zona Norte: zn/server.md
      - Caicó: cai/server.md
      - Mossoró: mos/server.md
    - SUPORTE: se/support.md
    - SISTEMAS: se/system.md
    - BACKUPS: se/backups.md
    - STORAGES: se/storage.md
    - FIREWALL: se/firewall.md
    - POLITICAS: se/policy.md
    - SEGURANÇA: se/security.md
    - ANTIVIRUS: se/antivirus.md
    - MONITORAMENTO: se/monitoring.md
    - SCRIPTS: se/scripts.md
    - FERRAMENTAS: se/tools.md
    - SOFTWARE HOMOLOGADOS: se/approved-programs.md
    - OUTSOURCING: se/outsourcing.md   
    - BASE DE CONHECIMENTO: se/knowledge.md
theme:
    language: pt
    name: material
    logo: img/network_icon.svg
    favicon: img/network_icon.svg
    palette:
      primary: deep orange
      accent: teal
      scheme: default
    features:
    - navigation.indexes
    - search.highlight
    # - navigation.tabs    
    # - navigation.instant
    font:
      text: Roboto
      code: Roboto Mono
markdown_extensions:
  - pymdownx.details
  - pymdownx.emoji:
      emoji_index: !!python/name:materialx.emoji.twemoji
      emoji_generator: !!python/name:materialx.emoji.to_svg
  - pymdownx.inlinehilite
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences
  - pymdownx.keys
  - pymdownx.betterem
  - pymdownx.critic
  - pymdownx.caret
  - pymdownx.tilde
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tabbed
  - pymdownx.tilde
  - admonition
  - codehilite:
      guess_lang: false
  - toc:
      permalink: true
plugins:
  - search:
      lang:
        - en
        - pt
#   - macros
extra:
  disqus: ""
  social:
    - icon: fontawesome/brands/github-alt
      link: https://github.com/plocandido
    - icon: fontawesome/brands/twitter
      link: https://twitter.com/plo_candido
    - icon: fontawesome/brands/linkedin
      link: https://www.linkedin.com/in/plocandido/
    - icon: fontawesome/brands/facebook
      link: https://www.facebook.com/plo.candido
    - icon: fontawesome/solid/paper-plane
      link: mailto:plo.candido@outlook.com

```
{: file='mkdocs.yml'}


Além disse teremos que criar uma pasta ``docs`` que será onde iremos colocar os arquivos markdown, imagens e etc, da documentação. Para uma melhor organização criei algumas subpastas para cada unidade da instituição.

No arquivo ``mkdocs.yml`` no campo ``nav`` está listado todos os arquivos da documentação, esses arquivos precisam está criados no diretorio ``docs``, para isso vamos criar os arquivos com extenções ``.md``, aqui podemos criar quantos arquivos forem necessário, cada arquivo desse, será uma pagina da documentação com o conteúdo correspondente, o nome do arquivo não importa desde que estejam no diretorio ``docs`` e listados no campo ``nav``. Nesse mesmo diretório vamos criar uma outra pasta ``img`` para adicionar as imagens dos documentos, logo, favicon e etc.

Agora estando dentro da pasta Aplicação, vamos executar o MkDocs:

```terminal
$ mkdocs serve
```

Nossa estrutura da documentação ficou conforme imagem abaixo:

![result-mkdocs](../../assets/img/result-mkdocs.png)


Pronto, agora é só escrever e fazer as alterações da documentação utiliando markdown e ir salvando que o servidor do mkdocs irar executar e atualizar automaticamente no navegador. [Neste site](https://squidfunk.github.io/mkdocs-material/) tem varios exemplos de como podemos formatar os arquivos markdown. Depois de feita as alterações, vamos buildar o projeto para ser gerado os arquivos do site:

```terminal
$ mkdocs build
```

Com esse comando ele irá criar uma pasta chamada ``site``, é essa pasta que estará todo o projeto criado, quando fomos enviar os arquivos para o servidor da documentação, será os arquivos dessa pasta apenas.

Se caso preferir segue o [Link do repositorio](https://github.com/plocandido/docinfrati) desse projeto no GitHub.

**Fonte:** [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/pt-br/windows/wsl/install), [Visual Studio Code](https://code.visualstudio.com/), [Python 3.10](https://computingforgeeks.com/how-to-install-python-on-debian-linux/), [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

Fico a disposição para qualquer dúvida ou sugestão.

<script src="https://utteranc.es/client.js"
        repo="plocandido/plocandido.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>