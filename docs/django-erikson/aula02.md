**Criação de um novo módulo**

Antes de começarmos com a parte de autenticação do sistema, vamos criar um novo módulo para manter o projeto organizado. Para isso, vamos pôr no terminal:  
	*python manage.py startapp usuarios*  
Será criada uma nova pasta com um conjunto de arquivos, assim como aconteceu quando criamos o módulo páginas e o módulo cadastros. Na pasta usuários, vamos criar:  
	Um arquivo chamado:		*urls.py*  
Uma pasta:			*templates*  
E dentro desta pasta:		*login.html*  
Você deve ter algo como isso:  
![][image1]  
Agora vamos no arquivo *settings.py* na pasta do projeto e vamos procurar por *INSTALLED\_APPS*. Dentro dos colchetes, vamos adicionar na última linha:  
	*'usuarios.apps.UsuariosConfig',*  
Por fim, ainda na pasta do projeto, vamos abrir o arquivo urls.py e nele adicionar na última linha de urlpatterns:  
	*path('', include('usuarios.urls')),*  
Com isso feito, temos nosso módulo criado e configurado e podemos finalmente começar a criar a parte de Autenticação.

**Login e Logout**

Vamos começar abrindo o arquivo *urls.py* da pasta *usuários*, que deve estar vazio no momento. Nele, vamos colocar:  
*from django.urls import path*  
*from django.contrib.auth import views as auth\_views*

*urlpatterns \= \[*  
    *path('login/', auth\_views.LoginView.as\_view(*  
        *template\_name='login.html'*  
    *), name='login'),*  
      
    *path('logout/', auth\_views.LogoutView.as\_view(),*  
        *name='logout'),*  
*\]*  
Dessa vez não precisamos criar as views, pois vamos usar uma do próprio django. Precisamos apenas definir o template que será utilizado, que será o arquivo *login.html* que criamos antes e que iremos definir a seguir. Então, em *usuarios/templates*, vamos abrir o arquivo *login.html* e colocar:  
	*{% extends 'modelo.html' %}*

*{% load static %}*

*{% block titulo %}*  
*\<title\>Autenticação\</title\>*  
*{% endblock %}*

*{% block conteudo %}*  
*\<div class="container"\>*  
    *\<h3\>Autenticação\</h3\>*

    *\<hr\>*

    *\<form action="" method="post"\>*  
        *{% csrf\_token %}*

        *{{ form.as\_p }}*

        *\<button type="submit" class="btn btn-primary"\>*  
            *Entrar*  
        *\</button\>*  
    *\</form\>*  
*\</div\>*  
*{% endblock %}*  
Esse código é uma cópia do arquivo *form.html*, alterando apenas o título e o texto do botão. Está quase pronto, mas se você acessar a página e logar, verá que dará erro, pois será encaminhado para um endereço inexistente. Esse endereço é um padrão do Django, mas temos como alterar isso. Para isso, vamos voltar no arquivo *settings.py* do projeto e vamos adicionar na última linha:  
	*LOGIN\_URL \= 'login'*  
*LOGIN\_REDIRECT\_URL \= 'inicio'*  
*LOGOUT\_REDIRECT\_URL \= 'login'*  
O que estamos fazendo aqui é informando que a página que será usada para o Login é a página *'login'*, ao efetuar um login o usuário será redirecionado (redirect) para a página *'início'* e ao efetuar um logout será redirecionado para a página *'login'*. Com isso, o login do usuário está pronto e funcional.

Vamos agora adicionar a opção de logout. Para isso, não precisamos criar uma página inteira, como fizemos no login. Vamos precisar apenas de um botão/link com a opção de logout e, quando o usuário clicar, será redirecionado de volta para a página de login (que definimos a pouco). Então, em *paginas/templates/modelo.html*, pois quero que essa opção esteja disponível em todas as páginas, vou adicionar ao menu do *navbar* (após o último *LI*, mas antes do *UL*):  
{% if request.user.is\_authenticated %}  
            \<li class="nav-item active"\>  
              \<a class="nav-link" href="{% url 'logout' %}"\>Logout\</a\>  
            \</li\>  
            {% else %}  
            \<li class="nav-item active"\>  
              \<a class="nav-link" href="{% url 'login' %}"\>Login\</a\>  
            \</li\>  
            {% endif %}  
Utilizamos o django para poder usar um IF e nele definimos dois links: Caso o usuário esteja autenticado, irá ver um link para logout. Caso contrário, irá ver um link para login.  
**Extra:** Caso queira informar o nome do usuário em algum lugar na página, você pode usar:  
	*{{ request.user }}*

**Autenticação**

No momento, o usuário consegue logar e deslogar no sistema, mas isso não implica em nada, pois mesmo deslogado o usuário consegue ver todas as páginas. Vamos mudar isso\! Em *cadastros/views.py*, vamos importar:  
	*from django.contrib.auth.mixins import LoginRequiredMixin*  
e **adicionar em cada view** que for necessário login para visualização (ex.: TurmaCreate):  
	*class TurmaCreate(**LoginRequiredMixin**, CreateView):*  
***login\_url \= reverse\_lazy('login')***  
*model \= …*  
Em negrito está o que foi adicionado à classe. Lembrando que é necessário realizar isso para todas as views necessárias. Agora, caso algum usuário não logado tente acessar essa página, ele será redirecionado à página de login.  
**Extra:** Caso você queira limitar uma página a um grupo de usuários (Ex.: professores, funcionários, administrador…), ao invés de utilizar o *LoginRequiredMixin*, você pode utilizar o *GroupRequiredMixin*. Para isso, primeiro se importa:  
	*from braces.views import GroupRequiredMixin*  
Lembrando que isso vem do django-braces, ou seja, caso já não tenha importado, importar pelo terminal usando:  
	pip install django-braces  
E adicionar em cada classe necessária:  
	*class AtividadeCreate(**GroupRequiredMixin**, CreateView):*  
***group\_required \= u"Professor"***  
***login\_url \= reverse\_lazy('login')***  
*model \= …*  
Os grupos podem ser criados pela página admin e, caso queira colocar mais de um grupo, basta usar uma lista como no campo fields. Ex.:  
		*group\_required \= \[u"Administrador", u"Professor"\]*

Caso queira, você pode adicionar uma mensagem para o usuário quando ele for redirecionado para a tela de login. Para isso, vamos trocar o que há no bloco conteúdo no arquivo *usuarios/login.html* por:  
	*{% block conteudo %}*  
*\<div class="container"\>*  
    *{% if request.user.is\_authenticated %}*  
    *\<h3\>Ação não permitida\!\</h3\>*  
    *\<hr\>*  
    *\<p class="lead"\>*  
        *\<b\>Houve algum problema para processar a ação desejada ou você não tem permissão para realizar a ação.\</b\>*  
    *\</p\>*  
    *{% else %}*  
    *\<h3\>Autenticação\</h3\>*

    *\<hr\>*

    *\<form action="" method="post"\>*  
        *{% csrf\_token %}*

        *{{ form.as\_p }}*

        *\<button type="submit" class="btn btn-primary"\>*  
            *Entrar*  
        *\</button\>*  
    *\</form\>*  
    *{% endif %}*  
*\</div\>*  
*{% endblock %}*  
Dessa forma, utilizamos a mesma página para dois propósitos diferentes.

Agora, apenas usuários cadastrados conseguem visualizar os dados sensíveis do site. Contudo, qualquer usuário consegue visualizar os dados de todos os usuários. Para alterar isso, precisamos criar uma relação entre os usuários e os registros criados por ele, de forma que depois possamos filtrar os registros para que o usuário possa apenas ver/editar/excluir os registros que ele próprio criou. 

Vamos começar criando a relação usuário-registro. Em *cadastros/models.py*, vamos importar:  
*from django.contrib.auth.models import User*  
Para cada classe que for necessário, adicionar como atributo:  
*usuario \= models.ForeignKey(User, on\_delete=models.PROTECT)*  
Como as classes foram alteradas, precisamos atualizar o banco de dados. Para isso, vamos digitar no terminal:  
*python manage.py makemigrations*  
Caso já haja algum cadastro no banco de dados, será necessário definir um padrão "default" para os registros já cadastrados. Para isso, vamos digitar 1 (primeira opção) e vamos definir como padrão 1 (ID do primeiro usuário criado). Em seguida, vamos por no terminal:  
*python manage.py migrate* 

Agora precisamos configurar como o usuário será associado ao registro automaticamente. Vamos em *cadastros/views.py* e, para cada classe **Create** que for necessária, vamos adicionar:  
*class…*   
*…*  
*success\_url…* 

*def form\_valid(self, form):*  
*form.instance.usuario \= self.request.user*  
*url \= super().form\_valid(form)*  
*return url*  
O Django chama essa função *form\_valid* automaticamente, executando as duas últimas linhas por padrão. O que fizemos foi definir *(def)* uma outra versão dessa função, adicionando a primeira linha, onde o ID do usuário logado será adicionado no campo *usuario* do registro.  
**Extra:** Caso queira, é possível modificar os dados recebidos pelo usuário após a validação (linha url \= ...). Vamos, por exemplo, adicionar o texto "\[Teste\]" no fim da descrição. Para isso, colocaríamos antes do *return*:  
*self.object.descricao \+= "\[Teste\]"*   
*self.object.save()*  
*return url*

Agora precisamos filtrar os registros na listagem, para que o usuário veja apenas os registros que ele criou. Para isso, vamos modificar as classes **List**, adicionando no fim das classes:  
*class…*   
*…*  
*template\_name…* 

*def get\_queryset(self):*  
*self.object\_list \= Turma.objects.filter(usuario \= self.request.user)*  
*return self.object\_list*  
Assim como o form\_valid, o Django chama automaticamente o *get\_queryset*, onde o padrão seria *…objects.all* . Ao invés disso, filtramos (filter) os registros pelo ID do usuário logado. 

Agora resta configurar as classes **Update** e **Delete** para que apenas o usuário que criou o registro possa editar/excluí-lo. Para isso, vamos adicionar no fim de cada classe **Update e Delete**:  
*class…*   
*…*  
*success\_url…* 

*def get\_object(self, query=None):*  
*self.object \= Turma.objects.get(pk \= self.kwargs\['pk'\], usuario \= self.request.user)*  
*return self.object*  
Usamos o *kwargs\['pk'\]* para pegar o ID do registro do endereço e pegamos o objeto (get) apenas se ele tiver este ID e se o usuário que o criou for o mesmo que está logado no sistema.

**Extra:** No momento, caso o usuário tente editar ou excluir um registro que ele não criou, ele receberá um erro de permissão. Dessa forma, ele vai saber que existe um registro com aquele ID específico. Para ter  uma solução mais elaborada, é possível alterar o erro recebido para um 404 (página não encontrada). Para isso, ainda em *cadastros/views.py*, vamos importar:  
*from django.shortcuts import get\_object\_or\_404*  
E vamos editar a função *get\_object* das classes **Update** e **Delete** para:  
*def get\_object(self, query=None):*  
*self.object \= get\_object\_or\_404(Turma, pk \= self.kwargs\['pk'\], usuario \= self.request.user)*  
*return self.object*

**Enviar dados para Template**

É possível enviar informação das nossas views para os nossos templates (HTML), como o título das páginas, por exemplo. Para isso, primeiro precisamos definir essas informações nas nossas views (ex.: cadastros/views.py, classe TurmaCreate) .  
*class…*   
*…*  
*success\_url…* 

*def get\_context\_data(self, \*args, \*\*kwargs):*  
*context \= super().get\_context\_data(\*args, \*\*kwargs)*  
*context\['Titulo'\] \= "Cadastro de Turmas"*   
*return context*  
Daí, no template html (ex.: form.html), podemos pegar essa informação com as chaves duplas:  
    \<h3\>{{ Titulo }}\</h3\>  
**Atenção:** O único "problema" de enviar os dados para o template é que você deve configurar o *context* para todas as views que usam aquele template, caso contrário não irá aparecer nada ao usar as chaves duplas.

**Cadastro de Usuário**

O usuário já está funcional, com login, logout e relação entre ele e os seus registros. Falta agora disponibilizar ao usuário uma tela de cadastros, já que no momento só é possível criar um novo usuário pela página do admin. Para isso, vamos em *usuarios/views.py* e vamos digitar:  
*from django.views.generic.edit import CreateView*  
*from django.contrib.auth.models import User*  
*from django.urls import reverse\_lazy*

*class UsuarioCreate(CreateView):*  
*model \= User*  
*fields \= \['username', 'email', 'password'\]*  
*template\_name \= 'form.html'*  
*success\_url \= reverse\_lazy('login')*  
E em usuarios/urls.py, vamos adicionar em *urlpatterns* o endereço desta página:  
*path('registrar/', UsuarioCreate.as\_view(), name='registrar'),*  
Já é possível acessar o endereço e testar a página de registro. No momento, o e-mail não é obrigatório e o campo Senha só aparece uma vez, mas desejo alterar isso para o padrão mais comum, onde o endereço é obrigatório e o campo Senha aparece duas vezes para confirmação.   
Então, na pasta usuarios, nós vamos criar um novo arquivo, o qual ainda não utilizamos, chamado **forms.py**. Nesse arquivo nós iremos configurar o formulário padrão do Django para ficar do nosso "agrado". Nesse arquivo, vamos colocar:  
*from django import forms*  
*from django.contrib.auth.models import User*  
*from django.contrib.auth.forms import UserCreationForm*

*class UsuarioForm(UserCreationForm):*  
*email \= forms.EmailField(max\_length=100)*  
*class Meta:*  
*model \= User*  
*fields \= \['username', 'email', 'password1', 'password2'\]*  
E, de volta para o *usuarios/views.py*, vamos modificá-lo para:  
*from django.views.generic.edit import CreateView*  
*~~from django.contrib.auth.models import User~~*  
*from django.urls import reverse\_lazy*  
***from .forms import UsuarioForm***

*class UsuarioCreate(CreateView):*  
***form\_class \= UsuarioForm***  
*~~model \= User~~*  
*~~fields \= \['username', 'email', 'password'\]~~*  
*template\_name \= 'form.html'*  
*success\_url \= reverse\_lazy('login')*  
Em negrito está o que adicionamos e em tachado (~~texto~~) está o que vamos remover. O motivo de removermos essas três linhas é porque elas já estão no arquivo forms, que importamos. Agora resta colocar um link no menu do navbar para a página de registro e está pronto.   
**Adicionar usuário em grupos**

Em alguns casos, onde há poucos grupos e/ou grupos previamente definidos, essa parte é feita manualmente pelo administrador pela página admin. Contudo, caso se deseje adicionar um usuário a um grupo automaticamente durante o registro, é possível.   
Para isso, vamos em *usuarios/views.py* para adicionar as seguintes importações:  
*from django.contrib.auth.models import Group*  
*from django.shortcuts import get\_object\_or\_404*  
No mesmo arquivo, vamos adicionar ao fim da classe *UsuarioCreate*:  
*class UsuarioCreate(CreateView):*  
*…*  
*success\_url \= …*

*def form\_valid(self, form):*  
	*grupo \= get\_object\_or\_404(Group, name="Professor")*  
	*url \= super().form\_valid(form)*  
	  
	*self.object.groups.add(grupo)*  
	*self.object.save()*  
*return url*  
A linha *grupo \= …* serve para garantir que um grupo com este nome ("Professor") existe e, após a validação dos dados (linha *url \= …*), o usuário criado é adicionado a este grupo.

**Extra:** Para que não seja possível utilizar o mesmo e-mail para diferentes cadastros, podemos utilizar a função *clean* para esta validação extra. Em *usuarios/forms.py*, vamos importar:  
    *from django.core.exceptions import ValidationError*  
E vamos adicionar após a classe *Meta*:  
*def clean\_email(self):*  
*e \= self.cleaned\_data\['email'\]*  
*if User.objects.filter(email \= e).exists():*  
*raise ValidationError("O email {} já está em uso.".format(e))*  
*return e*  
Podemos utilizar essa função (*clean\_nome-do-atributo*) para realizar qualquer validação extra, além de retornar (*raise*) mensagens de erro para o usuário.  


[image1]: <data:image/png;base64,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>