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
# Autenticação
- É necessário limitar o acesso a operações nos sistemas web
- Usuários devem apresentar credenciais (*login* e senha) para *provar* sua identidade para o sistema - Autenticação
- Cada usuário tem um conjunto de operações permitidas - Autorização
- O Django já possui essas funcionalidades

---
# Django User Model
- O Django já possui um Model padrão para User
- Já conta com vários atributos:
    - `username`
    - `first_name`
    - `last_name`
    - `email`
    - `password`
    - `groups`
    - `user_permissions`
    - `is_staff`
    - `is_active`
    - `is_superuser`
    - `last_login`
    - `date_joined`

---
# Custom User
- Nem sempre o User do Django atende as nossas necessidades
- Há duas possibilidades:
    - Criar nossa própria classe User, herdando de AbstractUser
    - Criar uma nova classe com os dados extras, deixando User apenas para autenticação
- Qual a melhor?

---
# Referências
- https://docs.djangoproject.com/en/5.1/topics/auth/

---
# <!--fit--> Dúvidas? 🤔
