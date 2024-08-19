**Hospedagem**

Hospedagem é um processo muito importante no processo de criar um sistema para internet, já que sem a hospedagem, basicamente ninguém será capaz de utilizar o seu sistema. Normalmente, é necessário pagar algum servidor para ser apto a hospedar um site em seu domínio. O endereço escolhido (Ex.: google.com ou wikipedia.com) também costuma ser um custo extra na hospedagem de um sistema. Esse custo pode ser do programador, da empresa na qual ele trabalha ou até mesmo do próprio cliente, a depender do contexto da situação

No entanto, existem alguns servidores gratuitos pela internet, um deles sendo o que iremos utilizar para teste, [Python Anywhere](https://www.pythonanywhere.com/). Esses servidores, por serem um serviço gratuito, costumam ser limitados de diversas formas e lentos. Ainda assim, são de extrema importância para um primeiro teste do nosso projeto. Seja um teste interno entre o grupo responsável por criar o sistema, um teste feito com o cliente para que este veja o progresso atual ou até mesmo um teste aberto para qualquer usuário para se ter um feedback sobre como prosseguir. Além de também ser útil para descobrir, e posteriormente corrigir, bugs.

Em relação ao Python Anywhere, ele permite que você hospede uma página web sem custo algum. Como limitação, há o limite de uso de CPU de 100s por dia e um máximo de armazenamento de 512 MB. Além disso, é necessário acessar o servidor ao menos uma vez a cada três meses, para que o site não seja retirado do ar. Já em relação ao endereço, não é possível escolher um nome, sendo este o mesmo do seu usuário. Ex.:  
	user.pythonanywhere.com

**Importante:** Leve essa informação acima em conta quando for definir o seu nome de usuário, já que colegas, professores e até mesmo clientes irão precisar usá-lo para acessar o seu site.

Vamos começar criando uma conta no site. Acesse o site e, no canto superior direito, clique em *Pricing & signup* ou acesse esse [link](https://www.pythonanywhere.com/pricing/). O Python Anywhere também possui alguns planos pagos, oferecendo uma melhor qualidade de serviço em troca. Contudo, vamos começar com o plano *Beginner*, que é 100% Free. Na faixa amarela, clique em *Create a Beginner account*. Preencha com o seu nome de usuário, email e senha. **Lembre-se que o seu nome de usuário será o mesmo utilizado no link de sua página\!**

Ao logar, vamos nos deparar com o *DashBoard*, que seria como um painel principal. Dentre várias opções, as mais importantes no momento serão: *Consoles*, onde poderemos abrir um terminal para executar comandos; *Files*, onde iremos carregar os nossos arquivos; e *Web apps*, que é onde faremos algumas configurações do nosso sistema. Além disso, também é possível ver o uso atual da CPU e do armazenamento, assim como o máximo de cada um.

Antes de começarmos a interagir com o servidor, vamos preparar o nosso projeto para o *upload*. O primeiro passo vai ser criar na pasta do projeto um arquivo chamado secret\_key.txt. Ele vai ficar na mesma pasta que o manage.py.  
![][image1]

Caso você abra o arquivo settings.py, você verá que há uma chave SECRET\_KEY com um valor sendo uma sequência aleatória de letras, números e símbolos. Essa chave é usada para verificações de segurança entre comunicações com o seu site, buscando evitar roubo de dados como login e senha do usuário, por exemplo. O django provê uma chave de exemplo inicial para testes, mas ao hospedar um site, queremos usar uma chave mais segura e única para o nosso site. Para gerar um chave como essa, podemos executar no terminal do próprio VS o seguinte código:

	python \-c "from django.core.management.utils import get\_random\_secret\_key; print(get\_random\_secret\_key())"

Esse código irá retornar, no próprio terminal, uma nova chave que poderemos usar como *secret\_key* do nosso site. Contudo, ao invés de modificarmos diretamente o arquivo settings.py, vamos adicionar essa chave no arquivo secret\_key.txt que criamos anteriormente e depois faremos essa importação no settings.py, mas isso apenas após hospedarmos o site.

A seguir vamos criar mais um arquivo, desta vez **fora** da pasta do projeto. Ele será requirements.txt e irá conter todas as importações necessárias para o funcionamento do nosso projeto. Tendo como base o projeto que vimos em aula, o arquivo requirements.txt iria conter:

django  
django-crispy-forms  
crispy-bootstrap4  
django-braces  
django-cleanup

Esse arquivo não é de fato necessário, mas iremos precisar instalar todas essas importações no servidor e será mais simples ao utilizar um arquivo dessa forma. Agora, iremos selecionar a pasta do nosso projeto e o arquivo requirements.txt e iremos zipar tudo num único arquivo (Ex.: projeto.zip).

Vamos retornar ao Python Anywhere e, no DashBoard, vamos clicar em *Files*. Na página que irá abrir, vamos clicar em *Upload a file* e vamos escolher o arquivo zipado (Ex.: projeto.zip) que irá conter o nosso projeto e o txt com os requisitos.

Agora precisamos *"deszipar"* esse arquivo e iremos fazer isso pelo terminal. Para isso, vamos clicar em *Open Bash console here*, essa opção fica na parte superior da tela, logo abaixo de DashBoard. No console, vamos digitar:  
	*unzip arquivo.zip*

Vamos esperar esse processo ser concluído e, ainda no console, vamos digitar:  
	*mkvirtualenv projeto-env*

Com *projeto-env* sendo qualquer nome que você queira dar a essa máquina virtual. Usar uma máquina virtual é um passo recomendado ao se utilizar o django e também será útil no caso de você desejar hospedar um outro site com diferentes requisitos, sendo necessário apenas deletar e criar uma nova máquina virtual, sem a necessidade de resetar o servidor como um todo. Após o ambiente virtual ser criado (isso pode levar alguns minutos), vamos digitar:  
	*pip install \-r requirements.txt*

Esse comando irá instalar todas as importações que colocamos no arquivo txt. Assim, podemos deixar o console executando cada uma das instalações de forma automática enquanto concluímos o restante da configuração.  
**Extra:** Caso seja necessário, use *workon projeto-env* para trabalhar dentro do ambiente virtual e *deactivate* para sair do ambiente.  
**Extra 2:** Ao voltar ao DashBoard, você verá que há um link para o console na área Consoles, mesmo se você tiver fechado a janela. Isso porque o console permanece aberto no servidor a não ser que você use o comando *exit*. Só é permitido ter no máximo três consoles abertos por vez.

Voltando ao DashBoard, vamos clicar em *Web Apps* para criar o endereço do nosso site. Vamos criar um *new web app* e, quando solicitado, vamos escolher a opção *Manual Configuration*. **Não** escolher a opção Django. Essa opção é para o caso de quem vai começar um projeto Django do zero e editar pelo próprio servidor (basicamente o que fizemos desde o início). Após o app ser criado, vamos preencher alguns campos:  
**Atenção:** Troque os nomes abaixo pelos nomes do seu projeto. User é o nome de usuário. Projeto é o nome da pasta do seu projeto. É possível conferir indo em DashBoard \> Files e vendo no lado esquerdo o nome da pasta.  
	Em Code, Source Code:  
		*/home/user/projeto*  
	Em Code, Working directory:  
		*/home/user/*  
	Em Virtualenv:  
		*/home/user/.virtualenvs/projeto-env*  
	Em Static files, URL:  
		*/static/*  
	Em Static files, Directory:  
*/home/user/projeto/static*

A parte de configuração nesta página está pronta, agora vamos editar o arquivo wsgi.py. Vamos procurar novamente por *Code* e vamos clicar no link para o arquivo wsgi. O link deve ser algo como:  
	/var/www/user\_pythonanywhere\_com\_wsgi.py

Esse arquivo wsgi é diferente do que temos no nosso projeto django. O servidor irá utilizar este e irá ignorar o padrão do projeto. Ao clicar, vamos apagar o que tiver e colar o seguinte modelo:

*\# \+++++++++++ DJANGO \+++++++++++*  
*\# To use your own Django app use code like this:*  
*import os*  
*import sys*

*\# assuming your Django settings file is at '/home/user/projeto/projeto/settings.py'*  
*path \= '/home/user/projeto'*  
*if path not in sys.path:*  
*sys.path.insert(0, path)*

*os.environ\['DJANGO\_SETTINGS\_MODULE'\] \= 'projeto.settings'*

*\#\# Uncomment the lines below depending on your Django version*  
*\#\#\#\#\#\# then, for Django \>=1.5:*  
*from django.core.wsgi import get\_wsgi\_application*  
*application \= get\_wsgi\_application()*  
*\#\#\#\#\#\# or, for older Django \<=1.4*  
*\#import django.core.handlers.wsgi*  
*\#application \= django.core.handlers.wsgi.WSGIHandler()*

Novamente, lembre-se de substituir os nomes *user* e *projeto* pelos mesmos nomes do seu usuário e pasta do projeto. Essa parte final é dividida em dois blocos. O primeiro é para o caso da versão do Django ser 1.5 ou superior (que é o nosso caso) e o segundo bloco (comentado) é para o caso de ser uma versão mais antiga do que a 1.5. Acredito que será improvável utilizar uma versão mais antiga, mas caso seja necessário, basta comentar o primeiro bloco e descomentar o segundo.

Agora vamos voltar no DashBoard e vamos clicar em Files. Vamos navegar até o nosso arquivo settings.py. Algo como:  
	*/home/user/projeto/projeto*

Na aba à direita vai ter os arquivos que esta pasta contém. Vamos clicar no arquivo settings.py e ele será aberto para edição. Vamos procurar, próximo do início, pela chave SECRET\_KEY. Nós vamos alterar esta chave e as duas seguintes (DEBUG e ALLOWED\_HOSTS), além de adicionar algumas flags após elas. De IA em diante permanece tudo igual. Então vamos trocar algo como:

![][image2]

por:

*\# SECRET\_KEY \= 'django-insecure-\_...'*  
*with open('/home/user/projeto/secret\_key.txt') as f:*  
*SECRET\_KEY \= f.read().strip()*

*\# SECURITY WARNING: don't run with debug turned on in production\!*  
*DEBUG \= False*

*ALLOWED\_HOSTS \= \['user.pythonanywhere.com'\]*

*SECURE\_SSL\_REDIRECT \= True*  
*SESSION\_COOKIE\_SECURE \= True*  
*CSRF\_COOKIE\_SECURE \= True*  
*SECURE\_HSTS\_INCLUDE\_SUBDOMAINS \= True*  
*SECURE\_HSTS\_PRELOAD \= True*  
*SECURE\_HSTS\_SECONDS \= 3600 \# 3600 segundos, uma hora*

Mais uma vez, **atenção** para os *user* e *projeto*. Troque-os pelos nomes corretos. É aqui onde vamos importar a chave naquele arquivo secret\_key.txt ao invés de usar o padrão do Django. O DEBUG False vai trocar as páginas de erro que costumamos ver durante a implementação do sistema por páginas padrão do servidor, de forma que o usuário não consiga ver dados do sistema como páginas disponíveis ou trechos do código. O ALLOWED\_HOSTS vai permitir comunicações entre páginas desse domínio. No caso de você usar imagens hospedadas em outro servidor, é aqui onde essa permissão será adicionada. Ex.:  
	*ALLOWED\_HOSTS \= \['user.pythonanywhere.com', 'server.imgserver.com'\]*

As seis flags que vem depois são referentes à segurança do site e podem causar alguns conflitos durante a implementação e testes do sistema, e é por isso que vamos adicionar apenas no servidor e não no arquivo que temos no computador e usamos para testes.

**Flags:**  
*SECURE\_SSL\_REDIRECT* redireciona requisições http para https. Ou seja, se alguém tentar acessar o seu site com o link:  
	***http**://user.pythonanywhere.com/*  
Ele será automaticamente redirecionado para:  
	***https**://user.pythonanywhere.com/*

*SESSION\_COOKIE\_SECURE* informa que os cookies (dados que mantém o usuário logado ao site) devem ser encriptados antes de serem enviados.

*CSRF\_COOKIE\_SECURE* é similar ao anterior, mas referente ao CSRF, aquele token que o Django usa nos formulário para evitar alguns ataques cibernéticos.

As três flags seguintes são referentes à mesma coisa e funcionam em conjunto. Caso um domínio não seguro (*http://…*) tente acessar o seu site, o acesso desse domínio ao seu site será bloqueado por um determinado tempo. No exemplo acima, seria por uma hora, para evitar um bloqueio muito longo em caso de algum erro ou devido a algum teste. Mas o valor padrão para essa flag é de um ano (31.536.000 segundos).

Por fim, vamos salvar a edição do arquivo settings.py e vamos retornar para o DashBoard. Vamos abrir o console e ver se as instalações já foram concluídas. Vamos esperar, se necessário e, após tudo ter sido devidamente instalado, vamos retornar ao DashBoard e clicar em Web Apps. Vamos clicar no botão Reload e, após um momento, clicar no link acima para testarmos o nosso sistema no ar e disponível para qualquer pessoa do globo\!

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAI0AAABGCAYAAADxcRfSAAAFfElEQVR4Xu2bz2sbRxTH88/EtMQmqlswbRqEU7eYRjFtXNcllyohh7o4vYTia2RoC0mhF1MkVQTaHnKRD27rSyAYdBak/9Gr9sfMzrx5s7uPuNhafQ8f8O7Mm1nt+2hm1pq9cvXqEgGg4Qo/AUAVkAaogTRADaQBaiANUKOU5hotrd2j1u5jWt19SMut60Id0HRU0izvP6eb3wzo0+FL+jzllD7cDOuBZqOSpnVwQp88eUF3rDQvqb0d1gPNppY0b239SO2DI4c/7GjT3r5FK7Pp6voNTFWLQrU0y49pvV+MLJz29iNqJ3/3n9O7y0K8pUfj6YRGwzFNp9OU8eHs/CE7NvUfjGiSn/fK0vNj6jlxk+H96jhedjai3nDixXZnxyZuetyLf4ZDv51uWnafRmdSf7NrDdqZb6ql6RzRlpGkfzqbnpwRZ/8RXdt8Wow6nwnxluSGO8kwSfeOixvcnck1epDHumUm8SYuPZ7YutE4k3BTlrdjpUnqWgGWqHfMZLRtuKJk9cy1pNI5siXHYRvzT7U0Hz+l26kUJ/TBZodW9x1pkvWNHYVO6WZHiLewpFUe81hXGvfbK3zDhTie0IQiqUIbiUTBaCNco3s9yd/eyBP7PPNNtTR2ehpQa3sQTE+WutNTVBLpOJ8CUmLSZN/2IuFynPSt59IUMTnOiCJfozlnrscRxROoWVRLk3DjIa3u3qO3rTQntOEtjI9obev9MM6D3/CyY6msjjTxOGmkKaaguqMCb38plGM2QiVtSpI2hXrSGLyRJlnfvKDbB3thPRF+w0uOuRjBmiYiDS8L4pz+2JomlUocGdypKx/FrHxZmS/HrM7ZOMW9xiahk2bzF+d/NCezhfCAPtr/MqwnUiKJcOw/yYxrjjQlcQnOE5f09JQuap3pKWuTSzPxngD56GXbEc43BZ00Ca0OrdxNfkbYo5W198LyOcJfC9WBiy6jb3e+0EvTFNgjfj1qSMPXOA1kcaTJ1zDF9FORfJEyacwTWKy8OSyONODcgDRADaQBaiANUANpgBpIA9RAGqAG0gA1kAao0Umz8S09efYzPYvSo70NIW4OkLZOABmdNDs/0e9//0N/RfmTftgR4i4UYVeewP8nTb3+U9iW08uKTpq5pF7SIE19dNJ0+/Tq9b/02uHV4OuwXoTYbv/Y+aAsv6HZjrtRtiHK3GTvB0nz63W+aYrF8+uy/Xib1d3NVW475gdJQYZgr4/QP98Mlv/a/pv7Oac1JbsgdNK0blFn5wu669BZfyesJxH7FpW8BcB30/WGhTT+tgb267PXppBcgUKaLNFFfRbviMFHJ3mLZ9h/EcfKYvfokvHG0vjcofWWEJcQfAsTwhtavAUQ37fLXy/hycsSb/oS+hDI2hiF2zeDRDrX5e2diV2v1H9eN9kB6F530NflRCeNMD35nNGvXSHOYLdb+gl1h+ViGnET7yNKw9som0YEbBs8ae4W0WD6iAnkEulf2gTWSGnOC3tzYt/OhPiGJ0macFowRJLGMKNVsL/XjnxhjCkvf/tA6j/73ONjNkI2UproI3efvuN1y+DrgsiNShMYWdN4CeKLSw8paSF8nVEkk69xOL3w7QNvKg7799dPsbXY5UUnTfSfe9/TV7wuxxvm/QTLbwEIZXkiA2mC9t2kO2UlCfHXRflTj6nPt4qydoLRia/f3P75tOQdF9N1XNKLRycNEGn62wecBZOG/d8k540SHl0AN5cFk+Y8MVNJbC3VXCANUANpgBpIA9RAGqAG0gA1kAaogTRADaQBaiANUANpgBpIA9RAGqAG0gA1kAaogTRADaQBaiANUANpgBpIA9RAGqAG0gA1kAaogTRADaQBaiANUANpgBpIA9RAGqDmP1V0VKpZUUS8AAAAAElFTkSuQmCC>

[image2]: <data:image/png;base64,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>