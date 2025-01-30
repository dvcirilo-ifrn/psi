---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 10: Autorização
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

**Aula 10**: Autorização

---
# Autorização
- A autorização define os recursos que podem ser acessados pelo usuário logado.
- Podemos limitar o acesso a views específicas apenas para usuários logados com o *decorator* `@login_required`
- É possível fazer verificação por meio de atributos específicos do model ou permissões do próprio Django.

---
# Permissões
- Regras de acesso a recursos.
- Para cada model, o Django cria automaticamente 4 permissões:
    - `add_nomedomodel`
    - `change_nomedomodel`
    - `delete_nomedomodel`
    - `view_nomedomodel`
- Também é possível criar outras permissões além das padrão.

---
# Grupos
- É possível controlar as permissões através de grupos ou diretamente para cada usuário.

---
# Referências
- https://docs.djangoproject.com/en/5.1/topics/auth/

---
# <!--fit--> Dúvidas? 🤔
