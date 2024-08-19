**Post Create**

Nós já conseguimos solicitar informação do servidor e atualizar um pedaço da página sem a necessidade de atualizá-la por completo. Agora vamos enviar informação para o servidor sem atualizar a página. O primeiro passo será aprimorar a função *evento\_create* que criamos no arquivo views.py para lidar com os dois tipos de requisição (get e post).

*def evento\_create(request):*  
*data \= dict()*

*if request.method \== 'POST':*  
*form \= EventoForm(request.POST)*  
*if form.is\_valid():*  
*form.save()*  
*data\['form\_is\_valid'\] \= True*  
*else:*  
*data\['form\_is\_valid'\] \= False*  
*else:*  
*form \= EventoForm()*

*context \= {'form': form}*  
*data\['html\_form'\] \= render\_to\_string('listas/parcial\_create.html', context, request \= request)*  
*return JsonResponse(data)*

Começamos definindo um dicionário (dict). Um dicionário é bem similar a uma lista, com a diferença que os valores são acessados através de chaves (como a chave 'form\_is\_valid') ao invés de posições (\[0\], \[1\]...). Mais informações sobre dicionário [aqui](https://curso.grupysanca.com.br/pt/latest/dicionarios.html). A seguir, caso o method seja 'POST', vamos pegar o que foi preenchido pelo usuário e validar o formulário, definindo o valor da chave de acordo. Caso contrário, vamos pegar os campos do formulário para enviar para o cliente. As três últimas linhas são praticamente iguais a antes, a diferença é que agora estamos definindo 'html\_form' como uma chave do dicionário e passando o dicionário inteiro para o JsonResponse. Vamos no arquivo *parcial\_create.html* para alterar apenas a primeira linha:

	*\<form method="post" action="{% url 'js-criar' %}" class="js-create-form"\>*

Aqui definimos o nome que vamos usar no javascript (js-create-form) e adicionamos a url *'js-criar'*, lembrando que essa url está associada a essa função que acabamos de editar. Agora vamos voltar para o nosso arquivo evento.js. Nós vamos definir uma nova função, após a parte do *js-create*, mas ainda dentro do primeiro function.

*$(function(){*  
		*$(".js-create").click(function(){*  
			*$.ajax({*  
				*// Corpo da função get*  
				*// …*  
*});*  
*});*  
***// AQUI\!***  
*$("\#modal-evento").on("submit",".js-create-form", function(){*  
	*var form \= $(this);*  
	*$.ajax({*  
		*// Corpo da função post*  
*});*  
*return false*  
*});*  
*});*

Dessa vez, essa função vai ser chamada quando o elemento de id *modal-evento* receber o comando de enviar dados (submit) do formulário *js-create-form*. Logo em seguida, ainda antes do ajax, nós vamos pegar as informações do formulário (form) e ao fim do ajax nós vamos retornar *False*. Isso serve para indicar ao cliente que a página não deve ser atualizada. No corpo da função post, nós vamos colocar:

*url: form.attr("action"),*  
*data: form.serialize(),*  
*type: form.attr("method"),*  
*dataType: 'json',*  
*success: function(data){*  
*if(data.form\_is\_valid){*  
*alert("Evento foi criado");*  
*}else{*  
*$("\#modal-evento .modal-content").html(data.html\_form)*  
*}*  
            *}*

Começamos definindo alguns parâmetros do ajax e, logo após, definimos o que faremos ao obter a resposta do servidor (success). Lembrando que esse *data.form\_is\_valid* está vindo lá da função que criamos no arquivo views.py. No caso de *form\_is\_valid* ser *True*, eu sei que o registro foi criado no banco de dados e, no momento, vou apenas informar isso ao cliente. Mais a frente, vamos atualizar a lista nesse trecho. Caso contrário, eu sei que o formulário está inválido e o registro não foi criado. Nesse caso, a resposta do servidor vai ser referente aos erros encontrados durante a validação feita pelo próprio Django.

Com isso, terminamos o post e conseguimos criar um registro do tipo Evento, salvando no banco de dados, sem atualizar a página sequer uma vez. Teste no cliente para confirmar que está tudo ok. Após o alerta, será necessário atualizar a página para ver a lista atualizada com o evento criado, mas o evento está sendo criado e salvo sem essa necessidade. A seguir, vamos ver como atualizar essa lista de forma assíncrona.