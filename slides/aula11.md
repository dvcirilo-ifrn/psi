---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 11: Mensagens/Paginação/Formsets
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

**Aula 11**: Mensagens/Paginação/Formsets

---
# Mensagens
- O Django possui uma ferramenta para notificações ou mensagens para o usuário;
- Essas mensagens servem de *feedback* sobre ações no sistema;
- Ex. erros nas ações, etc;
- Esse tipo de mensagem também é chamado de *toast*.

---
# Mensagens
- Existem 5 tipos de mensagem padrão:
    - `debug`, `info`, `success`, `warning` e `error`
- Devemos importar `from django.contrib import messages` nas *views*
- Para enviar uma notificação:
```py
messages.success(request, 'A ação foi realizada com sucesso!')

messages.error(request, 'Houve um erro na solicitação.')

```

---
# Mensagens nos templates
- Para que as mensagens sejam exibidas para os usuários, devemos adiciona-las ao template:
```django
{% if messages %}
    <div>
        {% for message in messages %}
            <div class="alert alert-{{ message.tags }}">
                {{ message }}
            </div>
        {% endfor %}
    </div>
{% endif %}
```
- O `message.tags` retorna o tipo de mensagem, permitindo criar o CSS específico para cada tipo de informação.

---
# Mensagens nos templates
- Existem bibliotecas próprias para esse tipo mensagem, além de funcionalidades do *Bootstrap*.
- Ex. Toastr

---
# <!--fit--> Dúvidas? 🤔
