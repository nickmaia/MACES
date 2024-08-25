# Deploy na RENDER
## Web Service


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