# Aula 2 (05/05) – Operações básicas com SQLAlchemy (Create e Read)

Nesta aula, vamos aprender como inserir e consultar dados no banco de dados utilizando SQLAlchemy, uma poderosa ORM do Python. Utilizaremos o modelo `Aluno` criado na aula anterior.

---

## Relembrando o modelo `Aluno`

Antes de começarmos, certifique-se de ter a classe `Aluno` e o banco de dados configurados como fizemos na Aula 1.

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

engine = create_engine('sqlite:///banco.db')
Base = declarative_base()

class Aluno(Base):
    __tablename__ = 'alunos'
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)

Base.metadata.create_all(engine)

Session = sessionmaker(bind=engine)
session = Session()
```

---

## Inserindo dados com `session.add()` e `session.commit()`

Vamos adicionar um novo aluno ao banco de dados:

```python
novo_aluno = Aluno(nome="João", idade=20)
session.add(novo_aluno)
session.commit()
```

✅ **Explicação:**
- `session.add(objeto)`: adiciona o objeto à sessão.
- `session.commit()`: grava as alterações no banco de dados.

---

## Consultando dados com `session.query()`

### Listando todos os alunos:

```python
alunos = session.query(Aluno).all()
for aluno in alunos:
    print(f"ID: {aluno.id} | Nome: {aluno.nome} | Idade: {aluno.idade}")
```

### Usando filtros:

```python
joao = session.query(Aluno).filter(Aluno.nome == "João").first()
print(joao.nome, joao.idade)
```

---

## Dicas úteis

- `.all()` retorna uma lista de todos os resultados.
- `.first()` retorna o primeiro resultado encontrado.
- `.filter()` permite aplicar condições nas consultas.

---

## Exercício

1. Insira 3 alunos diferentes no banco de dados, com nome e idade variados.
2. Liste todos os alunos cadastrados.
3. Filtre e exiba apenas o aluno com o nome "Maria".

---

## Resolução dos Exercícios

### 1. Inserção de 3 alunos:

```python
aluno1 = Aluno(nome="Ana", idade=22)
aluno2 = Aluno(nome="Carlos", idade=18)
aluno3 = Aluno(nome="Maria", idade=19)

session.add_all([aluno1, aluno2, aluno3])
session.commit()
```

### 2. Listando todos os alunos:

```python
alunos = session.query(Aluno).all()
for aluno in alunos:
    print(f"{aluno.nome}, {aluno.idade}")
```

### 3. Filtrar "Maria":

```python
maria = session.query(Aluno).filter(Aluno.nome == "Maria").first()
print(maria.nome, maria.idade)
```

---

## Conclusão

Agora você sabe como inserir e consultar dados utilizando SQLAlchemy! Na próxima aula, aprenderemos como atualizar e excluir registros, montando um CRUD completo no terminal.
