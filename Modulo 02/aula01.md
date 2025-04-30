# Aula 1: Introdução a Banco de Dados e SQLite (criação de tabelas)

Nesta aula, vamos entender o que é um banco de dados, para que ele serve e como podemos criar e manipular tabelas usando o SQLite — um sistema de banco de dados leve e embutido que funciona diretamente com arquivos.

---

## O que é um banco de dados?

Um **banco de dados** é uma forma organizada de armazenar informações para que elas possam ser facilmente acessadas, gerenciadas e atualizadas. Em vez de guardar dados soltos em arquivos de texto, usamos bancos de dados para trabalhar com grandes volumes de informação de forma estruturada e eficiente.

Exemplos de uso de banco de dados:
- Aplicativos que armazenam usuários e senhas.
- Sites de compras com listas de produtos, clientes e pedidos.
- Sistemas escolares com dados de alunos, turmas e notas.

---

## O que é o SQLite?

O **SQLite** é um sistema de banco de dados relacional leve e simples, ideal para pequenos projetos, protótipos e aplicações locais. Ele não precisa de um servidor instalado e funciona com um único arquivo `.db`.

Vantagens:
- Simples de usar com Python.
- Funciona em quase todos os sistemas operacionais.
- Armazena os dados em um arquivo local.

---

## Conectando ao SQLite no Python

Para começar a usar, usamos o módulo `sqlite3` (nativo do Python):

```python
import sqlite3

# Conecta ou cria um banco de dados chamado "meubanco.db"
conexao = sqlite3.connect("meubanco.db")
```

## Criando uma tabela

Para criar uma tabela no banco de dados, usamos a linguagem SQL:

```python
import sqlite3

conexao = sqlite3.connect("meubanco.db")
cursor = conexao.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT,
    idade INTEGER
)
""")

conexao.commit()
conexao.close()
```

Explicação:

- id: campo inteiro, chave primária, que aumenta automaticamente.

- nome: campo de texto.

- idade: campo numérico inteiro.

## Inserindo dados na tabela

```python
import sqlite3

conexao = sqlite3.connect("meubanco.db")
cursor = conexao.cursor()

cursor.execute("INSERT INTO usuarios (nome, idade) VALUES (?, ?)", ("Maria", 25))

conexao.commit()
conexao.close()
```

## Exercícios

Exercício 1:
Crie um banco de dados chamado escola.db e uma tabela chamada alunos com os campos:

- id (inteiro, chave primária, autoincremento)
- nome (texto)
- turma (texto)
- idade (inteiro)

Exercício 2:
Insira três alunos diferentes na tabela alunos.

Exercício 3:
Execute seu script várias vezes. O que acontece? Como evitar inserir os mesmos dados repetidamente?


Desafio Final
Crie um programa que:

- Crie um banco chamado cadastro.db

- Crie uma tabela contatos com os campos: id, nome, email e telefone

- Insira um contato

- Feche a conexão corretamente


## Resoluções

Exercício 1:

```python
import sqlite3

con = sqlite3.connect("escola.db")
cur = con.cursor()

cur.execute("""
CREATE TABLE IF NOT EXISTS alunos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT,
    turma TEXT,
    idade INTEGER
)
""")

con.commit()
con.close()

```

Exercício 2:

```python
import sqlite3

con = sqlite3.connect("escola.db")
cur = con.cursor()

alunos = [
    ("Ana", "5A", 10),
    ("Bruno", "5B", 11),
    ("Clara", "5A", 10)
]

for aluno in alunos:
    cur.execute("INSERT INTO alunos (nome, turma, idade) VALUES (?, ?, ?)", aluno)

con.commit()
con.close()

```

Exercício 3:

Observação:
Se você executar o código acima várias vezes, ele insere os mesmos alunos novamente. Para evitar isso, você pode:

- Verificar antes se o aluno já existe.

- Executar apenas uma vez ou apagar a tabela antes de rodar novamente (com DELETE FROM alunos).

```python
import sqlite3

con = sqlite3.connect("escola.db")
cur = con.cursor()

alunos = [
    ("Ana", "5A", 10),
    ("Bruno", "5B", 11),
    ("Clara", "5A", 10)
]

for nome, turma, idade in alunos:
    cur.execute("SELECT * FROM alunos WHERE nome = ?", (nome,))
    if cur.fetchone() is None:
        cur.execute("INSERT INTO alunos (nome, turma, idade) VALUES (?, ?, ?)", (nome, turma, idade))
    else:
        print(f"Aluno '{nome}' já está cadastrado.")

con.commit()
con.close()

```

Desafio:

```python
import sqlite3

con = sqlite3.connect("cadastro.db")
cur = con.cursor()

cur.execute("""
CREATE TABLE IF NOT EXISTS contatos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT,
    email TEXT,
    telefone TEXT
)
""")

cur.execute("INSERT INTO contatos (nome, email, telefone) VALUES (?, ?, ?)",
            ("João", "joao@email.com", "12345-6789"))

con.commit()
con.close()
```