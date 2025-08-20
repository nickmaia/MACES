# Deploy na RENDER
## Web Service
Passso 01. No seu Código Instale as bibliotecas:

´
pip install whitenoise
pip install gunicorn
pip install psycopg2
´

depois de instalar as bibliotecas em seu teerminal de comando escreva o seguinte:

´pip freeze > requirements.txt´

Coloque em settings:
´
STATIC_URL = "static/"
STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
STATICFILES_DIRS = [BASE_DIR / "static"]  // Se tiver arquivos estaticos.
´

substitua ´ALLOWED_HOSTS = [*]´ por ALLOWED_HOSTS = ['127.0.0.1','localhost','name_project.onrender.com']

em urls do seu projeto principal ou seja esta na mesma pasta que settings coloque:

´
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = ([
    path("admin/", admin.site.urls),
    path("", include("home.urls")), // urls do seu app 
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) // para arquivos estaticos
+ static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) // para medias se tiver
)

´

Crie um arquivo nomeado como build.sh e adicione nele as seguintes informações:

´
set -o errexit

pip install -r requirements.txt

python manage.py collectstatic --no-input

python manage.py migrate 
´

obs: Se voce colocar o comando ´python manage.py collectstatic --no-input´ no seu build não pode fazer as colect no seu codigo principal se não falha na nuvem.

Passo 02. Na plataforma RENDER

Click em ´New´ logo em seguida selecione ´WEB SERVICE´ e conecte com seu github e selecione o repositorio do seu projeto, para criar substitua e crie:

name: nome do seu projeto
Build Command: sh build.sh
Start Command: gunicorn myproject.wsgi

## PostgreSQL

Passso 01. Na platarforma render:

Iniciar banco de dados em ´New´ selecione ´PostgreSQL´, Depois disso de um name para seu database e crie.



Passo 02. No seu codigo:

Instale as bibliotecas:

pip install dj-database-url


Depois em settings.py import as bibliotecas:
import dj_database_url
import os

Procure por DATABASES e substitua:

´if not DEBUG:
    DATABASES = {"default": dj_database_url.parse(os.environ.get("DATABASE_URL"))}

else:

    DATABASES = {
        "default": {
            "ENGINE": "django.db.backends.sqlite3",
            "NAME": BASE_DIR / "db.sqlite3",
        }
    }
´

Passo 03. Na plataforma render pegue a senha externa do database conhecida como ´External Database URL´ em seguida entre no seu web service nas variaveis de ambiente ´Environment Variables´ e adicione como key ´DATABASE_URL´  e value a senha externa do seu database e tambem adicione como key ´DEBUG´ e value ´False´, adicione sua como key ´SECRET_KEY´ e value ´sua senha secreta´
 

Passo 04. em seu codigo no settings mude DEBUG= True para DEBUG = os.environ.get("DEBUG", "True") == "True", e SECRET_KEY para SECRET_KEY = os.environ.get("SECRET_KEY", "sua senha secreta aqui")

logo depois atualize o requirements.txt:  pip freeze > requirements.txt