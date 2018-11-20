# mongoDb

comando do mongodB

exemplo-> 
https://www.youtube.com/watch?v=pWbMrx5rVBE

//--directoryperdb

//Usa um diretório separado para armazenar dados para cada banco de dados. 
Os diretórios estão sob o --dbpathdiretório e cada nome de subdiretório corresponde ao nome do banco de dados.

//--dbpath <path> 	
	
//O diretório no qual a instância do mongod armazena seus dados.


mongod --directoryperdb --dbpath C:\mongodb\data\db --logpath C:\mongodb\log\mongo.log --logappend --install
 
// startando o serviço do mongo
net start MongoDB


// ver á versão do mongo
mongo

// limpa dados da tela do prompt
cls

// mostra os bancos disponiveis
show dbs

//cria um database
use nomeDataBase

//saber qual o database que estou usando no momento
db

# Criação do usuário do banco 

//comando de criação de um usuario no banco de dados e seu nivel de acesso

db.createUser({

	user: "lais",	
	pwd: "123456",	
	roles: [ "readWrite", "dbAdmin" ]
	
});

//cria no nome da tabela

db.createCollection('nomeDaTabela');

//mostra as tabelas criadas

show collections

 
# Inserção 

//acessa os dados no database atual, no qual tem uma tabela chamada customers,
//o qual quero inserir apenas os seguintes dados entre parenteses e chaves

db.customers.insert({first_name:"John",last_name:"Doe"});

//inserir varios dados os mesmo tempo
db.customers.insert([

	{first_name:"Steven",last_name:"Smith"},	
	{first_name:"Joan",last_name:"Johnson", gender:"female"}
	
]);

// inserção em cadeia

db.customers.insert([

	{
		first_name:"Troy",
		last_name:"Makons",
		gender:"male",
		age:33,
		address:{
			street:"432 Essex st",
			city:"Lawrence",
			state:"MA"
		},
		memberships:["mem1", "mem2"],
		balance:125.32
	}, 
	{
		first_name:"Beth",
		last_name:"Jenkins",
		gender:"female",
		age:23,
		address:{
			street:"411 Blue st",
			city:"Boston",
			state:"MA"
		},
		memberships:["mem2", "mem3"],
		balance:505.33
	},
	{
		first_name:"Timothy",
		last_name:"Wilkins",
		gender:"male",
		age:53,
		address:{
			street:"22 School st",
			city:"Amesbury",
			state:"MA"
		},
		memberships:["mem3", "mem4"],
		balance:22.25
	},
	{
		first_name:"Willian",
		last_name:"Jackson",
		gender:"male",
		age:43,
		address:{
			street:"11 Albany st",
			city:"Boston",
			state:"MA"
		},
		memberships:["mem1"],
		balance:333.23
	}, 
	{
		first_name:"Sharon",
		last_name:"Thompson",
		gender:"female",
		age:35,
		address:{
			street:"19 Willis st",
			city:"Worchester",
			state:"MA"
		},
		memberships:["mem1", "mem2"],
		balance:99.99
	}
	
]);


# Busca 

//traz dos os dados daquele database contendo aquela tabela informada

db.customers.find();

// traz os dados ispecificos que eu estiver passando

db.customers.find({first_name:"John"});

//realiza á busca de ambos os dados informados

db.customers.find({$or:[{first_name:"John"},{first_name:"Steven"}]});

//busca todos que tenham menos de 40 anos

db.customers.find({age:{$lt:40}});

//faz consulta de todos os dados quando o city seja igual á boston(exemplo de objeto)

db.customers.find({"adress.city":"Boston"});

//faz consulta de todos os dados quando o memberships seja igual á mem1(exemplo de array)

db.customers.find({memberships:"mem1"});

//ele mostra os registros ideitado

db.customers.find().pretty();


db.customers.find().sort({last_name:1});

db.customers.find().sort({last_name:-1}).pretty();

//conta á quantidade de registros totais da tabela customers

db.customers.find().count();

//conta á quantidade de registros que o genero sejá igual á male da tabela customers

db.customers.find({gender:"male"}).count();

// mostra apenas 4 registros da tabela

db.customers.find().limit(4);

// ordene pelo last_name do A á Z, e mostra apenas 4 registros da tabela

db.customers.find().limit(4).sort({last_name:1});

//traga todos os campos, porem quero da forma Customer Name: first_name

db.customers.find().forEach(function(doc){print("Customer Name:"+doc.first_name)});
 
 

# Atualização 
 
//atualização de dados da tabela customers
procure dados da linha em que first_name seja = "John"
e atualize por {first_name:"John",last_name:"Doe", gender:"male"}

db.customers.update(

	{first_name:"John"},
	{first_name:"John",last_name:"Doe", gender:"male"}
	
 );
 
//para atualizar á tabela criando uma coluna da tabela ->{$set:{ gender:"male"}}

db.customers.update(

	{first_name:"Steven"},
	{$set:{gender:"male"} }
	
 );


db.customers.update(

	{first_name:"Steven"},
	{$set:{age:45} }
	
 );


db.customers.update(

	{first_name:"Steven"},
	{$inc:{age:5} }
	
 );

 //remove coluna e dado da tabela
 
db.customers.update(

	{first_name:"Steven"},
	{$unset:{age:1} }
	
 );


 //NÃO ATUALIZA
 
db.customers.update(

	{first_name:"Mary"},	
	{$unset:{age:1} }
	
 );

 // adiciona os dados de Mary no banco
 
db.customers.update(

	{first_name:"Mary"},	
	{first_name:"Mary",last_name:"Samson"},{upsert:true}
	
 );

//renomeia apenas o nome da coluna de gender para sex

db.customers.update(

	{first_name:"Steven"},	
	{$rename:{"gender":"sex"} }
	
 );

 
# Exclução 
 
//exclui os dados do Steven da tabela

db.customers.remove({first_name:"Steven"});


db.customers.remove({first_name:"Steven"}, {justOne:true});
