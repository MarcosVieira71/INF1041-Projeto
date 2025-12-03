---

# 📚 **Biblioteca Clean Architecture — Flask**

Este projeto implementa um **sistema de gerenciamento de biblioteca** utilizando **Python + Flask**, seguindo rigorosamente os princípios da **Clean Architecture**, **SOLID** e com **testes unitários via Pytest**.

O objetivo é demonstrar uma arquitetura desacoplada, sustentável e escalável, com casos de uso isolados da infraestrutura e repositórios que respeitam o princípio de inversão de dependência (DIP).

---

## 📌 **🎯 Objetivo do Projeto**

O sistema permite gerenciar uma biblioteca simples, incluindo:

* Cadastro de livros
* Edição de informações do livro
* Controle de disponibilidade
* Cadastro e edição de usuários
* Empréstimo e devolução de livros
* Remoção de usuários sem empréstimos ativos

A aplicação foi construída com foco em:

* Estrutura limpa e organizada baseada em **Clean Architecture**
* Aplicação explícita dos princípios **SOLID**
* Testes unitários isolando a lógica de negócio
* Uso de **Flask** como camada web
* Persistência via **SQLite**

---

# ✨ **User Stories implementadas no projeto**

Abaixo estão **as quatro histórias de usuário realmente implementadas** no sistema, com critérios de aceitação utilizados na lógica do código e nos testes.

---

## 🟦 **US01 — Alterar a disponibilidade de um livro**

**Como** bibliotecário
**Quero** editar a disponibilidade de um livro existente
**Para** manter o acervo atualizado corretamente

### ✔ Critérios de Aceitação (GWT)

* **Dado que** o livro não possui empréstimos ativos
  **Quando** altero sua disponibilidade
  **Então** a mudança deve ser salva com sucesso.

* **Dado que** o livro está emprestado
  **Quando** tento alterar sua disponibilidade
  **Então** o sistema deve impedir e mostrar:
  **“Não é permitido alterar a disponibilidade de um livro emprestado.”**

* **Dado que** o livro está emprestado
  **Quando** edito apenas título ou autor
  **Então** a alteração deve ser permitida.

* **Dado que** o ID informado não existe
  **Quando** tento editar
  **Então** o sistema exibe:
  **“Livro não encontrado.”**

---

## 🟩 **US02 — Realizar empréstimo de livro**

**Como** usuário da biblioteca
**Quero** pegar um livro emprestado
**Para** utilizá-lo temporariamente

### ✔ Critérios de Aceitação (GWT)

* **Dado que** o livro está disponível
  **E** o usuário existe
  **Quando** solicito o empréstimo
  **Então** o empréstimo é registrado e o livro fica indisponível.

* **Dado que** o livro já está emprestado a outra pessoa
  **Quando** tento pegar o mesmo livro
  **Então** recebo:
  **“Livro já está emprestado.”**

* **Dado que** o usuário já possui o mesmo livro emprestado
  **Quando** tenta pegar novamente
  **Então** deve aparecer:
  **“Este usuário já possui este livro emprestado.”**

* **Dado que** o livro não existe
  **Quando** o empréstimo é solicitado
  **Então** deve aparecer:
  **“Livro não encontrado.”**

* **Dado que** o usuário não existe
  **Quando** o empréstimo é solicitado
  **Então** deve aparecer:
  **“Usuário não encontrado.”**

---

## 🟧 **US03 — Devolver um livro**

**Como** usuário da biblioteca
**Quero** devolver um livro emprestado
**Para** regularizar minha situação e liberar o livro

### ✔ Critérios de Aceitação

* **Dado que** o usuário possui aquele empréstimo
  **Quando** devolve o livro
  **Então** o empréstimo é removido e a disponibilidade volta para *True*.

* **Dado que** o usuário não possui aquele livro emprestado
  **Quando** tenta devolver
  **Então** o sistema retorna:
  **“Este livro não está emprestado para este usuário.”**

* **Dado que** o livro não existe mais no acervo
  **Quando** a devolução é feita
  **Então** o sistema deve exibir:
  **“Livro não encontrado.”**

---

## 🟥 **US04 — Excluir usuário da biblioteca**

**Como** administrador
**Quero** excluir um usuário do sistema
**Para** manter o cadastro limpo e atualizado

### ✔ Critérios de Aceitação

* **Dado que** o usuário não possui empréstimos ativos
  **Quando** solicito a exclusão
  **Então** o usuário deve ser removido do sistema.

* **Dado que** o usuário possui empréstimos ativos
  **Quando** tento excluí-lo
  **Então** recebo:
  **“Não é permitido deletar um usuário com empréstimos ativos.”**

* **Dado que** o ID informado não existe
  **Quando** tento excluir
  **Então** deve aparecer:
  **“Usuário não encontrado.”**

---

# 🏛 **Arquitetura do Projeto (Clean Architecture)**

Estrutura oficial do projeto:

```
clean-library/
│
├── app/                     # Interface (Frameworks & Drivers)
│   ├── main.py              # Inicialização do Flask + DI
│   ├── routes/              # Rotas HTTP
│   ├── templates/           # Templates HTML (Jinja2)
│   └── static/              # CSS, JS, imagens
│
├── domain/                  # Regras essenciais do negócio
│   ├── book.py              # Entidade Book
│   └── user.py              # Entidade User
│
├── repositories/            # Interfaces abstratas (Ports)
│   ├── book_repository.py
│   ├── user_repository.py
│   └── loan_repository.py
│
├── use_cases/               # Application Business Rules
│   ├── book/
│   ├── user/
│   └── loan/
│
├── infra/                   # Implementações concretas (Adapters)
│   ├── db/
│   │   ├── database.py
│   │   └── init_db.py
│   └── repositories/
│
├── tests/
│   ├── domain/
│   └── use_cases/
│
├── requirements.txt
└── README.md
```

---

# 🚀 **Como rodar a aplicação**

## 1️⃣ Criar o ambiente virtual `.venv`

```
python -m venv .venv
```

Ativar:

**Windows:**

```
.venv\Scripts\activate
```

**Linux/macOS:**

```
source .venv/bin/activate
```

---

## 2️⃣ Instalar dependências

```
pip install -r requirements.txt
```

---

## 3️⃣ Inicializar o banco

```
python infra/db/init_db.py
```

---

## 4️⃣ Rodar o servidor Flask

```
python -m app.main
```

Acesse no navegador:

```
http://127.0.0.1:5000
```

---

# 🧪 Testes (pytest)

## Rodar todos os testes:

```
pytest
```

## Com detalhes:

```
pytest -vv
```

---

# 📘 Tecnologias Utilizadas

* Python 3.x
* Flask
* SQLite
* Jinja2
* Pytest
* SOLID + Clean Architecture

---
