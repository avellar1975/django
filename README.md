# django
Criação de repositório para treinar curso em Django

[![Build Status](https://travis-ci.org/avellar1975/django.svg?branch=master)](https://travis-ci.org/avellar1975/django)
[![Updates](https://pyup.io/repos/github/avellar1975/django/shield.svg)](https://pyup.io/repos/github/avellar1975/django/)
[![Python 3](https://pyup.io/repos/github/avellar1975/django/python-3-shield.svg)](https://pyup.io/repos/github/avellar1975/django/)

## 1.  Criar repositório com README, LICENCE e gitignore (python)

<p>Após criar o repositório público com os três arquivos, clonar o repositório
na máquina local.</p>

```
git clone git@github.com:avellar1975/django.git
```

## 2. Criar o pipenv dentro da pasta do projeto
<p>Inserir no arquivo ~/.bashrc a seguinte variável de ambiente:</p>

<kbd>export PIPENV_VENV_IN_PROJECT=1</kbd>

 <p>Isso vai garatir que o diretório .venv fique na pasta do projeto.</p>

```
cd django

python3.8 -m pipenv install
```

<p>Isso ai criar os arquivos Pipfile, Pipfile.lock e a pasta .venv</p>

## 3. Instalar a biblioteca django e flake8 via pipenv

```
python3.8 -m pipenv install django

python3.8 -m pipenv install -d flake8
```

<p>Para que o flake8 não faça a verificação dos arquivos do .venv é preciso
criar um arquivo .flake8 com o seguinte conteúdo:</p>

```
[flake8]
exclude=.venv
```

## 4. Ativar o ambiente virtual

```
python3.8 -m pipenv shell
```

## 5. Ativar integração com Travis-CI
<p>Ativar seu repositório no site TRAVIS-CI</p>
<p>Criar o arquivo .travis.yml</p>

## 6. Integrar com o Pyup
<p>Criar o arquivo .pyup.yml com o conteúdo:</p>

```
requirements:
  - Pipfile
  - Pipfile.lock
```
<p> Adicionar o repositório no pyup.io e quando subir o commit vai aparecer uma
issue para confirmar a integração com o Pyup.</p>

## 7.Setup de Projeto e Arquivo Manage

```
$ django-admin startproject pypro .
```
<p>Cria estrutura inicial do projeto com diretório pypro e arquivo manage.py</p>

<p>Para visualizar os comando o manage.py <kbd>python manage.py</kbd>, para executar
o servidor do Django <kbd>python manage.py runserver</kbd>, para parar o servidor
<kbd>CONTROL-C</kbd></p>

## 8. Publicação no heroku

Instalar o cliente do heroku
<kbd>curl https://cli-assets.heroku.com/install.sh | sh</kbd>

ALLOWED_HOSTS = ['*'] # Arquivo pypro/settings.py

Criar o arquivo Procfile na raiz do Projeto

```
python3.8 -m pipenv install gunicorn

heroku apps:create python-avellar-django
```
Realizar o git add., git commit e git push heroku master:master -f

heroku config:set DISABLE_COLLECTSTATIC=1

git push heroku master:master -f

heroku open
