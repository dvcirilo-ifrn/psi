---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 13: Introdução ao JavaScript
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

**Aula 13**: Introdução ao JavaScript

---
# Introdução
- Usualmente as funcionalidades de um sistema web estão no servidor;
- O servidor recebe as requisições, processa/acessa dados e *monta* o HTML;
- As páginas HTML são enviadas *prontas* para o cliente (navegador);
- Depois de enviado ao cliente, o servidor não tem mais controle sobre a página;
- *Front-end* - interface gráfica do usuário;
- Como *desacoplar* a interface de usuário do servidor?

---
# JavaScript
- Linguagem *interpretada*, com tipagem dinâmica e multi-paradigma;
- Desenvolvida nos anos 90 para dinamizar páginas web;
- Permite alterar o conteúdo da página no lado do cliente;
- É executada por uma *engine* no navegador;
- Em meados dos anos 2000 surgiram os *runtimes* nativos, como o Node.js.

---
# Versões do JavaScript
- Padrão ECMAScript (*European Computer Manufacturers Association*);
- Iniciais:
    - ECMAScript 1 (1997): Primeira versão padronizada para compatibilidade entre navegadores;
    - ECMAScript 3 (1999): Introduziu tratamento de erros, expressões regulares e estabeleceu a base do JavaScript moderno.

---
# Versões do JavaScript
- ECMAScript 5 (2009):
    - Adicionou modo estrito, suporte a JSON e métodos de arrays (map, filter, reduce);
    - `"use strict";`
- ECMAScript 6 (ES6, 2015):
  - Let/Const (variáveis de escopo de bloco);
  - *Arrow functions*;
  - Classes e módulos;
  - *Promises* para operações assíncronas.

---
# Recursos Modernos do JavaScript
- ECMAScript 2016-2017 (ES7 & ES8):
    - ES7 (2016): Adicionou `Array.prototype.includes()` e o operador de exponenciação (`**`);
    - ES8 (2017): Introduziu async/await e métodos de objeto como `Object.entries()`;
- ECMAScript 2018-2023 (ES9 a ES13):
    - Operadores spread/rest para objetos;
    - Encadeamento opcional (`?.`) e operador de coalescência nula (`??`);
    - Top-level await para simplificar operações assíncronas.


---
# Runtimes do JS
- Node.js:
    - Ambiente de execução de JavaScript *server-side*;
    - Utiliza a *engine* V8 do Chrome;
    - Oferece APIs para acessar o sistema de arquivos, redes e outras funcionalidades do servidor;
- Browser Engines:
    - V8 (Chrome, Edge), SpiderMonkey (Firefox), JavaScriptCore (Safari);
    - Executam JavaScript diretamente nos navegadores, oferecendo suporte para aplicações web interativas.

---
# JavaScript em PSI
- Nessa disciplina utilizaremos o JS no *browser*;
- É possível usar o *console* do navegador diretamente;
- Também é possível embutir o JS em arquivos HTML (ou templates Django);
- Tag `<script></script>`;

---
# JavaScript em PSI
- A tag `<script>` pausa o carregamento do HTML para baixar e executar o JS;
- Normalmente os *scripts* JS são carregados no final do arquivo, antes do `</body>`:
    - Não atrapalha o carregamento do HTML;
    - Garante que DOM já foi toda carregada antes.

---
# JavaScript em PSI
- Podemos separar o código JS em arquivos estáticos `.js`;
- No HTML é carregado com:
    - `<script src="meuscript.js"></script>`;
- No Django usamos a *tag* `static`:
    - `<script src="{% static 'meuscript.js' %}"></script>`;

---
# JavaScript em PSI
- É possível importar scripts no `<head>`
- Para garantir que só sejam executados com o DOM carregado usamos o atributo `defer`:
    - `<script src="meuscript.js" defer></script>`
- A vantagem em relação a colocar a tag no final do `<body>` é que o *script* é baixado junto com a página.

---
# Sintaxe JS
- A sintaxe é parecida com C;
- Usa `;` para indicar o fim de uma diretiva;
- Usa `{}` para abrir e fechar diretivas, funções, etc;
- O *whitespace* não importa, ao contrário do Python;
- Comentários com `//` (linha) ou `/* etc */` (bloco).

---
# Sintaxe JS
- Comumente usamos:
    - camelCase para nome de funções e variáveis;
    - Um espaço antes do `{}`;
    - [*Style Guide*](https://javascript.info/coding-style)

---
# Declarações
- Variáveis podem ser declaradas com:
    - Automaticamente (não recomendado);
    - `var` - Escopo global com *hoisting*;
    - `let` - Variável com escopo de bloco;
    - `const` - Constantes, o valor/tipo não pode mudar.

---
# *Hoisting*
- Joga as declarações automaticamente para o topo do script;
- Permite usar variáveis/funções que ainda serão declaradas;
- Funciona com `var` e declaração de funções;
- Pode ser uma fonte de *bugs* se não for tratado com cuidado.

---
# Tipo de dados
- O JavaScript tem tipagem *dinâmica* e *fraca*;
- `var` e `let` podem receber tipos de dados diferentes;
- Tipos primitivos:
    - String, Number, Bigint, Boolean, Undefined, Null, Symbol;
- O resto é objeto (*Object*).

---
# Objetos JS
- JavaScript Object Notation (JSON):
```js
const bejeto = {
    nome: "Ana",
    idade: 20,
    profissao: "Desenvolvedora",
    saudacao: function() {
        return `Olá, meu nome é ${this.nome}.`;
    }
};
```

---
# Strings
- Podem ser delimitadas com:
    - ````
    - ""
    - ''
- O ```` é chamado de *string literal* e permite interpolação, múltiplas linhas, etc.
```js
const cor = "azul";
const informacao = `O display é ${cor}.`;
```

---
# Condicionais
- `if`:
```js
if (year < 2015) {
  alert( 'Too early...' );
} else if (year > 2015) {
  alert( 'Too late' );
} else {
  alert( 'Exactly!' );
}
```
- ternário:
```js
let resultado = (idade > 18) ? "acesso permitido" : "acesso negado";
```

---
# Condicionais
- `switch`
```js
switch (a) {
  case 1:
    alert( 'Primeiro!' );
    break;
  case 2:
    alert( 'Segundo!' );
    break;
  case 3:
    alert( 'Terceiro!' );
    break;
  default:
    alert( "Desclassificado!" );
}
```

---
# Laços
- `for`
```js
for (let i = 0; i < 3; i++) {
  alert(i);
}
```

- `while`
```js
while (i < 3) { // shows 0, then 1, then 2
  alert( i );
  i++;
}
```

- `do..while`
```js
do {
  alert( i );
  i++;
} while (i < 3);
```

---
# Funções no JS
- Funções padrão:
```js
function somar(a, b) {
    return a + b;
}
```

- Funções anônimas:
```js
const saudacao = function(nome) {
    return `Olá, ${nome}!`;
};
```

- *Arrow functions*
```js
const multiplicar = (a, b) => {
    const resultado = a * b;
    return resultado;
};
```

---
# *Arrow functions*
- Retornam o valor por padrão
```js
hello = () => "Hello World!";
```

- Se houver apenas um parâmetro
```js
hello = val => "Hello " + val;
```

---
# Arrays
- Funcionam como listas do Python;
- Coleção indexada de objetos;
- Ex.
```js
const meuArray = [];
meuArray = ["coisa1", {coisa2: "coisa 2"}, 5]; // não pode!
meuArray.push("coisa1");
meuArray.push({coisa2: "coisa 2"});
meuArray.push(5);
meuArray[6] = "coisa 6";
console.log(meuArray[0]);
```

---
# Arrays
- Percorrendo Arrays:
```js
meuArray.forEach( function (item) {
    console.log(item);
});
```

---
# Manipulação do DOM
- *Document Object Model*;
- Uma das principais funções do JS é manipular o DOM;
- Criar/remover elementos, substituir conteúdo, alterar atributos, etc;
- Isso permite interfaces de usuário dinâmicas.

---
# Manipulação do DOM
- Selecionar elementos:
    - `document.getElementById('id')`
    - `document.querySelector('.classe')`
    - `document.querySelectorAll('tag')`
- Modificar Conteúdo:
    - `element.textContent = 'Novo texto'`
    - `element.innerHTML = '<p>Novo HTML</p>'`

---
# Manipulação do DOM
- Alterar Estilos
    - `element.style.color = 'red'`
    - `element.classList.add('nova-classe')`
    - `element.classList.remove('classe-existente')`
- Criar e Inserir Elementos
    - `document.createElement('div')`
    - `parentElement.appendChild(novoElemento)`

---
# Manipulação do DOM
- Exemplo:
```js
const paragrafo = document.createElement('p');
paragrafo.textContent = 'Este é um novo parágrafo.';
document.body.appendChild(paragrafo);
```

---
# Eventos DOM
- Os eventos reagem a ações do usuário, servidor ou temporizadas
- Permitem a execução de funções quando algo acontece
- Ex. `click`, `mouseover`, etc;
- Usamos o `elemento.addEventListener('nome_do_evento', funcao_callback())`

---
# Eventos DOM
- Funções *callback* são executadas quando o *EventListener* detecta o evento;
- Ex.:
```js
elemento.addEventListener('click', function() {
    alert('Elemento clicado!');
});
```

---
# jQuery
- Biblioteca desenvolvida pra simplificar a manipulação da DOM, eventos, requisições, etc;
- Usa uma linguagem menos *verbosa* que o JavaScript puro (*Vanilla*);
- Já foi quase "obrigatória", hoje nem tanto;
- Mesmo assim, ainda é mais cômodo utilizá-la;
- [*You might not need jQuery*](https://youmightnotneedjquery.com/).

---
# jQuery
- Pode ser baixada e incluída no projeto localmente;
- Também pode ser importada online via CDN (*Content Delivery Network*);
- Versões:
    - `uncompressed`: código normal;
    - `minified`: código comprimido;
    - `slim`: versão com menos recursos;
- Normalmente utilizamos a versão `minified` ou `min`;
- `<script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>`

---
# Conceitos
- As funções *jQuery* iniciam com `$`;
- A sintaxe básica é:
    - `$("seletor").ação()`
- O `seletor` seleciona qual/quais elementos serão utilizados;
- A `ação()` executa alguma funcionalidade;

---
# `$(document).ready()`
- Normalmente os códigos *jQuery* ficam dentro de um:
```js
$(document).ready(function(){
    //...codigo...
})
```
- Essa função garante que a página já foi toda carregada.

---
# Seletores *jQuery*
- O seletor básico é `$()`:
    - `$("p")`
    - `$(".classe")`
    - `$("#id")`
- Equivalente aos `document.querySelector()`, `document.querySelectorAll()` e `document.getElementById()` do JS *Vanilla*;
- É possível compor seletores mais específicos

---
# Seletores *jQuery*
- Exemplos:
```js
$("*")	                // tudo
$(this)	                // o elemento atual
$("p.intro")	        // todos <p> com classe intro
$("p:first")	        // o primeiro <p>
$("ul li:first")	    // o primeiro <li> do primeiro <ul>
$("ul li:first-child")	// o primeiro <li> de todos <ul>
$("[href]")	            // todos elementos com atributo href
$("a[target='_blank']")	// todos <a> com target="_blank"	
$("a[target!='_blank']")// todos <a> com target diferente de "_blank"	
$(":button")	        // todos <button> e <input> com type="button"	
$("tr:even")	        // <tr> par
$("tr:odd")	            // <tr> impar
```

---
# Eventos *jQuery*
- `click`, `dblclick`, `hover`, `keypress`, `focus`, `change`, etc;
- Ex.:
```js
$("#meuBotao").click(function() {
  $(this).css("color", "red");
});
```

---
# Manipulação da DOM com *jQuery*
- CSS
```js
$("#id").css({
  "color": "blue",
  "background-color": "red"
});
```
- Class
```js
$("#id").addClass("btn btn-primary");
```

---
# Manipulação da DOM com *jQuery*
- Hide/Show:
```js
$("#id").hide();
$("#id").show();
```
- Get/Set:
```js
let conteudo = $(sel).text(); // get
$(sel).text("conteúdo de texto"); // set
let conteudo = $(sel).html(); //get
$(sel).html("conteudo <strong>HTML interno</strong>"); //set
let conteudo = $(sel).val(); //get
$(sel).val("valor de form"); //set
```

---
# Manipulação da DOM com *jQuery*
- Atributos:
```js
let atributo = $("#id").attr("href"); //get
$("#id").attr("href", "https://site.com"); //set
```

- Propriedades:
```js
$("#id").prop("checked", false);
```

---
# Manipulação da DOM com *jQuery*
- Adicionar elementos:
```js
$("#lista").append("<li>Item no final</li>");
$("#lista").prepend("<li>Item no início</li>");
$("#lista").after("<p>Texto depois da lista</p>");
$("#lista").before("<p>Texto antes da lista</p>");
```
- Remover:
```js
$("#id").remove();  // Remove o elemento completamente
$("#id").empty();   // Remove apenas o conteúdo interno
```

---
# Manipulação da DOM com *jQuery*
- Clone:
```js
let clone = $("#meuElemento").clone();
$("#outroElemento").append(clone);
```

- Substituir:
```js
$("#meuElemento").replaceWith("<p>Novo Parágrafo</p>");
```

---
# Manipulação da DOM com *jQuery*
- Criar elemento
```js
let botao = $("<button>", {
    text: "Clique Aqui",
    id: "meuBotao",
    class: "btn btn-primary"
});

$("#container").append(botao);

```

---
# Manipulação da DOM com *jQuery*
- Acessar elementos *pais*:
```js
$("span").parent();
$("span").parents();
$("span").parents("ul"); //filtro
$("span").parentsUntil("div");
```
- *Filhos*/*Irmãos*
```js
$("div").children();
$("div").siblings();
$("div").prev();
$("div").next();
$("div").find();

```

---
# Referências
- https://javascript.info/
- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript
- https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript
- https://jquery.com/
- https://www.w3schools.com/jquery/default.asp

---
# <!--fit--> Dúvidas? 🤔
