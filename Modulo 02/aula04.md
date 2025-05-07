# Aula 4 — Conectando CRUD com Tkinter – Parte 1

Nesta aula, vamos começar a criar a interface gráfica do nosso CRUD usando a biblioteca Tkinter e integrá-la com o SQLAlchemy para cadastrar e listar registros do banco de dados.

---

## Revisão rápida do Tkinter

Tkinter é a biblioteca padrão do Python para construção de interfaces gráficas (GUI). Com ela, podemos criar janelas, botões, campos de entrada, listas e muito mais.

```bash
pip install tk
```

---

## Criando a interface gráfica

Vamos criar uma janela com campos para cadastrar alunos (nome, idade e turma) e um botão para salvar.

```python
import tkinter as tk
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

# Configuração do banco
engine = create_engine("sqlite:///alunos.db")
Base = declarative_base()
Session = sessionmaker(bind=engine)
session = Session()

# Modelo
class Aluno(Base):
    __tablename__ = "alunos"
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)
    turma = Column(String)

Base.metadata.create_all(engine)

# Função para salvar aluno
def salvar_aluno():
    nome = entry_nome.get()
    idade = entry_idade.get()
    turma = entry_turma.get()
    novo_aluno = Aluno(nome=nome, idade=int(idade), turma=turma)
    session.add(novo_aluno)
    session.commit()
    listar_alunos()

# Interface Tkinter
janela = tk.Tk()
janela.title("Cadastro de Alunos")

tk.Label(janela, text="Nome:").grid(row=0, column=0)
entry_nome = tk.Entry(janela)
entry_nome.grid(row=0, column=1)

tk.Label(janela, text="Idade:").grid(row=1, column=0)
entry_idade = tk.Entry(janela)
entry_idade.grid(row=1, column=1)

tk.Label(janela, text="Turma:").grid(row=2, column=0)
entry_turma = tk.Entry(janela)
entry_turma.grid(row=2, column=1)

btn_salvar = tk.Button(janela, text="Salvar", command=salvar_aluno)
btn_salvar.grid(row=3, column=0, columnspan=2, pady=10)

janela.mainloop()
```

---

## Exibindo dados cadastrados (Listbox)

Vamos incluir agora uma lista com os dados dos alunos cadastrados.

```python
# Adicione abaixo de btn_salvar:
listbox = tk.Listbox(janela, width=40)
listbox.grid(row=4, column=0, columnspan=2)

def listar_alunos():
    listbox.delete(0, tk.END)
    alunos = session.query(Aluno).all()
    for aluno in alunos:
        listbox.insert(tk.END, f"{aluno.nome} | {aluno.idade} anos | Turma: {aluno.turma}")
```

---

## Exercício

### Enunciado:
Crie uma interface que permita cadastrar alunos com nome, idade e turma e exibir os dados cadastrados logo abaixo dos campos de entrada.

---

## Resolução do Exercício

```python
import tkinter as tk
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

engine = create_engine("sqlite:///alunos.db")
Base = declarative_base()
Session = sessionmaker(bind=engine)
session = Session()

class Aluno(Base):
    __tablename__ = "alunos"
    id = Column(Integer, primary_key=True)
    nome = Column(String)
    idade = Column(Integer)
    turma = Column(String)

Base.metadata.create_all(engine)

def salvar_aluno():
    nome = entry_nome.get()
    idade = entry_idade.get()
    turma = entry_turma.get()
    if nome and idade and turma:
        novo = Aluno(nome=nome, idade=int(idade), turma=turma)
        session.add(novo)
        session.commit()
        listar_alunos()

def listar_alunos():
    listbox.delete(0, tk.END)
    for aluno in session.query(Aluno).all():
        listbox.insert(tk.END, f"{aluno.nome} | {aluno.idade} | {aluno.turma}")

janela = tk.Tk()
janela.title("Cadastro de Alunos")

tk.Label(janela, text="Nome:").grid(row=0, column=0)
entry_nome = tk.Entry(janela)
entry_nome.grid(row=0, column=1)

tk.Label(janela, text="Idade:").grid(row=1, column=0)
entry_idade = tk.Entry(janela)
entry_idade.grid(row=1, column=1)

tk.Label(janela, text="Turma:").grid(row=2, column=0)
entry_turma = tk.Entry(janela)
entry_turma.grid(row=2, column=1)

tk.Button(janela, text="Salvar", command=salvar_aluno).grid(row=3, column=0, columnspan=2)
listbox = tk.Listbox(janela, width=40)
listbox.grid(row=4, column=0, columnspan=2, pady=10)

janela.mainloop()
```

---

## Conclusão

Com esta aula, aprendemos a integrar o SQLAlchemy com o Tkinter para criar um sistema de cadastro e exibição de dados. Na próxima aula, vamos aprender a criar interfaces para **atualizar e excluir registros**, completando nosso CRUD gráfico.
