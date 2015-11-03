# QuestionnaireWeb
Esta é uma aplicação para gerenciamento de questionário e usuários. O usuário cadastrado pode responder a qualquer questionário apenas com seu email.

A seguir está detalhado algumas ferramentas utilizadas no desenvolvimento e na execução deste projeto.
Para cada ferramente, está detalhado como realizar sua configuração para que elas possam realizar o build da aplicação, assim como também a sua execução.

-----------------------------------------------------

Maven
-----
A primeira ferramenta a ser detalhada é o 'Maven', que é utilizado para gerenciar as dependências utilizadas pela aplicação. Com ele é possível também gerenciar algumas documentações para o sistema, além de ser possível executar a aplicação diretamente no servidor Tomcat (detalhado a seguir).
O projeto foi desenvolvido utilizando a versão 3.3.3 do Maven, que pode ser encontrada para download no link (http://ftp.unicamp.br/pub/apache/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.zip).
Para sua configuração, siga as instruções:

	1. Fazer download desta versão do Maven;
	2. Descompactar os fontes no diretório: "C:\Desenvolvimento";
	3. Incluir o endereço do diretório Maven ("C:\Desenvolvimento\apache-maven-3.3.3\bin") na variável "Path" do windows;
	4. Caso o Maven seja instalado em outro diretório, é preciso alterar a propriedade 'mvn.executable' presente no arquivo "common.properties", com o endereço do diretório e o arquivo batch referente ao executável do Maven. (ex. mvn.executable=C\:\\Desenvolvimento\\apache-maven-3.3.3\\bin\\mvn.cmd).
	
------------------------------------------------------

Tomcat
------
O Tomcat foi o servidor de aplicações web utilizado para o desenvolvimento deste projeto.
Ele apresenta funcionalidades simples e consistentes para realização de deploy da aplicação no servidor e também para a execução desta aplicação.
Além disso, existe um plugin Maven que realiza a integração destas duas ferramentas, facilitando a execução da aplicação através de uma propriedade disponível neste plugin.
A configuração necessária para integração do Tomcat e Maven, é apresentado a seguir.
A versão deste servidor utilizada foi a 8.0.28, que pode ser encontrada para download no link (http://mirror.nbtelecom.com.br/apache/tomcat/tomcat-8/v8.0.28/bin/apache-tomcat-8.0.28.zip).
Para a configuração inicial do Tomcat, siga as instruções:

	1. Fazer o download desta versão do Catalina Apache Tomcat Server;
	2. Descompactar os fontes no diretório: "C:\Desenvolvimento";
	
------------------------------------------------------

Configuração 'tomcat-maven-plugin'
----------------------------------

Para que o Maven seja capaz de realizar operações com o servidor Tomcat, é preciso garantir que estas configurações estejam presentes nas configurações do servidor Tomcat.
Para a configuração adicional do Tomcat, siga as instruções:

	1. Adicionar um usuário com as seguintes regras nas configurações do Tomcat ("C:\Desenvolvimento\apache-tomcat-8.0.28\conf\tomcat-users.xml"):
		<tomcat-users>
			<role rolename="manager-gui" />
			<role rolename="manager-script" />
			<user username="admin" password="admin" roles="manager-gui,manager-script" />
		</tomcat-users>
		Obs: O nome de usuário e senha são apenas uma sugestão.
	2. Criar uma autenticação para o Tomcat nas configurações do Maven ("C:\Desenvolvimento\apache-maven-3.3.3\conf\settings.xml")
		<settings ...>
			<servers>
			   
				<server>
					<id>tomcatmaven</id>
					<username>admin</username>
					<password>admin</password>
				</server>

			</servers>
		</settings>
		Obs: O nome de usuário e senha devem ser os mesmos criados na configuração do Tomcat, passo 1. Além disso, o atributo "id" da tag "servidor" adicionado no passo 2, deve conter exatamente este nome para o funcionamento correto do deploy da aplicação desenvolvida aqui no servidor Tomcat. Caso este nome seja alterado, o arquivo de configuração do Maven da aplicação web desenvolvida, deve ser alterado também.
		
-----------------------------------------------------

Mysql
-----

O geranciador de banco de dados escolhido para o desenvolvimento desta aplicação foi o Mysql.
Este gerenciador é simples e gratuito e é capaz de realizar as operações necessárias para avaliação do projeto desenvolvido aqui.
A versão do gerenciador utilizado foi a 5.7.9 que pode ser encontrada para download no link (http://dev.mysql.com/downloads/file/?id=459896).
Sua configuração é simples, basta seguir as seguintes instruções:

	1. Fazer o download do arquivo "mysql-installer-community-5.7.9.0.msi"
	2. Instalar o mysql com o arquivo anterior com as configurações padrão.
		Obs 1: A senha do root pode ser definida como desejar.
		Obs 2: O usuário e senha para utilização na aplicação serão definidas no script de criação do banco de dados.
	3. Incluir o endereço do diretório onde foi instalado o MYSQL  (se não foi alterado durante a instalação é o endereço "C:\Program Files\MySQL\MySQL Server 5.7\bin", verificar se foi instalado neste endereço) na variável "Path" do windows.

-----------------------------------------------------

Criando o banco de dados
------------------------

Para criar o banco de dados, foi disponibilizado no diretório deste projeto "script_db" o script "scriptBD.sql", que contém a definição do banco de dados. Neste script, é criado também o usuário que será utilizado pela aplicação.
Existe um script ("scriptCreate.sql") que contém as instruções para criar as tabelas da aplicação e que também cria as ligações entre as tabelas.
Por fim, tem-se um script para incluir o primeiro usuário ("scriptCharge.sql"), este que será responsável por incluir de outros usuários. O nome do usuário é "Administrador", o email é "admin@email.com" e a senha é "admin".
Todos os usuários cadastrados poderão gerenciar os questionários cadastrados, além de cadastrar novos questionários e editar questionários já cadastrado.
Para executar os scripts descritos aqui, basta seguir as seguintes instruções:

	1. Abra o prompt do Windows;
	2. Altere o diretório corrente para o diretório onde se encontra os scripts;
	3. Execute o comando "mysql -u root -p". Será requisitado a senha do usuário root definida durante a instalação do mysql;
	4. utilize o comando "\. <script-file>" para executar cada script descrito aqui. É importante que eles sejam executados na ordem em que estão sendo descritos, caso contrário poderá ser gerado algum erro.
		> \. scriptBD.sql
		> \. scriptCreate.sql
		> \. scriptCharge.sql


-----------------------------------------------------

Eclipse
-------

Como ambiente de desenvolvimento foi utilizado o Eclipse que apresenta várias fucionalidades para o desenvolvimento de aplicações.
A versão utilizada foi a "Mars release (4.5.0)" que apresenta um plugin Maven já integrado.

-----------------------------------------------------

Deploy da aplicação
-------------------

O padrão de desenvolvimento utilizado foi o MVC (Model, View, Control).
Com este padrão é possível dividir a aplicação em camadas de tal forma que cada camada seja responsável por uma funcionalidade na aplicação.
Assim, realizar alterações de algumas tecnologias, como por exemplo alterar o gerenciador de bandos utilizado, pode ser feita facilmente e sem grandes mudanças na aplicação como um todo.
Desta forma, para realizar o deploy da aplicação é preciso realizar duas etapas separadamente.

Primeira etapa
--------------
Na primeira etapa do deploy, deve ser executado um comando Maven no módulo "Questionnaire".
Este módulo contém as operações de persistência de informação no banco de dados, além de conter também algumas regras de negócio da aplicação.
Pare realizar o deploy deste módulo, deve seguir as seguintes instruções:

	1. Abra o prompt do Windows (console no Linux);
	2. Altere o diretório corrente para o diretório onde se encontra o módulo "Questionnaire";
	3. Execute o seguinte comando Maven:
		>mvn install
	4. Ao final da execução é preciso exibir a mensagem "BUILD SUCCESS".

Como forma alternativa, é possível executar esta operação diretamente no ambiente Eclipse, através de uma target Ant.
Para isso, siga as instruções:

	1. Abra o projeto "Questionnaire" no ambiente do Eclipse;
	2. Arraste o arquivo "build.xml" presente neste projeto, para o "Ant View Window" presente no eclipse (garanta que esta view do Eclipse esteja disponível e aberta);
	3. Na janela "Ant View Window" do eclipse deve aparecer várias targets Ant que podem ser executadas, dentre elas tem a target "install" que realiza a mesma operação descrita na instrução anterior. Esta target executa também outras operações Maven, como o "clean", "compile" e "package" antes de executar o comando "install".
	4. Ao final da execução é preciso exibir também a mensagem "BUILD SUCCESS".
Obs: Caso este módulo não seja exibido corretamente no eclipse, talvez seja necessário reconstruir o arquivo .project utilizado pelo eclipse para exibir corretamente o módulo e suas dependências Maven. Para isso basta executar a target "eclipse:eclipse" do Ant, ou executar o comando ">mvn eclipse:eclipse" na linha de comando no diretório do projeto.

Após esta etapa, o módulo "Questionnaire" da aplicação se encontra instalado nas dependências do Maven.

Segunda etapa
-------------
Na segunda etapa do deploy da aplicação, deve ser executado um comando Maven no módulo "QuestionnaireWeb".
Este módulo apresenta as páginas web a serem exibidas para o usuário da aplicação, além de várias regras de negócio e de controle da aplicação.
Para preparar o módulo para ser executado no servidor Tomcat, siga as instruções:

	1. Abra o prompt do Windows (console no Linux);
	2. Altere o diretório corrente para o diretório onde se encontra o módulo "QuestionnaireWeb";
	3. Execute o seguinte comando Maven:
		>mvn package
	4. Ao final da execução é preciso exibir a mensagem "BUILD SUCCESS".

Da mesma forma realizada na etapa anterior, é possível realizar esta operação diretamente no ambiente Eclipse, também através de uma target Ant.

	1. Abra o projeto "QuestionnaireWeb" no ambiente do Eclipse;
	2. Arraste o arquivo "build.xml" presente neste projeto, para o "Ant View Window" presente no eclipse (garanta que esta view do Eclipse esteja disponível e aberta);
	3. Na janela "Ant View Window" do eclipse deve aparecer várias targets Ant que podem ser executadas, dentre elas tem a target "package" que realiza a mesma operação descrita na instrução anterior. Esta target executa também outras operações Maven, como o "clean" e "compile" antes de executar o comando "package".
	4. Ao final da execução é preciso exibir também a mensagem "BUILD SUCCESS".
Obs 1: Da mesma forma realizada na primeira etapa, caso este módulo não seja exibido corretamente no eclipse, talvez seja necessário reconstruir o arquivo .project utilizado pelo eclipse para exibir corretamente o módulo e suas dependências Maven. Para isso basta executar a target "eclipse:eclipse" do Ant, ou executar o comando ">mvn eclipse:eclipse" na linha de comando no diretório do projeto.

Após esta etapa é gerado um arquivo .war dentro do diretório "target" do módulo "QuestionnaireWeb". Este arquivo contém todas as páginas web da aplicação, assim como as regras de negócio e controle da aplicação, e inclui também todas dependências utilizadas pela aplicação. O módulo "Questionnaire" gerado na etapa anterior está incluso nestas dependências.

-----------------------------------------------------

Execução da aplicação
---------------------

Existe três formas de executar a aplicação no servidor Tomcat.

Primeira
--------
No módulo "QuestionnaireWeb" é possível executar a aplicação através de uma propriedade disponibilizada pelo plugin "tomcat-maven-plugin".
Para executar a aplicação, siga as instruções:

	1. Abra o prompt do Windows (console no Linux);
	2. Altere o diretório corrente para o diretório onde se encontra o módulo "QuestionnaireWeb";
	3. Execute o seguinte comando Maven:
		>mvn tomcat:run
	4. Se não ocorreu nenhum erro, o servidor já está pronto e a aplicação já se encontra sendo executada no endereço local (http://localhost:8080/QuestionnaireWeb/).

Segunda
-------
É possível realizar também esta operação diretamente no ambiente Eclipse, também através de uma target Ant.

	1. Abra o projeto "QuestionnaireWeb" no ambiente do Eclipse;
	2. Arraste o arquivo "build.xml" presente neste projeto, para o "Ant View Window" presente no eclipse (garanta que esta view do Eclipse esteja disponível e aberta);
	3. Na janela "Ant View Window" do eclipse deve aparecer várias targets Ant que podem ser executadas, dentre elas tem a target "tomcat:run" que realiza a mesma operação descrita na instrução anterior. É possível executar esta merma operação, porém executando outras operação Maven. Para isso, deve ser executado a target Ant "tomcat:run_all", que executa também as operações Maven "clean", "compile" e "package" antes de executar o comando "install".
Obs 1: Existe uma dependência da aplicação que deve ter uma versão específica para a execução desta operação. A versão do "slf4j-log4j12" deve ser a "1.7.12", caso contrário o comando "tomcat:run" não será executado. Não foi possível encontrar em tempo hábil a razão desta inconsistência na aplicação.
Obs 2: Para finalizar o processo do servidor Tomcat utilizando este plugin, é preciso finalizar manualmente o processo "java" que ele cria. Sendo que se este processo não for finalizado, não será possível reexecutar a aplicação.

Terceira
--------
Outra forma de executar a aplicação é copiar o arquivo .war gerado na segunda etapa da instrução anterior, para o diretório Tomcat "<tomcat_dir>/webapps". Em seguida o servidor deve ser iniciado, através da execução do arquivo "<tomcat_dir>/bin/startup.bat". O servidor deve ser iniciado sem nenhum erro e a aplicação já se encontrará disponível par aser acessada pelo link (http://localhost:8080/QuestionnaireWeb/).
A versão da dependência "slf4j-log4j12" não pode ter sido alterada, para que a aplicação seja executada corretamente através desta forma.
A versão correta desta dependência deve ser "1.4.2".
Não foi possível também encontrar em tempo hábil a razão desta inconsistência.

-----------------------------------------------------
