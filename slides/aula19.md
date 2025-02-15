---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 17: Customizando o Django Admin
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

**Aula 17**: Customizando o Django Admin

---
# Customizando o Django Admin
- O `admin.site.register` aceita mais um argumento além do model, o ModelAdmin;
- Nessa classe, podemos customizar o comportamento do Admin;
```python
from django.contrib import admin
from .models import Tarefa

class TarefaAdmin(admin.ModelAdmin):
    pass # não faz nada

admin.site.register(Tarefa, TarefaAdmin)

```
- No exemplo acima, o comportamento é o padrão.

---
# Customizando o Django Admin
- https://docs.djangoproject.com/en/5.1/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fields
- https://docs.djangoproject.com/en/5.1/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display

---
# Exemplo

---
# Fieldsets
- https://docs.djangoproject.com/en/5.1/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets

---
# Inlines
- https://docs.djangoproject.com/en/5.1/ref/contrib/admin/#inlinemodeladmin-objects

---
# <!--fit--> Dúvidas? 🤔
