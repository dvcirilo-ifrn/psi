---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 15: AJAX
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

**Aula 15**: AJAX

---
# AJAX
- *Asynchronous JavaScript and XML*
- XML (*eXtensible Markup Language*)
- Permite a troca de informações com o servidor sem recarregar a página;
- Ao invés no *browser* fazer a requisição diretamente, o JS é o responsável;
- Pode carregar novas informações do servidor depois que a página já está carregada;
- Dá *dinamicidade* aos sites.

---
# AJAX
- Hoje se usa mais o JSON no lugar do XML;
- JSON (*JavaScript Object Notation*);
- Usa programação no front e no back-end.

---
# AJAX com *jQuery*
- O *jQuery* tem uma função `$.ajax`;
```js
$.ajax({
  url: "https://api.exemplo.com/dados", // URL do servidor
  type: "POST", // Método HTTP (GET, POST, PUT, DELETE)
  data: JSON.stringify({ nome: "João" }), // Dados enviados
  contentType: "application/json", // Tipo de conteúdo
  success: function(response) { // Quando a requisição for bem-sucedida
    console.log(response);
  },
  error: function(xhr, status, error) { // Em caso de erro
    console.error("Erro:", error);
  }
});

```

---
# AJAX com *jQuery*
- Também existem os *atalhos*:
```js
$.get("https://jsonplaceholder.typicode.com/posts/1", function(data) {
  console.log(data);
});

$.post("https://api.exemplo.com/novo", { nome: "Maria" }, function(response) {
  console.log("Usuário criado!");
});

```

---
# AJAX com *jQuery*
- As requisições são *assíncronas*;
- O código continua antes da resposta chegar;
- Devemos executar o que for necessário dentro da função `success`
- O JavaScript permite lidar melhor com essas operações assíncronas:
    - `async/await`
    - `.then()`
- Assunto da próxima disciplina.

---
# Atualizando o DOM com AJAX GET
- A resposta do request GET pode ser acessada na função *callback*;
- Se for um JSON, podemos tratar como um objeto JS comum (parece o dict do Python);
- Usamos as funções de manipulação do DOM para atualizar o conteúdo;

---
# Exemplo
```js
$(#meuBotao).click(() => {
  $.get("https://jsonplaceholder.typicode.com/posts/1", function(data) {
    $("#minhaDiv").append(
      `<div>
         <p>${data.title}</p>
         <p>${data.body}</p>
       </div>`
    );
  });
});
```

---
# AJAX no Django
- Para o Django o AJAX é uma requisição qualquer;
- A diferença é que ele vai receber/responder JSON e não HTML;
- Já existem bibliotecas para isso:
    - `from django.http import JsonResponse`
    - `import json`

---
# AJAX no Django
- Normalmente separamos as *views* do AJAX das comuns;
- Exemplo de consulta:
```python
def ajax_livro(request, id):
    livro = get_object_or_404(Livro, id=id)
    resultado = {
        "nome": livro.nome,
        "autor": livro.autor,
        "ano": livro.ano,
    }
    return JsonResponse(resultado)
```

---
# Serializer

---
# POST

---
# Referências
- https://api.jquery.com

---
# <!--fit--> Dúvidas? 🤔
