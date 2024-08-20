# Paginação com busca
## Paginação

A DataTable consegue dividir os nossos dados em páginas e mostrar um conjunto de informações por vez, mas a página web irá baixar todos os registros de uma vez do banco de dados. Caso a quantidade de registros seja de centenas ou milhares, isso pode levar um tempo muito alto, o que não é o ideal. Portanto, pode ser necessário fazermos a paginação através do Django, de forma que apenas uma parcela dos registros seja transferida do banco de dados para a página web por vez.

Como exemplo, vamos acessar o arquivo `cadastros/views.py` e adicionar na classe `TurmaList`:

```py
class TurmaList(...  
    …  
    template_name = 'listas/turma.html'  
    paginate_by = 10
```

O número informado (10 no exemplo) é o número de registros que terá por página. Por incrível que pareça, a paginação já funciona. Podemos acessar o link da listagem de turmas e veremos apenas 10, caso haja mais do que isso. Para acessar as páginas seguintes, nós podemos digitar na barra de navegação:

http://127.0.0.1:8000/listar/turma/?page=1  
http://127.0.0.1:8000/listar/turma/?page=2  
http://127.0.0.1:8000/listar/turma/?page=3  

No entanto, apesar de já estar funcional, não queremos que os nossos usuários tenham que digitar manualmente o link de cada página, então iremos adicionar os links para eles. No template `cadastros/templates/listas/turma.html` vamos adicionar após a tabela:

```django
…  
</table>

<div class="text-center">  
    {% if page_obj.has_previous %}  
    <a href="?page={{page_obj.previous_page_number}}">Anterior</a>  
    {% endif %}

    <span>Página: {{page_obj.number}}</span>

    {% if page_obj.has_next %}  
    <a href="?page={{page_obj.next_page_number}}">Próxima</a>  
    {% endif %}  
</div>  
…
```

Agora nós informamos a página em que o usuário está atualmente (em `span`) e mostramos um link para a página anterior e outro para a página seguinte, caso haja.  

**Extra:** Também é possível adicionar links para a primeira e última página com:  

```django
<a href="?page=1">Primeira</a>  
…  
<a href="?page={{page_obj.paginator.num_pages}}">Última</a>
```

## Busca

Além da paginação, podemos adicionar um campo de busca para que o usuário possa filtrar os registros e encontrar o que procura mais facilmente. Para isso, vamos começar adicionando no html o seguinte formulário:

```django
<form action="?" method="GET">  
    <input type="text" name="nome" class="p-1 border" value="{{ request.GET.nome }}">  
    <button type="submit" class="btn btn-success">Buscar</button>  
    <a href="{% url 'listar-turmas' %}" class="btn btn-light">Limpar</a>  
</form>
```

O input será a caixa de texto onde o usuário irá colocar o que deseja procurar, o button será para enviar essa requisição ao servidor e o link no final (`a href`) é o link padrão da página e será usado para limpar o campo de texto e resetar a busca, deixando sem filtro.  
Ao clicar no botão, o texto será colocado após o link da página. Então, se buscarmos por "info", o link ficará:  

```django
link_da_pagina?nome=info  
```

Com `nome` sendo valor que colocamos no campo `name` em input. Ainda sobre o input, no campo `value`, o comando `{{ request.GET.nome }}` irá pegar o valor da informação `nome` do link da página (no exemplo acima, seria "info"). Caso contrário, o campo de busca ficaria vazio após recarregar a página.

Agora que o usuário já consegue escrever um termo para a busca e nós já conseguimos acessar essa informação, vamos criar o filtro de busca durante o acesso ao banco de dados. Para isso, vamos no arquivo `views.py` e, na classe onde ocorre a listagem, vamos adicionar:

```py
*…*  
def get_queryset(self):  
    nome = self.request.GET.get('nome')  
    if nome:  
        turmas = Turma.objects.filter(nome__icontains = nome)  
    else:  
        turmas = Turma.objects.all()  
    return turmas
```

Na primeira linha nós estamos buscando a informação no link da página, como fizemos no html anteriormente. Caso haja essa informação, vamos aplicar um filtro, caso contrário, vamos retornar todas as turmas. Com isso, a busca está pronta e funcional e já pode ser testada. Contudo, ainda há um pequeno inconveniente que precisamos corrigir: Ao mudar de página, a busca é perdida.

Para corrigir esse problema, precisamos passar adiante o termo procurado pelo usuário sempre que mudamos de página. Assim sendo, vamos retornar ao template e, em cada link que criamos durante a paginação, vamos adicionar:  

```django
<a href=?page={{...}}&nome={{request.GET.nome}}">...  
```

O que estamos fazendo aqui é definindo, para cada link, dois parâmetros: O `page` representa a página, o `nome` representa o termo de busca. E, para separá-los, usamos o símbolo `&`. Dessa forma, o termo será passado adiante e a paginação com a busca está pronta.

## Busca em dois ou mais campos

Para uma busca mais completa, é possível aplicar um filtro em múltiplos campos. O primeiro passo é decidir se será um único termo aplicado em múltiplos campos (como os resultados terem "info" no campo Nome e/ou no campo Descrição) ou se será múltiplos termos aplicados em múltiplos campos (ou seja, um campo de busca para cada campo). No caso de haver múltiplos campos, será necessário criar novas caixas de texto no template, seguindo o mesmo modelo de antes. Algo como:

```django
<form action="?" method="GET">  
    <input type="text" name="nome" class="p-1 border" value="{{ request.GET.nome }}">  
    <input type="text" name="descricao" class="p-1 border" value="{{ request.GET.descricao }}">  
    <button type="submit" class="btn btn-success">Buscar</button>  
    <a href="{% url 'listar-turmas' %}" class="btn btn-light">Limpar</a>  
</form>
```

Atente-se que o input foi adicionado no mesmo form de antes, pois é possível usar o mesmo botão de busca para ambos. A seguir, vamos retornar ao views, na classe da listagem, onde aplicamos o filtro. Vamos alterar para:

```py
def get_queryset(self):  
    nome = self.request.GET.get('nome')  
    descricao = self.request.GET.get('descricao')

    turmas = Turma.objects.all()

    if nome:  
        turmas = turmas.filter(nome__icontains = nome)  
    if estado:  
        turmas = turmas.filter(descricao__icontains = descricao)  
    return turmas
```

Normalmente, essa não seria uma boa prática de programação, pois em caso de haver dois termos, seria aplicado um acesso ao banco de dados, seguido de um filtro, seguido de um segundo filtro. Três operações distintas, cada qual com seu processo de processamento, resultando em algo lento e nada prático. No entanto, o Django possui algo chamado de *Lazy Query*, onde os filtros são acumulados e executados apenas uma única vez, resultando em apenas uma operação.  
Além disso, vale ressaltar que trocamos:  

```py
turmas = Turma.objects.filter(nome__icontains = nome)  
```

por:  

```py
turmas = turmas.filter(nome__icontains = nome)  
```

Isso porque o filtro é para ser aplicado no resultado do anterior. Assim, o Django vai entender isso e vai "somar" os filtros numa única operação com o *Lazy Query*.
