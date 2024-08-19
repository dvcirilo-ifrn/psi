# Ajax - Editar registro (Update)

Com o Create e o Read implementados, o próximo passo será o Update. Boa parte do que é necessário para fazermos a edição de um registro é igual ao que fizemos para criar um novo registro no banco de dados. Sendo assim, para podermos reutilizar o código, vamos dividir as funções que criamos para o Create em duas: uma com o código que será reutilizado e outra com o código que será específico para o Create. Daí, basta criar uma terceira função, essa específica para o Update, para concluirmos esta etapa. Então, vamos começar no views.py. Primeiro de tudo, vamos fazer a seguinte importação, caso já não tenha no projeto:

	*from django.shortcuts import get\_object\_or\_404*

Em seguida, vamos dividir a função que criamos anteriormente (evento\_create) nas duas seguintes:

*def save\_form(request, form, template\_name):*  
*data \= dict()*

*if request.method \== 'POST':*  
*if form.is\_valid():*  
*form.save()*  
*data\['form\_is\_valid'\] \= True*  
*eventos \= Evento.objects.all()*  
*data\['html\_list'\] \= render\_to\_string('listas/parcial\_list.html', {'object\_list': eventos})*  
*else:*  
*data\['form\_is\_valid'\] \= False*

*context \= {'form': form}*  
*data\['html\_form'\] \= render\_to\_string(template\_name, context, request \= request)*  
*return JsonResponse(data)*

Dá para notar que esta função é praticamente igual a evento\_create que tínhamos antes, a diferença sendo que retiramos as linhas onde definimos o form e trocamos o endereço do template pela variável template\_name. E é isso que iremos fazer na função específica do Create:

*def evento\_create(request):*  
*if request.method \== 'POST':*  
*form \= EventoForm(request.POST)*  
*else:*  
*form \= EventoForm()*  
*return save\_form(request, form, 'listas/parcial\_create.html')*

Agora podemos criar a terceira função, que será específica do Update:

*def evento\_update(request, pk):*  
*evento \= get\_object\_or\_404(Evento, pk \= pk)*  
*if request.method \== 'POST':*  
*form \= EventoForm(request.POST, instance \= evento)*  
*else:*  
*form \= EventoForm(instance \= evento)*  
*return save\_form(request, form, 'listas/parcial\_update.html')*

Ela é bem similar à evento\_create, com a diferença sendo que precisamos identificar o registro que iremos editar. Para isso, vamos passar o pk (*primary key* ou chave primária) como argumento à esta função e, na primeira linha, ela irá buscar no banco de dados por este registro. Além disso, também iremos passar este registro (evento) como argumento para o formulário (instance). Por fim, definimos o template que será usado como base (parcial\_update), o qual iremos criar em breve. Mas antes, vamos importar esta nova função no arquivo urls.py e adicionar seu *path*.

	*from .views import evento\_create, evento\_update*  
	*…*  
		*path('js/editar/\<int:pk\>', evento\_update, name='js-editar'),*

O próximo passo é criar o arquivo parcial\_update.html na pasta templates/listas como uma cópia do arquivo parcial\_create.html. Vamos editar os nomes "Criar" por "Editar" e alterar apenas a primeira linha para:

	*\<form method="post" action="{% url 'js-editar' form.instance.pk %}" class="js-update-form"\>*

Trocamos a url e adicionamos nela o pk do registro com form.instance.pk, ou, a chave primária da instância do formulário, lembrando que passamos o registro como argumento *instance* lá no arquivo views.py. E também definimos o nome que usaremos no nosso arquivo javascript para chamar a função ajax (js-update-form) que irá salvar a edição no banco. Mas antes de irmos alterar o script, precisamos adicionar/alterar um botão, o qual irá trazer os dados do registro a ser editado para o formulário. Vamos no arquivo parcial\_list.html e vamos comentar (Ctrl \+ ;) ou remover os links que criamos para editar e remover os registros utilizando o CRUD convencional do Django. No lugar deles, vamos adicionar um botão para o nosso Update utilizando o Ajax.

*\<button type="button" class="btn btn-warning btn-sm js-update" data-url="{% url 'js-editar' evento.pk %}"\>Editar\</button\>*

Aqui definimos mais um nome que usaremos para chamar o ajax (js-update), sendo que este será para pegar os dados do banco e trazer para o formulário. Para o Create, deixamos o formulário vazio após o Get, pegando apenas os nomes dos campos e seus tipos. Para o Update, além dos campos, precisamos pegar também os valores salvos para cada um. Além disso, também definimos data-url. Esta variável será utilizada para identificar qual botão/operação está chamando o ajax, se é o do Create ou o do Update. Sendo assim, também precisamos definir o data-url no botão usado para o Create. Vamos no arquivo evento.html e vamos adicionar o data-url no botão Novo Evento que está logo após a tabela:

	*\<button type="button" class="btn btn-primary js-create" data-url="{% url 'js-criar' %}"\>Novo Evento\</button\>*

Finalmente, vamos editar o nosso arquivo evento.js e finalizar a operação Update. Assim como fizemos no arquivo views.py, vamos dividir as funções já existentes aqui em duas (código que será reutilizado e código específico do Create) para então criar as funções específicas do Update. Vamos começar pela função do 'js-create', trocando toda a primeira linha por:

	*var loadForm \= function(){*

Agora, ao fim desta função, terá um parênteses ) a mais , o qual deve ser removido. Ainda nessa função loadForm, na primeira linha, vamos adicionar:

	*var btn \= $(this);*

E vamos substituir o valor em url por:

	*url: btn.attr("data-url"),*

Agora, ao invés de definirmos uma url para cada operação, vamos pegar essa informação direto do html, utilizando aquela variável (data-url) que definimos a pouco. Agora vamos para a segunda função, trocando a primeira linha inteira por:

	*var saveForm \= function(){*

Lembre de retirar o parênteses ) no final. E essas serão as edições. Com exceção da pequena atualização que fizemos com o data-url, todo o código dessas duas funções é exatamente o mesmo para o Create e o Update. Agora, basta criar a parte específica para cada um, que será:

	*// Create*  
	*$(".js-create").click(loadForm);*  
	*$(".js-create-form").submit(saveForm);*

	*// Update*  
	*$("\#table-evento").on("click", ".js-update", loadForm);*  
	*$("\#modal-evento").on("submit", "js-update-form", saveForm);*

**Importante:** As funções podem ser definidas das duas formas acima e ambas estão corretas. Ou se faz *.click() / .submit()* ou se faz *.on("click") / .on("submit")*. Apenas atente ao fato de que, no primeiro caso, a classe que usamos para definir qual operação ajax será chamada está logo no início, dentro do *$()*, enquanto que no segundo caso nós definimos isso dentro do *.on()*.  
	A vantagem de se utilizar o segundo caso é que definimos, além da classe, de onde ela vem, ou seja, dentro de qual elemento ela se encontra. Como por exemplo: classe *js-update* dentro do elemento com id *table-evento*. Dessa forma, no caso de haver outra classe de mesmo nome (js-update) dentro de um elemento de id diferente (modal-usuario), não haveria conflito.

Com isso nós terminamos o Update utilizando o ajax e já podemos testar o nosso progresso. O próximo passo é realizar o Delete e então teremos um CRUD completo utilizando o ajax, inteiramente feito de forma assíncrona e sem a necessidade de atualizar a página por completo sequer uma vez.
