# Aula 1 : Introdução a banco de dados com SQLAlchemy

## Objetivos da aula
- Compreender o que é um banco de dados e uma ORM
- Aprender a instalar e configurar o SQLAlchemy
- Criar um banco de dados SQLite usando Python
- Definir uma classe modelo (Aluno)
- Gerar tabelas automaticamente no banco
- Criar um pequeno exercício prático

---

## 📚 O que é um banco de dados?

Um **banco de dados** é um local onde armazenamos informações de forma organizada. É como um "arquivo digital" onde podemos guardar dados (como nomes de alunos, produtos, clientes) e depois acessar, atualizar ou remover essas informações.

---

## 🧠 O que é uma ORM?

ORM significa **Object-Relational Mapping**. Em vez de escrever comandos SQL direto no seu código, usamos **objetos Python** para representar as tabelas e os dados.

### Por que usar ORM?

- Evita escrever comandos SQL direto
- Integra melhor com código Python
- Facilita a manutenção do sistema
- Ajuda a organizar melhor os dados e suas relações

---

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
