# Ajax - Excluir registro (Delete)

A última etapa para concluirmos o CRUD completo com ajax é o Delete. Ele funciona de forma similar ao Update, sendo necessário um pk (chave primária) para indicar o registro que será deletado, contudo, iremos apagar o registro do banco de dados ao invés de salvá-lo. Sendo assim, ao invés de reutilizarmos aquela mesma função que usamos para o Create e o Update (save\_form), vamos criar uma única para o delete. Então, no arquivo views.py, vamos adicionar a função:

*def evento\_delete(request, pk):*  
*evento \= get\_object\_or\_404(Evento, pk \= pk)*  
*data \= dict()*

*if request.method \== 'POST':*  
*evento.delete()*  
*data\['form\_is\_valid'\] \= True*  
*eventos \= Evento.objects.all()*  
*data\['html\_list'\] \= render\_to\_string('listas/parcial\_list.html', {'object\_list': eventos})*  
*else:*  
*context \= {'evento': evento}*  
*data\['html\_form'\] \= render\_to\_string('listas/parcial\_delete.html', context, request \= request)*  
*return JsonResponse(data)*

Dentro do *if POST*, vamos usar o comando delete ao invés do save e, dentro do *else*, vamos trocar o template para parcial\_delete.html, o que vamos criar em breve. Primeiro, vamos passar pelo arquivo urls.py para poder importar essa função e adicionar seu *path*:

	*from .views import evento\_create, evento\_update, evento\_delete*  
	*…*  
		*path('js/excluir/\<int:pk\>', evento\_delete, name='js-excluir'),*

Na pasta templates/listas, vamos criar o arquivo parcial\_delete.html como cópia do arquivo parcial\_update.html. Vamos trocar os nomes Editar por Excluir e vamos editar a primeira linha para:

	*\<form method="post" action="{% url 'js-excluir' evento.pk %}" class="js-delete-form"\>*

Além disso, também vamos alterar o corpo do modal, já que para deletar o registro, não precisamos mostrar seus campos e respectivos valores.

\<div class="modal-body"\>  
\<p class="lead"\>Tem certeza que deseja excluir esse evento \<strong\>{{evento}}\</strong\>?\</p\>  
\</div\>

Com o template pronto, vamos adicionar na tabela o botão para deletar o registro. Em parcial\_list.html, vamos adicionar logo após o botão de Editar que declaramos anteriormente:

*\<button type="button" class="btn btn-danger btn-sm js-delete" data-url="{% url 'js-excluir' evento.pk %}"\>Excluir\</button\>*

Por fim, precisamos apenas adicionar as funções javascript. Por sorte, aqui no javascript nós poderemos reutilizar as funções já criadas. Com isso, teremos:

	*// Delete*  
	*$("\#table-evento").on("click", ".js-delete", loadForm);*  
	*$("\#modal-evento").on("submit", ".js-delete-form", saveForm);*

E terminamos o nosso CRUD completo utilizando o ajax. O ajax não precisa ser utilizado especificamente com o CRUD, ele pode ser usado para basicamente qualquer comunicação assíncrona com o banco de dados, enviando ou recebendo dados sem a necessidade de se recarregar a página por completo, poupando dados e rede e tornando não apenas a comunicação em si, mas o site como um todo, muito mais dinâmico.
