---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 09: Autenticação e Autorização
author: Diego Cirilo

---
<style>
img {
  display: block;
  margin: 0 auto;
}
</style>

# <!-- fit --> Programação de Sistemas para Internet

### Prof. Diego Cirilo

**Aula 09**: Autenticação e Autorização

---
# Autenticação e Autorização
- É necessário limitar o acesso a operações nos sistemas web
- Usuários devem apresentar credenciais (*login* e senha) para *provar* sua identidade para o sistema - Autenticação
- Cada usuário tem um conjunto de operações permitidas - Autorização
- O Django já possui essas funcionalidades

---
# Django User Model
- O Django já possui um Model padrão para User
- Já conta com vários atributos:
    - `username`, `first_name`, `last_name`, `email`, `password`, `groups`, `user_permissions`, `is_staff`, `is_active`, `is_superuser`, `last_login` e `date_joined`
---
# Custom User
- Nem sempre o User do Django atende as nossas necessidades
- Há duas possibilidades:
    - Criar nossa própria classe *User*, herdando de *AbstractUser* ou *AbstractBaseUser*
    - Criar uma nova classe com os dados extras, deixando *User* apenas para autenticação
- Qual a melhor?

---
# Custom User
- *AbstractUser*: É basicamente o *User* do Django, porém como classe abstrata. 
- *AbstractBaseUser*: É classe base, sem a maioria dos atributos da classe *User*. É útil quando não temos interesse nesses atributos padrão.
- [Referência](https://docs.djangoproject.com/en/5.1/topics/auth/customizing/)

---
# Exemplos
- Classe *User* customizada (`models.py`)
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser): # ou AbstractBaseUser
  cpf = models.CharField(max_length=11, unique=True) # exemplo
```
- Devemos adicionar a configuração (`settings.py`)
```python
AUTH_USER_MODEL = "nomedoapp.User"
```

---
# Exemplos
- Classe de *Perfil*, que adiciona campos extras sem alterar *User*
- Exemplo (`models.py`)
```python
from django.db import models
from django.contrib.auth.models import User

class Perfil(models.Model):
  user = models.OneToOneField(User, on_delete=models.CASCADE)
  cpf = models.CharField(max_length=11, unique=True) # exemplo
```

---
# Exemplos
- No caso do *Perfil*, podemos acessar os dados extras com:
```python
usuario = User.objects.get(id=2) # pega o user com id 2
cpf_do_user = usuario.perfil.cpf
```

---
# Views padrão de autenticação

---
# Formulários de Cadastro/Login/Logout

---
# Recuperação de Senha

---
# Autorização
- Decorators

---
# Permissões

---
# Referências
- https://docs.djangoproject.com/en/5.1/topics/auth/

---
# <!--fit--> Dúvidas? 🤔
