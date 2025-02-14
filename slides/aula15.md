---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 15: RichText/Formsets
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

**Aula 15**: Formsets

---
# Formsets
- Conjuntos (sets) de forms;
- Permite exibir e salvar mais de uma cópia de um form em uma mesma página;
- Ex. adicionar várias tarefas de uma só vez;
- Faz muito sentido quando utilizado com JS/AJAX.

---
# Exemplo
- Considerando um model Tarefa e um ModelForm TarefaForm (feitos normalmente)
- No forms.py
```python
from django import forms
from django.forms import formset_factory
from .models import Tarefa

class TarefaForm(forms.ModelForm):
    class Meta:
        model = Tarefa
        fields = "__all__"


TarefaFormSet = formset_factory(TarefaForm, extra=2)  # 2 forms por padrão
```

---
# Exemplo
- No views.py:
```python
from django.shortcuts import render, redirect
from .forms import TarefaFormSet

def criar_tarefas(request):
    if request.method == 'POST':
        formset = TarefaFormSet(request.POST)
        if formset.is_valid():
            for form in formset:       # Fazemos o for pois são vários forms
                if form.cleaned_data:  
                    task = form.save()  
            return redirect('success_url')  
    else:
        formset = TarefaFormSet()  # Cria o formset vazio

    context = {
        'formset': formset
    }
    return render(request, 'criar_tarefas.html', context)
```

---
# Exemplo
- No template:
```django
...
<form method="post">
    {% csrf_token %}
    {{ formset.management_form }}  <!-- Necessário para formsets -->
    
    <!-- Carrega os forms um por um -->
    {% for form in formset %}
        <div class="form">
            {{ form }}
        </div>
        <hr>
    {% endfor %}
    
    <button type="submit">Save</button>
</form>
...
```

---
# Editar Formsets
- Caso seja necessário carregar dados no formulário, como em uma view de editar:
- No views.py
```python
from .models import Tarefa
from .forms import TarefaFormSet

def editar_tarefas(request):
    if request.method == 'POST':
        formset = TaskFormSet(request.POST)
        if formset.is_valid():
            for form in formset:
                if form.cleaned_data:
                    form.save()
            return redirect('success_url')
    else:
        tarefas = Tarefas.objects.all()
        initial_data = [{'title': task.title, 'description': task.description, 'completed': task.completed} for task in tasks]
        lista_tarefas = []
        for tarefa in tarefas:
            lista_tarefas.append(tarefa)
        formset = TarefasFormSet(initial=lista_tarefas)

    context = {
        'formset': formset,
    }

    return render(request, 'editar_tarefas.html', context)
```

---
# Formsets Inline
- Muitas vezes nossos formulários escrevem em mais de um model;
- Ex. a página de editar perfil de um usuário (models User e Perfil);
- Usamos formsets inline para criar esses formulários mais complexos;
- https://stackoverflow.com/questions/53035151/django-formset-factory-vs-modelformset-factory-vs-inlineformset-factory

---
# <!--fit--> Dúvidas? 🤔
