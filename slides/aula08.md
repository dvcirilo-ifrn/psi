---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 08: Forms
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

**Aula 08**: Forms

---
# Entrada de dados no sistema
- Como enviar dados para o sistema?
- Formulários - *Forms*

---
# HTML Forms
- Definidos pela tag `<form></form>`
- Os dados são preenchidos nos elementos `<input>`
- Os *inputs* podem ter vário tipos (`type`)
    - `<input type="button">`
    - `<input type="checkbox">`
    - `<input type="color">`
    - `<input type="date">`
    - `<input type="datetime-local">`
    - `<input type="email">`
    - `<input type="file">`
    - `<input type="hidden">`
    - `<input type="image">`
    - `<input type="month">`

---
# Inputs
- Cont.
    - `<input type="number">`
    - `<input type="password">`
    - `<input type="radio">`
    - `<input type="range">`
    - `<input type="reset">`
    - `<input type="search">`
    - `<input type="submit">`
    - `<input type="tel">`
    - `<input type="text">`
    - `<input type="time">`
    - `<input type="url">`
    - `<input type="week">`

---
# Labels
- Labels são as legendas ou nome do campo
- `<label for="id do campo">Conteúdo do label</label>`
- Ex.
```html
<form>
    <input type="text" id="nome" name="nome"> 
    <label for="nome">Nome: </label>
</form>
```

---
# Submit
- O `input` do tipo `submit` envia os dados do formulário
- `<input type=submit value="Enviar">`

# Name
- O atributo `name` é importante pois é como o dado vai ser referenciado no servidor.

---
# Method/Action
- O atributo `action` indica o caminho/url para onde o form será enviado.
- O `method` pode ser `POST` ou `GET`
- `GET`
    - Os dados são enviados na URL
    - Utilizado quando a ação não modifica dados no sistema
    - É útil pois permite salvar a URL com conteúdo
    - Exemplos: pesquisa, ordenação, filtros, etc.
- `POST`
    - Os dados são enviados na requisição HTTP
    - Deve ser utilizado sempre que a ação modifique dados no sistema

---
# Cross-site Request Forgery
- Falsificação de requisição entre sites
- Aplicações maliciosas podem fazer requisições de usuários "logados"
- Pode ser prevenido com um token (ficha) de segurança
- O Django já faz isso com a tag `{% csrf_token %}`
- É uma chave que o Django gera quando renderiza o form, e é enviado em um input hidden.
- Quando o Django recebe a requisição, ele compara se o token é o mesmo que ele gerou.

---
# Recebendo os dados de um form
- Quando o usuário clica no `submit` os dados do form são enviados para o servidor
- Se o `method` for `GET` os dados vão como uma *query string* na própria URL
- Ex. `http://localhost:8000?nome=Maria&idade=23`
- Se o `method` for `POST` os dados vão na requisição (`request`) HTTP
- Quando usar `GET` ou `POST`?

---
# Recebendo os dados de um form
- Caso o `action` não seja definido, o formulário é enviado para a mesma URL onde o formulário foi carregado.
- No Django, esses dados são acessados na `view`, através dos *dicionários* `request.POST` e `request.GET`
- Ex. `request.POST['nome']`
- É possível utilizar os dados diretamente assim, porém o Django conta com uma ferramenta mais completa.

---
# Django Forms
- O Django permite a descrição de forms diretamente no python
- As vantagens são a possibilidade de validação, redução da quantidade de HTML, segurança, integração com os Models, etc.
- Maior facilidade de manutenção do código também.

---
# Definindo um Form no Django
- Criamos um arquivo `forms.py` na pasta do `app`
- [Tipos de Dados](https://docs.djangoproject.com/pt-br/5.1/ref/forms/fields/#built-in-field-classes)
- Usam uma sintaxe parecida com a dos Models
- Ex.
```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100, label="Seu Nome")
    email = forms.EmailField(label="Seu Email")
    message = forms.CharField(widget=forms.Textarea, label="Mensagem")
```

---
# Widgets
- Os widgets são os componentes que renderizam os `inputs` do HTML
- Os fields já tem widgets padrão, mas é possível customizar
- [Tipos de Widgets](https://docs.djangoproject.com/pt-br/5.1/ref/forms/widgets/#built-in-widgets)

---
# Model Forms
- Normalmente os Forms usam dados muito parecidos com algum Model
- É possível criar um Form "automaticamente" com os dados de um Model
- Ex.
```python
from django import forms
from .models import MyModel

class MyModelForm(forms.ModelForm):
    class Meta:
        model = MyModel  # Modelo associado
        fields = ['field1', 'field2', 'field3']  # Campos incluídos no formulário
```

---
# Usando o Django Forms nas views
- O form pode ser instanciado vazio ou com dados
- Um form vazio é utilizado para um formulário de criação
- Um form com dados pode ser utilizado para receber/tratar/armazenar dados ou para realizar a alteração dos dados

```python
from .forms import MeuForm

def minha_view(request):
    
```

---
# Validação

---
# Mensagens de Erro

---
# Customização

---
# Django Crispy Forms


---
# <!--fit--> Dúvidas? 🤔
