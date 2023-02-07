## Iniciando projetos Python <img align="center" alt="Gui-Phyton" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" />

---

### **Iniciando um Back-end 🚀**

[Link para pesquisar as dependências](https://pypi.org/)

Primeiro vamos iniciar nosso ambiente virtual com o seguinte comando:

`python3 -m venv .venv`

`source .venv/bin/activate`

Criamos o arquivo `requirements.txt` para colocar apenas as dependências necessárias para a produção.

```
fastapi==0.88.0
sqlmodel==0.0.8
```

Depois criaremos um arquivo chamado `dev-requirements.txt`, onde informamos todas as dependências, com o seguinte comando:

```
-r requirements.txt   #todos que estão no requirements
flake8                #linter
black                 #formatador
uvicorn               #servidor
```

Após feito isso, executar:

`pip install dev-requirements.txt`

No arquivo `main.py` configurar o `FastAPI`:

```
from fastapi import FastAPI

app = FastAPI()

@app.get('/')
async def home():
    return {'details': 'Go to /docs for documentation'}
```

No arquivo `model.py` configurar o `SQLModel`

```
//exemplo
from typing import Optional
from sqlmodel import SQLModel

class SuaClasse(SQLModel, table=True):
    id: Option[int] = Field(default=None, primary_key=True)
    atributo: tipo = Field(max_length=100)
```

Depois, precisamos fazer a conexão. Para isso no arquivo `db.py`:

```
from sqlmodel import create_engine, SQLModel

db_name = "...nome do banco"
db_url = f"...url do banco"

connect_args = {"check_same_thread": False}

engine = create_engine(db_url, echo=True, connect_args=connect_args)

def create_db_and_tables():
    from . import model  # noqa: F401

    SQLModel.metadata.create_all(engine)
```

Após criar a comunicação com o banco de dados, no arquivo `main.py`, adicione:

```
...
from raiz.db import create_db_and_table, engine
from sqlmodel import Session

...

def get_session():    #Cria uma conexão falsa, para poder testar em um banco de dados diferente do que tem na produção
    return Session(engine)

@app.on_event("startup")
def on_startup():
    create_db_and_table()
...
```

### **Testes 🛠**

No arquivo `dev-requirements.txt` inserir a seguinte dependência:

```
...
pytest                #testes
```

Depois criar a pasta de testes `./tests`
