# Data Base MySQL Ordem de Serviço (OS) Oficina Mecânica
### Projeto banco de dados My SQL Workbench

## Modelo ER Entidade Relacionamento extendido
OS_oficina_mecanica.mwb
Tambem disponível em pdf e png como imagens

![Modelo ER Entidade Relacionamento Oficina Mecânica Ordem de Serviço](/src/modeloER_OS_OficinaMecanica.png "Oficina Mecânica Ordem de Serviço")



## DataBase
OS_oficina_mecanica.mwb.bak


## Script de criação, população de dados e queries de consulta
OS_oficina_mecanica_script_final.sql
Script para criação das tabelas, Inserção de dados para popular as tabelas, Queries de consulta


### criar tabelas
-- criar tabela pessoa


create table people(
	idPeople int auto_increment,
    Fname varchar(10),
    Minit varchar(10),
    Lname varchar(20),
    CPF char(11) not null,
    Bdate date,
    phone char(11),
    street varchar(20),
    houseNumber int,
    complement varchar(10),
    district varchar(20),
    city varchar(20),
    state char(2),
    zip_code char(8),
    constraint unique_cpf_people unique (CPF),
    constraint pk_people primary key (idPeople)
);


-- criar a tabela Cliente
create table clients(
	idClient int auto_increment,
	idClientPL int,	
    constraint pk_idClient primary key (idClient),
    constraint pk_idClientPL foreign key (idClientPL) references people(idPeople)
);

-- criar tabela Mecanico
 create table employee(
	idEmployee int auto_increment,
    idEmployeePL int,
    specialty ENUM('Atendimento','Mecanico', 'Eletricista', 'Funilaria', 'Auxiliar'),
    constraint pk_idEmployee primary key (idEmployee),
    constraint pk_idEmployeePL foreign key (idEmployeePL) references people(idPeople)
);

-- criar tabela equipe responsavel
create table team(
	idTeam int auto_increment,
    idTeamEmployee int,
    teamPosition ENUM('Atendimento', 'TimeA', 'TimeB', 'TimeC'),
    constraint pk_idTeam primary key (idTeam),
    constraint pk_idTeamEmployee foreign key (idTeamEmployee) references employee(idEmployee)
);

-- criar tabela oficina mecanica
create table repairShop(
	idRepairShop int auto_increment,
    SocialName varchar(45) not null,
    CNPJ char(15) not null,
    phone char(11),
    street varchar(20),
    houseNumber int,
    complement varchar(10),
    district varchar(20),
    city varchar(20),
    state char(2),
    zip_code char(8),
    constraint repairShopCNPJ unique (CNPJ),
    constraint pk_idRepairShop primary key (idRepairShop)
);

-- criar tabela veículo
create table vehicle(
	idvehicle int auto_increment,
    idClVehicle int,
    license_plate char(7),
    brand varchar (45) not null,
    model varchar (45)not null,
    constraint vehicle_plate unique (license_plate),
    constraint pk_idvehicle primary key (idvehicle),
    constraint fk_idClVehicle foreign key (idClVehicle) references clients(idClient)
);


-- criar tabela OS Ordem de Serviço

create table serviceOrder (
	idServiceOrder int auto_increment,
    kindServiceOrder ENUM('Manutencao', 'Revisão'),
    idVehicleOS int, -- veiculo
    idrepairShopOS int, -- oficina
    idTeamOS int, -- equipe
    idclientOS int, -- cliente
    dateIn date,
    dateOut date,
    statusOS ENUM('Em Serviço', 'Aguardando autorização cliente', 'Aguardando peça', 'Finalizado'),
    descriptionOrder varchar(255),
    clientAuthorization ENUM('True', 'False'),
    constraint pk_idServiceOrder primary key (idServiceOrder),
    constraint fk_idVehicleOS foreign key (idVehicleOS) references vehicle(idvehicle),
    constraint fk_idrepairShopOS foreign key (idrepairShopOS) references repairShop(idRepairShop),
    constraint fk_idTeamOS foreign key (idTeamOS) references team(idTeam),
    constraint fk_idclientOS foreign key (idclientOS) references clients(idClient)
);







-- criar tabela Peças
create table parts (
	idPart int auto_increment,
    part_description varchar(45) not null,
    partRate float not null,
    constraint pk_idPart primary key (idPart)
);

-- criar tabela valor das peças
create table rateParts (
	idrateParts int,
    idPartsOS int,
    quantity float not null,
    totalPartsAmount float not null,
    primary key (idrateParts,idPartsOS),
    constraint fk_idPartsOS foreign key (idPartsOS) references serviceOrder(idServiceOrder),
    constraint fk_idrateParts foreign key (idrateParts) references parts(idPart)
);


-- criar tabela Serviço (mão de obra)
create table services(
	idService int auto_increment,
    service_description varchar(45) not null,
    serviceRate float not null,
    constraint pk_idService primary key (idService)
);

-- criar tabela valor serviço (mão de obra)
create table rateServices (
	idrateServices int,
    idServiceOS int,
    quantity float not null,
    totalService float not null,
    primary key (idrateServices,idServiceOS),
    constraint fk_idrateServices foreign key (idrateServices) references serviceOrder(idServiceOrder),
    constraint fk_idServiceOS foreign key (idServiceOS) references services(idService)
);



### Populando a base de dados
show tables;
desc people;
select * from people;

-- Populando pessoas
insert into people (Fname, Minit,Lname,CPF,Bdate, phone, street,houseNumber, complement, district,city,state,zip_code)
			values ('Carlos', 'Dos', 'Santos',12345678911,'1985-01-09',49999985281,'Rua das Palmeiras',124,'casa', 'Centro','São Paulo','SP','89990000'),
					('Fernanda', 'Maria', 'Pereira',45645678911,'1993-03-10', 11999985299,'Tamandare',65, 'casa', 'Novo Bairro','São Paulo','SP','89990000'),
					('Rafael', 'Carlos', 'Silva',12125678911,'2000-03-03', 31999958281,'Av Paulista',1123, 'apto 103', 'Centro','Chapeco','SC','89990000'),
					('Mariana', 'Mei', 'Meireles',88345678911,'2010-01-10',11999980081,'Rua dos Araças','569','casa','Aeroporto','Curitiba','PR','89990000'),
					('Murilo', 'Fernando', 'Piovesan',33345678911,'1992-06-09',21999985331,'Rua XV de Maio',658,'casa', 'Novo Horizonte','Campinas','SP','89990000'),
					('Paula', 'Chan', 'Moraes',12598678911,'1989-04-09',45999987781, 'Rua Bandeirantes',1266,'fundos','Centro','Hortolandia','SP','89990000'),
					('Antonio', 'de Paula', 'Fratin',12658978911,'1991-02-09',11999989981,'Rua Rio de Janeiro',15624,'casa', 'Nova Mutum','Umuarama','PR','89990000'),
					('Carolina', 'Suzuky', 'Santos',12658678911,'2002-11-10', 11999987881,'Av Altinopolis',1246, 'apto 103','Pedreira','São Paulo','SP','89990000'),
                    ('Fabio', 'Suzuky', 'Santos',66588678911,'2002-06-10', 11779987881,'Av Paraisopolis',46, 'apto 201','Centro','São Paulo','SP','89990000'),
                    ('Arthur', 'Ronesco', 'Santos',56858678911,'2002-05-10', 13889987881,'Av Primo Edruado',1446, 'apto 303','Bairro Novo','São Paulo','SP','89990000'),
                    ('Marcos', 'Junior', 'Silva',37858678911,'2002-10-03', 11888987881,'Av Conde Matarazzo',1246, 'apto 402','Alvorada','São Paulo','SP','89990000'),
                    ('Paulo', 'Souza', 'Martins',74558678911,'2002-08-10', 11779987881,'Av Jardins',1246, 'Sucupira','apto 302','São Paulo','SP','89990000'),
                    ('Joao', 'M', 'Neto',17845678911,'2002-10-09', 11879987881,'Av das Palmeiras',1246, 'Miranda','apto 103','São Paulo','SP','89990000'),
                    ('Altamir', 'T', 'Saboia',12356678911,'2002-07-10', 14899987881,'Av Tucunare',1246, 'Alvorada','apto 77','São Paulo','SP','89990000'),
                    ('Nelmiro', 'S', 'Fagundes',78458678911,'2002-08-10', 11669987881,'Av Amazonas',1246, 'Centro','apto 402','São Paulo','SP','89990000');

-- Populando clientes
insert into clients (idClientPL)
			values (46),
					(47),
                    (48),
                    (49),
                    (50),
                    (51),
                    (52);

-- Populando funcionários
insert into employee (idEmployeePL,specialty)
			values (51,'Atendimento'),
					(52,'Atendimento'),
					(53,'Mecanico'),
					(54,'Eletricista'),
                    (55, 'Funilaria'),
                    (56,'Auxiliar'),
                    (57,'Atendimento'),
                    (58,'Auxiliar'),
                    (59,'Mecanico'),
                    (60,'Mecanico');
                    
-- Populando equipes
insert into team (idTeamEmployee,teamPosition)
			values (1,'Atendimento'),
					(2,'Atendimento'),
                    (3,'TimeA'),
                    (4,'TimeB'),
                    (5,'TimeC'),
                    (6,'TimeA'),
                    (7,'Atendimento'),
                    (8,'TimeB'),
                    (9,'TimeC'),
                    (10,'TimeA');

-- Populando Oficina Mecanica
insert into repairshop (SocialName,CNPJ, phone,street,houseNumber,complement,district,city,state,zip_code)
			values ('Oficina Mecanica','131355660001500','11965875687','Av Paulista',1134,'Ala 1','Bairro Indistrial', 'São Paulo', 'SP',89990000);
			



-- Populando Veiculos
insert into vehicle (idClVehicle,license_plate, brand,model)
			values (1,'AUR1144','Ford','2008'),
					(2,'AHU5568','Fiat','2020'),
                    (3,'ITA5689','VW','2022'),
                    (4,'TYA1245','Audi','2021'),
                    (5,'AQJ7845','BMW','2010'),
                    (6,'LOA3254','Ford','2015'),
                    (7,'AQU7785','Fiat','2009'),
                    (1,'APQ1155','Renault','2010'),
                    (2,'AAS3289','Ford','2013'),
                    (3,'ART9945','Fiat','2022');



-- poulando OS Ordem de serviço
insert into serviceOrder (kindServiceOrder,idVehicleOS, idrepairShopOS,idTeamOS,idclientOS,dateIn,dateOut,statusOS,descriptionOrder,clientAuthorization)
			values ('Manutenção',11,1,3,1,'2022-10-09',null,'Em Serviço','Manutenção no sistema de freio','True'),
					('Revisão',12,1,4,2,'2022-10-09',null,'Aguardando autorização cliente','Manutenção no sistema hidráulico','False'),
                    ('Manutenção',13,1,5,3,'2022-10-09',null,'Aguardando peça','Manutenção no sistema de freio','True'),
                    ('Revisão',14,1,8,4,'2022-10-09',null,'Em Serviço','Revisão básica, Incluir kit revisão.','True'),
                    ('Manutenção',15,1,9,5,'2022-10-09',null,'Em Serviço','Manutenção sistema eletrico','True'),
                    ('Revisão',16,1,10,6,'2022-10-09',null,'Aguardando peça','Revisão do sistema hidráulico','True'),
                    ('Manutenção',17,1,7,1,'2022-10-09',null,'Aguardando autorização cliente','Troca de amortecedores e kit ar','False'),
                    ('Revisão',18,1,4,1,'2022-10-09',null,'Aguardando peça','Manutenção 10.000 KM','True'),
                    ('Manutenção',11,1,3,2,'2022-10-09','2022-11-09','Finalizado','Manutenção no sistema de freio','True'),
                    ('Revisão',12,1,4,3,'2022-10-09','2022-10-10','Finalizado','Revisão 5.000 KM','True');
					


-- populando tabela peças
insert into parts (part_description,partRate)
			values ('Kit Limpador de Parabrisa',1129.99),
					('Kit Pastilhas de freio',549.50),
                    ('Jogo de Amortecedor',825.50),
                    ('Kit Suspensão completo',1532.50),
                    ('Kit retentor isotônico',12180.50),
                    ('Kit reparo sistema hidráulico',325.80);
                    
                    
-- populando tabela Serviço (mão de obra)
insert into services (service_description,serviceRate)
			values ('Hora mecânico básica',80.5),
					('Hora auxiliar básica',40.5),
                    ('Hora mecânico adicional',120.5),
                    ('Serviço revisão',250.25),
                    ('Serviço troca kit freio/ suspensão',320.5),
                    ('Serviço regulagem injeção eletrõnica',125.80);
                    
                    

-- populando tabela relação peças
insert into rateParts (idrateParts,idPartsOS,quantity,totalPartsAmount)
			values (1,1,1.0,1125.5),
					(2,2,1.0,1435.2),
                    (3,3,1.0,2356.2),
                    (4,4,1.0,2356.2),
                    (5,5,1.0,586.3),
                    (6,6,1.0,653.2),
                    (1,3,1.0,825.7);




-- populando tabela relação serviços
insert into rateServices (idrateServices,idServiceOS,quantity,totalService)
			values (1,1,1.0,257.5),
					(2,2,2.0,1435.2),
                    (3,3,1.0,725.2),
                    (4,3,2.0,235.2),
                    (5,4,1.0,284.2),
                    (6,5,1.5,566.3),
                    (7,6,2.5,255.2),
                    (8,1,1.0,825.7);



### Consultas e queries
use oficina;
show tables;
desc rateServices;
select * from people;
select * from clients;
select * from employee;
select * from parts;
select * from rateparts;
select * from services;
select * from rateservices;
select * from repairshop;
select * from serviceorder;
select * from vehicle;
select * from team;


-- consultas basicas
-- pessoa dados das pessoas com concat
select * from people;
select concat(Fname, ' ', Minit, ' ',Lname) as Pessoa, CPF, phone, Bdate from people;
select concat(Fname, ' ', Minit, ' ',Lname) as Pessoa, concat(street,' ',housenumber,' ',complement,' ',district,' ',city,' ',state,' ',zip_code) as Endereço from people;


-- Recuperando a relação de clientes (Nome, CPF e Telefone)
select concat(Fname, ' ', Minit, ' ',Lname) as Cliente, CPF, phone from people, clients
	where idPeople = idClientPL;


-- Recuperando a relação dos veiculos e seus respectivos proprietários
select * from vehicle;
select concat(Fname, ' ', Minit, ' ',Lname) as Cliente, phone, license_plate, brand, model from people, clients, vehicle
	where idPeople = idClientPL and idClient = idClVehicle;

-- Recuperando o(s) veículo(s) de um proprietário específico
select concat(Fname, ' ', Minit, ' ',Lname) as Cliente, phone, license_plate, brand, model from people, clients, vehicle
	where idPeople = idClientPL and idClient = idClVehicle and Fname = 'Carlos';
    
-- Recuperando o(s) proprietário a partir da marca do veículo
select concat(Fname, ' ', Minit, ' ',Lname) as Cliente, phone, license_plate, brand, model from people, clients, vehicle
	where idPeople = idClientPL and idClient = idClVehicle and brand = 'Ford';
    
    
-- Recuperando o(s) proprietário a partir da placa do veículo
select concat(Fname, ' ', Minit, ' ',Lname) as Cliente, phone, license_plate, brand, model from people, clients, vehicle
	where idPeople = idClientPL and idClient = idClVehicle and license_plate = 'LOA3254';
    
    

-- Recuperando a relação dos Mecânicos e sua especialidade
select * from people;
select * from employee;
select concat(Fname, ' ', Minit, ' ',Lname) as Colaborador, specialty as Especialidade from people, employee
	where idPeople = idEmployeePL;

-- Recuperando a relação dos Mecânicos de uma única especialidade ('Atendimento', 'Auxiliar', 'Mecanico', 'Eletricista')
select concat(Fname, ' ', Minit, ' ',Lname) as Colaborador, specialty as Especialidade from people, employee
	where idPeople = idEmployeePL and specialty = 'Mecanico';


-- Recuperando os Funcionários, as equipes e sua especialidade
select * from team;
select * from employee;
select concat(Fname, ' ', Minit, ' ',Lname) as Colaborador, specialty as Especialidade, teamPosition as Posição from people, employee, team
	where idPeople = idEmployeePL and idEmployee = idTeamEmployee;

-- Recuperando os Funcionários por equipe e sua especialidade
select concat(Fname, ' ', Minit, ' ',Lname) as Colaborador, specialty as Especialidade, teamPosition as Posição from people, employee, team
	where idPeople = idEmployeePL and idEmployee = idTeamEmployee and teamPosition = 'TimeA';

-- recuperando um mecanico que tambem é cliente   
select concat(Fname, ' ', Minit, ' ',Lname) as 'Colaborador/Cliente' from people, clients, employee
	where idPeople = idClientPL and idPeople = idEmployeePL;



-- Recuperando os dados de uma OS
-- Cliente e dados da OS
select * from clients;
select * from serviceorder;


-- Recuperando dados da OS com equipe responsável e veículo
select idServiceOrder as 'Número OS', teamPOsition as Equipe, license_plate, kindServiceOrder as 'Tipo Serviço', 
	dateIn as 'Data entrada', dateOut as 'Data comclusão', StatusOS, descriptionOrder  as Descrição, clientAuthorization as 'Autorização cliente'
    from team, serviceorder, vehicle
	where idTeamOs = idTeam and idvehicle = idVehicleOS;







select * from parts;
select * from rateparts;
select * from services;
select * from rateservices;
select * from serviceorder;


-- Recuperando valores da Tabela Valores das peças
select idServiceOrder as 'Número OS', totalPartsAmount as 'Valores das Peças', teamPOsition as Equipe, license_plate, kindServiceOrder as 'Tipo Serviço', 
	dateIn as 'Data entrada', dateOut as 'Data comclusão', StatusOS, descriptionOrder  as Descrição, clientAuthorization as 'Autorização cliente'
    from team, serviceorder, vehicle, rateparts
	where idTeamOs = idTeam and idvehicle = idVehicleOS and idPartsOS = idServiceOrder;



-- Recuperando valores da Tabela Valores das peças com filtro de busca nos valores
select idServiceOrder as 'Número OS', totalPartsAmount as 'Valores das Peças', teamPOsition as Equipe, license_plate, kindServiceOrder as 'Tipo Serviço', 
	dateIn as 'Data entrada', dateOut as 'Data comclusão', StatusOS, descriptionOrder  as Descrição, clientAuthorization as 'Autorização cliente'
    from team, serviceorder, vehicle, rateparts
	where idTeamOs = idTeam and idvehicle = idVehicleOS and idPartsOS = idServiceOrder and totalPartsAmount > 1000;


-- Recuperando valores da Tabela Valores dos Serviços
select idServiceOrder as 'Número OS', totalService as 'Valores dos Serviços', teamPOsition as Equipe, license_plate, kindServiceOrder as 'Tipo Serviço', 
	dateIn as 'Data entrada', dateOut as 'Data comclusão', StatusOS, descriptionOrder  as Descrição, clientAuthorization as 'Autorização cliente'
    from team, serviceorder, vehicle, rateservices
	where idTeamOs = idTeam and idvehicle = idVehicleOS and idServiceOS = idServiceOrder;


-- Recuperando valores da Tabela Valores dos Serviços com filtro de busca nos valores
select idServiceOrder as 'Número OS', totalService as 'Valores dos Serviços', teamPOsition as Equipe, license_plate, kindServiceOrder as 'Tipo Serviço', 
	dateIn as 'Data entrada', dateOut as 'Data comclusão', StatusOS, descriptionOrder  as Descrição, clientAuthorization as 'Autorização cliente'
    from team, serviceorder, vehicle, rateservices
	where idTeamOs = idTeam and idvehicle = idVehicleOS and idServiceOS = idServiceOrder and totalService < 1000;







select * from parts;
select * from rateparts;
select * from services;
select * from rateservices;
select * from serviceorder;



-- Recuperando valores de 3 tabelas com inner join e operações matemáticas
-- partRate from parts
-- quantity, totalPartsAmount from rateparts
select distinct idServiceOrder, kindserviceOrder, dateIn, statusOs, part_description, quantity, totalPartsAmount, partRate, round((totalPartsAmount + partRate),2) as Valor_total
	from rateparts
	inner join serviceorder on idPartsOS = idServiceOrder
    inner join parts on idrateParts = idPart
    order by idServiceOrder, statusOs;



-- filtro com like
select distinct idServiceOrder, kindserviceOrder, dateIn, dateOut, statusOs, part_description, quantity, totalPartsAmount, partRate, round((totalPartsAmount + partRate),2) as Valor_total
	from rateparts
	inner join serviceorder on idPartsOS = idServiceOrder
    inner join parts on idrateParts = idPart
    where part_description like 'Kit%';




-- Contagem de itens, soma, maximo, minimo e media de valores
select idServiceOrder, kindserviceOrder, dateIn, statusOs, part_description, quantity, partRate,
	count(*) as contagem, round(sum(partRate),2) as Soma_valores, max(partRate) as valor_maximo, min(partRate) as valor_minimo,
    round(avg(partRate),2) as Valor_medio
	from rateparts
	inner join serviceorder on idPartsOS = idServiceOrder
    inner join parts on idrateParts = idPart
    order by idServiceOrder, statusOs;





-- Recuperando valores de 4 tabelas com inner join e operações matemáticas
-- serviceorder OS
-- cliets
-- vehicles
--  totalPartsAmount from rateparts
-- totalService from rateservice

select idServiceOrder, idVehicleOS, idclientOS, kindserviceOrder, dateIn, statusOs, totalPartsAmount, totalService
	from serviceorder
	inner join rateParts on idPartsOS = idServiceOrder
    inner join rateServices on idrateServices = idServiceOrder
    order by idServiceOrder;



-- Identificação da OS, ID Veículo, ID Cliente, Valor das peças e serviços com filtro no cliente
select idServiceOrder, idVehicleOS, idclientOS, kindserviceOrder, dateIn, statusOs, totalPartsAmount, totalService
	from serviceorder
	inner join rateParts on idPartsOS = idServiceOrder
    inner join rateServices on idrateServices = idServiceOrder
    where idclientOS = 4;
    
    
    
-- Identificação da OS, ID Veículo, ID Cliente, Valor das peças e serviços com filtro no veiculo
select idServiceOrder, idVehicleOS, idclientOS, kindserviceOrder, dateIn, statusOs, totalPartsAmount, totalService
	from serviceorder
	inner join rateParts on idPartsOS = idServiceOrder
    inner join rateServices on idrateServices = idServiceOrder
    where idVehicleOS = 11;
