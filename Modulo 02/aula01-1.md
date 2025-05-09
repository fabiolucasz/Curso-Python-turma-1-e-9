# Aula 1: Introdução a Banco de Dados com SQLAlchemy

## Objetivos da aula
- Compreender o que é um banco de dados e uma ORM
- Aprender a instalar e configurar o SQLAlchemy
- Criar um banco de dados SQLite usando Python
- Definir uma classe modelo (Aluno)
- Gerar tabelas automaticamente no banco
- Entender o que é e como criar um ambiente virtual
- Compreender a utilidade do arquivo requirements.txt
- Criar um pequeno exercício prático

---

## 📚 O que é um banco de dados?

Um **banco de dados** é um local onde armazenamos informações de forma organizada. É como um "arquivo digital" onde podemos guardar dados (como nomes de alunos, produtos, clientes) e depois acessar, atualizar ou remover essas informações.

---

## 🧠 O que é uma ORM?

ORM significa **Object-Relational Mapping**. Em vez de escrever comandos SQL diretamente no código, usamos **objetos Python** para representar as tabelas e os dados.

### Por que usar ORM?

- Evita escrever comandos SQL direto
- Integra melhor com código Python
- Facilita a manutenção do sistema
- Ajuda a organizar melhor os dados e suas relações

---

## 🧪 Ambiente Virtual em Python

Um **ambiente virtual** permite que você isole as bibliotecas e dependências de um projeto Python, evitando conflitos com outros projetos.

### 🔨 Como criar um ambiente virtual no Windows:

1. No terminal (CMD ou PowerShell), crie a pasta do seu projeto:
```shell
mkdir crud_sqlalchemy
cd crud_sqlalchemy
```

2. Crie o ambiente `virtual`:
```python
python -m venv venv
```
Obs: O windows restringe a execução de scripts. Abra o powershell como administrador e digite o seguinte comando:
```shell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
3. Ative o ambiente `virtual`:
```python
venv\Scripts\activate
```
Você verá algo como `(venv)` aparecendo no início da linha do terminal — isso indica que o ambiente virtual está ativo.

## 📦 requirements.txt

O arquivo `requirements.txt` armazena todas as bibliotecas instaladas em seu ambiente virtual. Isso facilita compartilhar o projeto com outras pessoas ou instalar as dependências rapidamente em outro computador.

### Como gerar o requirements.txt:

Com o ambiente virtual ativado, use:
```python
pip freeze > requirements.txt
```

### Como instalar as dependências a partir do arquivo:
```python
pip install -r requirements.txt
```



## 🔧 Instalando o SQLAlchemy

Para começar, instale o SQLAlchemy com o seguinte comando no terminal:

```
pip install sqlalchemy
```

---

## 🛠️ Estrutura básica do SQLAlchemy

Vamos entender como criar um banco SQLite e uma tabela utilizando o SQLAlchemy.

### 1. Importando bibliotecas e preparando a base

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker
```

### 2. Criando o banco de dados e a base das tabelas

```python
# Criação do banco de dados SQLite (arquivo: alunos.db)
engine = create_engine("sqlite:///alunos.db", echo=True)

# Base para criação das tabelas
Base = declarative_base()
```

### 3. Criando uma tabela (modelo) com classe Python

```python
class Aluno(Base):
    __tablename__ = "alunos"

    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)
```

Essa classe representa a **tabela "alunos"** com três colunas: `id`, `nome` e `idade`.

---

## 💾 Gerando o banco de dados e a tabela

Depois de definir o modelo, criamos a tabela no banco assim:

```python
Base.metadata.create_all(engine)
```

Esse comando verifica se a tabela já existe e a cria se necessário.

---

## 🔄 Criando uma sessão para interagir com o banco

Com a sessão, podemos inserir e consultar dados no banco:

```python
Session = sessionmaker(bind=engine)
session = Session()
```

---

## ✅ Código completo até aqui:

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

# Criando engine e base
engine = create_engine("sqlite:///alunos.db", echo=True)
Base = declarative_base()

# Definindo o modelo (tabela)
class Aluno(Base):
    __tablename__ = "alunos"
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)

# Criando tabela no banco
Base.metadata.create_all(engine)

# Criando sessão
Session = sessionmaker(bind=engine)
session = Session()
```

---

## 🧪 Exercício

Crie um programa Python com SQLAlchemy que:

1. Crie o banco de dados `escola.db`
2. Crie uma tabela `alunos` com os campos: `id`, `nome` e `idade`
3. Use a classe `Aluno` como modelo

> Dica: Você pode usar como base o exemplo acima e alterar o nome do banco de dados.

---

## 📘 Resolução do Exercício

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

# Criando engine e base
engine = create_engine("sqlite:///escola.db", echo=True)
Base = declarative_base()

# Definindo o modelo Aluno
class Aluno(Base):
    __tablename__ = "alunos"
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)

# Criando tabela no banco
Base.metadata.create_all(engine)

# Criando sessão
Session = sessionmaker(bind=engine)
session = Session()
```

---

## 📝 Conclusão

Nesta aula, aprendemos o que é uma ORM, por que usar o SQLAlchemy e como criar uma estrutura de banco de dados usando Python. Agora temos a base pronta para começar a inserir e consultar dados nas próximas aulas!

Na próxima aula, vamos aprender a **inserir e buscar registros** usando o SQLAlchemy.

---
