# Aula 3: Update e Delete com SQLAlchemy – CRUD no terminal

Nesta aula, vamos aprofundar o uso do SQLAlchemy para criar um CRUD completo no terminal, adicionando as funcionalidades de atualização e exclusão de registros.

---

## O que vamos aprender

- Atualização de registros (UPDATE)
- Remoção de registros (DELETE)
- Montagem de um menu de CRUD no terminal
- Exercício prático com nome, idade e turma

---

## Atualizando Registros com SQLAlchemy

Para atualizar um registro, primeiro buscamos o dado, depois modificamos os atributos e, por fim, usamos `session.commit()`.

```python
from sqlalchemy.orm import Session
from database import SessionLocal
from models import Aluno

session = SessionLocal()

# Buscar aluno
aluno = session.query(Aluno).filter(Aluno.nome == "Ana").first()

if aluno:
    aluno.idade = 18
    session.commit()
    print("Aluno atualizado com sucesso!")
else:
    print("Aluno não encontrado.")
```

## Deletando Registros com SQLAlchemy

A exclusão é feita usando `session.delete()` e `session.commit()`.

```python
# Buscar aluno
aluno = session.query(Aluno).filter(Aluno.nome == "Ana").first()

if aluno:
    session.delete(aluno)
    session.commit()
    print("Aluno removido com sucesso!")
else:
    print("Aluno não encontrado.")

```

## Criando um Menu CRUD no Terminal

```python
def menu():
    while True:
        print("\n1. Cadastrar Aluno")
        print("2. Listar Alunos")
        print("3. Atualizar Aluno")
        print("4. Remover Aluno")
        print("0. Sair")

        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            cadastrar_aluno()
        elif opcao == "2":
            listar_alunos()
        elif opcao == "3":
            atualizar_aluno()
        elif opcao == "4":
            remover_aluno()
        elif opcao == "0":
            break
        else:
            print("Opção inválida!")
```

## Exercício

Crie um sistema CRUD completo no terminal para gerenciar alunos com os seguintes dados:

- Nome (texto)
- Idade (inteiro)
- Turma (texto)

As operações devem ser acessadas através de um menu, como no exemplo acima.

## Resolução do Exercício 

### Estrutura de arquivos:

```css
projeto/
│
├── models.py
├── database.py
└── main.py

```

### models.py
```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class Aluno(Base):
    __tablename__ = "alunos"

    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)
    turma = Column(String)
```

### database.py
```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from models import Base

engine = create_engine("sqlite:///meubanco.db")
SessionLocal = sessionmaker(bind=engine)

Base.metadata.create_all(engine)

```

### main.py
```python
from database import SessionLocal
from models import Aluno

session = SessionLocal()

def cadastrar_aluno():
    nome = input("Nome: ")
    idade = int(input("Idade: "))
    turma = input("Turma: ")
    aluno = Aluno(nome=nome, idade=idade, turma=turma)
    session.add(aluno)
    session.commit()
    print("Aluno cadastrado!")

def listar_alunos():
    alunos = session.query(Aluno).all()
    for aluno in alunos:
        print(f"{aluno.id} - {aluno.nome}, {aluno.idade} anos, Turma: {aluno.turma}")

def atualizar_aluno():
    id = int(input("ID do aluno a atualizar: "))
    aluno = session.query(Aluno).get(id)
    if aluno:
        aluno.nome = input("Novo nome: ")
        aluno.idade = int(input("Nova idade: "))
        aluno.turma = input("Nova turma: ")
        session.commit()
        print("Aluno atualizado!")
    else:
        print("Aluno não encontrado.")

def remover_aluno():
    id = int(input("ID do aluno a remover: "))
    aluno = session.query(Aluno).get(id)
    if aluno:
        session.delete(aluno)
        session.commit()
        print("Aluno removido!")
    else:
        print("Aluno não encontrado.")

def menu():
    while True:
        print("\n1. Cadastrar Aluno")
        print("2. Listar Alunos")
        print("3. Atualizar Aluno")
        print("4. Remover Aluno")
        print("0. Sair")
        opcao = input("Opção: ")
        if opcao == "1":
            cadastrar_aluno()
        elif opcao == "2":
            listar_alunos()
        elif opcao == "3":
            atualizar_aluno()
        elif opcao == "4":
            remover_aluno()
        elif opcao == "0":
            break

menu()

```

