**Atualizar tabela (List)**

Até agora, nós fizemos o Create do CRUD utilizando o ajax, contudo precisamos atualizar a página para poder conferir a lista de registros atualizada. Vamos mudar isso. Primeiro, vamos criar um novo arquivo na pasta templates/listas chamado *parcial\_list.html*. Nesse template, nós vamos colocar a parte da página que será atualizada pelo ajax. Como nós queremos atualizar a lista com os registros, vamos copiar o corpo da tabela do arquivo evento.html para esse arquivo que acabamos de criar.

	*// Copiar de evento.html e colar em parcial\_list.html*  
	*{% for evento in object\_list %}*  
		*…*  
	*{% endfor %}*

No arquivo evento.html, onde antes estava o *%for*, nós vamos colocar a seguinte linha:

	*{% include 'listas/parcial\_list.html' %}*

Basicamente, estamos importando de volta o que acabamos de retirar. Fazemos isso porque, ao carregar a página, já queremos que a lista apareça com os registros. Anteriormente, quando criamos a janela modal e o parcial\_create, não precisamos usar o *include* pois ao carregar a página o modal fica "escondido", somente após o GET é que ele será mostrado com os campos do formulário. Vamos agora para o arquivo views.py, mais uma vez editar a função que criamos. Dentro do *if form.is\_valid()*, vamos adicionar no fim:

	*…*  
*if form.is\_valid():*  
	*…*  
	  
		*data\['form\_is\_valid'\] \= True*  
		*eventos \= Evento.objects.all()*  
*data\['html\_list'\] \= render\_to\_string('listas/parcial\_list.html', {'object\_list': eventos})*  
	*else:*  
		*…*

O que fizemos aqui foi adicionar uma nova chave ao dicionário (html\_list) e salvar nela uma lista com todos os registros do tipo Evento. Por fim, vamos ao nosso arquivo evento.js e vamos trocar o nosso alerta por:

	*// alert("Evento foi criado"); // Placeholder*  
	*$("\#table-evento tbody").html(data.html\_list);*  
	*$("\#modal-evento").modal("hide");*

Com essas duas linhas nós atualizamos o corpo da tabela com a lista atualizada dos registros e escondemos a janela modal após a criação do registro ter sido finalizada.