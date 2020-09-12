# django
Criação de repositório para treinar curso em Django

[![Build Status](https://travis-ci.org/avellar1975/django.svg?branch=master)](https://travis-ci.org/avellar1975/django)
[![Updates](https://pyup.io/repos/github/avellar1975/django/shield.svg)](https://pyup.io/repos/github/avellar1975/django/)
[![Python 3](https://pyup.io/repos/github/avellar1975/django/python-3-shield.svg)](https://pyup.io/repos/github/avellar1975/django/)
[![codecov](https://codecov.io/gh/avellar1975/django/branch/master/graph/badge.svg)](https://codecov.io/gh/avellar1975/django)

https://python.radikaro.com/

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

pipenv install
```
<p>Para utilizar o comando pipenv no meu sistema operacional precisei criar um alias para não ter que ficar digitando <kbd>python3.8 -m pipenv ...</kbd>, a criação de um alias no Linux: `alias pipenv='python3.8 -m pipenv'`</p>
<p>Isso ai criar os arquivos Pipfile, Pipfile.lock e a pasta .venv</p>

## 3. Instalar a biblioteca django e flake8 via pipenv

```
pipenv install django

pipenv install -d flake8
```

<p>Para que o flake8 não faça a verificação dos arquivos do .venv é preciso
criar um arquivo .flake8 com o seguinte conteúdo:</p>

```
[flake8]
exclude=.venv
```

## 4. Ativar o ambiente virtual

```
pipenv shell
```
<p>Quando preciso instalar uma nova biblioteca no pipenv primeiro saio do ambiente virtual através do comando `exit`.</p>

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

<p>Para visualizar os comando do manage <kbd>python manage.py</kbd>, para executar
o servidor do Django <kbd>python manage.py runserver</kbd>, para parar o servidor
<kbd>CONTROL-C</kbd></p>

## 8. Publicação no heroku

* Instalar o cliente do heroku através do comando (no Ubuntu ou Debian):

<kbd>curl https://cli-assets.heroku.com/install.sh | sh</kbd>

* Editar a linha do arquivo `pypro/settings.py`
`ALLOWED_HOSTS = ['*']`

* Criar o arquivo Procfile na raiz do Projeto

* Instalar o gunicorn e executar o comando do heroku:
```
pipenv install gunicorn

heroku apps:create python-avellar-django
```
* Realizar os comandos do git: <kbd>git add .</kbd>, <kbd>git commit</kbd> e <kbd>it push heroku master:master -f</kbd>

Vai dar um erro que pode ser corrigido através do comando abaixo:

```
heroku config:set DISABLE_COLLECTSTATIC=1
```
* Repetir o comando do push
`git push heroku master:master -f`

* Abrir o site:
`heroku open`

## 9. Deploy automático

* Acessar a aplicação no heroku, aba Deploy

* Conectar o repositório do Github no heroku

* Marcar a opção Wait for CI to pass before deploy e clicar em Enable Automatic Deploys

## 10. Olá Django

* Estando no ambiente virtual, criar um app através do comando `python -m manage.py startapp base`

* Editar o arquivo views.py com o seguinte conteúdo:

```
from django.shortcuts import render
from djando.http import HttpResponse

# Create your views here.


def home(request):
    return HttpResponse('Olá Django')
```

* Executar o servidor localmente `python manage.py runserver`

* Inserir em settings.py, na lista de INSTALLED_APPS a linha: `pypro.base`

* Editar o arquivo urls.py para conter o código abaixo:

```
from django.contrib import admin
from django.urls import path
from pypro.base.views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home),
]

```
* Apagar a pasta migrations e demais arquivos da pasta base, mantendo somente o arquivo views.py

## 11. Pyteste django

* Instalar o plugin de teste `pipenv install 'pytest-django'`

* Criar o arquivo pytest.ini na raiz do projeto com o seguinte conteúdo:

```
# -- FILE: pytest.ini (or tox.ini)
[pytest]
DJANGO_SETTINGS_MODULE = pypro.settings
# -- recommended but optional:
python_files = tests.py test_*.py *_tests.py
```
* Criar pastas tests dentro do diretório base, com os arquivos __init__.py e test_home.py

* Executar o pytest dentro o ambiente virtual PIPENV_VENV_IN_PROJECT

* Alterar o .traves.yml incluindo no script a linha `- pipenv run pytest`

## 12. Cobertura de Testes

* Instalar as bibliotecas de cobertura pytest-cov e codecov através do comando `pipenv install --dev 'pytest-cov' codecov`

* Executar o comando `pipenv run pytest --cov=pypro`

* Adequar o script de teste do .travis.yml para contemplar a opção --cov

* Inserir no .travis.yml o código abaixo:

```
after_success:
  - pipenv run codecov
```

## 13. Lib Python Decouple

* Instalar a lib python-decouple através do comando `pipenv install python-decouple`

* Importar a biblioteca no arquivo settings.py `from decouple import config` e setar a variável `DEBUG = config('DEBUG', cast=bool)`

* Criar um arquivo `.env` com o conteúdo `DEBUG=True`, na raiz do Projeto

* Comando para passar a variável DEBUG para o heroku: `heroku config:set DEBUG=False`

* Criar na raiz uma pasta contrib e compiar o arquivo .env com o nome de env-sample

* Incluir na parte de install do .travis.yml a linha `  - cp contrib/env-sample .env`

## 14. Secret key

* No arquivo settings.py alterar o valor da constante `SECRET_KEY = config('SECRET_KEY')`

* Incluir no arquivo contrib/env-sample `SECRET_KEY='Defina aqui sua chave secreta'`

* Incluir no arquivo .env `SECRET_KEY='Chave secreta'`

* Gerar chave no console python através da função:
```
>>> from django.core.management.utils import get_random_secret_key
>>> get_random_secret_key()
```

Setando no Heroku:

`heroku config:set SECRET_KEY=<sua_chave_secreta>`

## 15. Domínio e ALLOWED_HOSTS

* `heroku domains:add SEUDOMINIO.com`

* Será gerado uma url para configurar no servidor de DNS, na Zona DNS do domínio utilizando o tipo CNAME

* Inserir no importe `from decouple import config, Csv`

* Configurar o settings.py, variável ALLOWED_HOSTS com o conteúdo `ALLOWED_HOSTS = config('ALLOWED_HOSTS', cast=Csv())`

* Atualizar o .env e contrib/env-sample com a linha `ALLOWED_HOSTS=localhost, 127.0.0.1`

* `heroku config:set ALLOWED_HOSTS='python.radikaro.com, python-avellar-django.herokuapp.com'`

## 16. Endereço de Banco de Dados

* Instalar a biblioteca `pipenv install dj-database-url`

* Importar a biblioteca no settings.py: `import dj_database_url`

* Importar a função partial, `from functools import partial`

* Alterar o arquivo settings.py

```
default_db_url = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')

parse_database = partial(dj_database_url.parse, conn_max_age=600)

DATABASES = {
    'default': config('DATABASE_URL',
                      default=default_db_url,
                      cast=parse_database)
}
```
<p>Importante dizer que se fizer chegar esse commit na master vai dar erro no heroku, pois depende dos passoas da próxima aula.

## 17. Testando Postgresql no Travis

* Instalar a biblioteca `pipenv install psycopg2-binary`

* Alterar o arquivo .travis.yml

```
services:
  postgresql
addons:
  postgresql: '9.5'


  before_script:
    - psql -c "CREATE DATABASE testdb;" -U postgres
```
* Alterar o arquivo env-sample inserindo a variável: `DATABASE_URL=postgres://postgres:postgres@localhost/testdb`

## 18. Lingua e Fuso Horário

* Alterar no settings.py:
```
LANGUAGE_CODE = 'pt-br'

TIME_ZONE = 'America/Sao_Paulo'
```
## 19. Comando de Coleta de Arquivos Estáticos

* Alterar o arquivo settings.py com o conteúdo abaixo:

```
# Configuração de ambiente de desenvolvimento

STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'mediafiles')
```
* Executar o comando `python manage.py collectstatic`

## 20. Configurar os usuários o S3 da ALLOWED_HOSTS

* Inserir no arquivo .env os valores de AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY e AWS_STORAGE_BUCKET_NAME=curso-python-django

* Executar as configurações de AIM e S3 na Amazon

* Instalar a biblioteca `pipenv install django_s3_folder_storage`

* Inserir informações de variáveis no settings.py:
```
# STORAGE AWS CONFIGS

AWS_ACCESS_KEY_ID = config('AWS_ACCESS_KEY_ID')

if AWS_ACCESS_KEY_ID:
    AWS_SECRET_ACCESS_KEY = config('AWS_SECRET_ACCESS_KEY')
    AWS_STORAGE_BUCKET_NAME = config('AWS_STORAGE_BUCKET_NAME')
    AWS_S3_OBJECT_PARAMETERS = {'CacheControl': 'max-age=86400', }
    AWS_PRELOAD_METADATA = True
    AWS_AUTO_CREATE_BUCKET = False
    AWS_QUERYSTRING_AUTH = True
    AWS_S3_CUSTOM_DOMAIN = None
    AWS_DEFAULT_ACL = 'private'

# static assets
    STATICFILES_STORAGE = 's3_folder_storage.s3.StaticStorage'
    STATIC_S3_PATH = 'static'
    STATIC_ROOT = f'/{STATIC_S3_PATH}/'
    STATIC_URL = f'//s3.amazonaws.com/{AWS_STORAGE_BUCKET_NAME}/{STATIC_S3_PATH}/'
    ADMIN_MEDIA_PREFIX = STATIC_URL + 'admin/'

# Upload Media Folder
    DEFAULT_FILE_STORAGE = 's3_folder_storage.s3.DefaultStorage'
    DEFAULT_S3_PATH = 'media'
    MEDIA_ROOT = f'/{DEFAULT_S3_PATH}/'
    MEDIA_URL = f'//s3.amazonaws.com/{AWS_STORAGE_BUCKET_NAME}/{DEFAULT_S3_PATH}/'

    INSTALLED_APPS.append('s3_folder_storage')
    INSTALLED_APPS.append('storages')


```

* Executar o comando `python manage.py collectstatic --no-input`

* Atualizar o arquivo env-sample:
```
# Configurações da AWS
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_STORAGE_BUCKET_NAME=
```
* Setar as 3 variáveis AWS no servidor do heroku: `heroku config: set VARIAVEL=VALOR`

* Dar o "unset" na variável DISABLE_COLLECTSTATIC através do comando `heroku config:unset DISABLE_COLLECTSTATIC`

* Instalar a biblioteca Collectfast com o comando `pipenv install Collectfast`

* Alterar a lista do settings:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'collectfast',
    'django.contrib.staticfiles',
    'pypro.base',
]

```

* No arquivo settings.py inserir a variável `COLLECTFAST_ENABLE = False` fora do if da AWS e `COLLECTFAST_ENABLE = True` dentro do if.
