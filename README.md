
#               Projeto de Banco de Dados  
#     
#     
#       Alunos: Raphael Santana Galdino  
#               Samuel de Melo Barros  
#  
# ////////////////////////////////////////////////////  
    
########################################################  
#------------ Criação do Banco de Dados ----------------  
########################################################  


# Banco_de_Dados_AutomatHouse
# Trabalho do 3° estágio de informatica industrial

########################################################  
#------------ Criação de Tabelas ----------------  
########################################################  

CREATE TABLE Usuario (  
    ID int(32) NOT NULL AUTO_INCREMENT,
    Nome varchar(30) NOT NULL,  
    Email varchar(50) NOT NULL UNIQUE,  
    CPF varchar(11) NOT NULL UNIQUE,  
    RFIDcode varchar(50) UNIQUE,  
    Senha varchar(50) NOT NULL,  
    ID_Endereco int(30) NOT NULL,   
    ID_Admin int(32) ,  
    PRIMARY KEY (ID,CPF)  
);  
  
CREATE TABLE Endereco (  
    ID int(32) NOT NULL AUTO_INCREMENT,  
    Rua varchar(50) NOT NULL,  
    Bairro varchar(50) NOT NULL,  
    Cidade varchar(50) NOT NULL,  
    Numero varchar(50) NOT NULL,  
    CEP int(10) NOT NULL, 
    Estado varchar(50) NOT NULL,
    Pais varchar(50) NOT NULL,
    PRIMARY KEY (ID)  
);  
  
CREATE TABLE Telefone_Usuarios (  
    ID_Usuario int(32) NOT NULL,  
    Telefone varchar(15),  
    PRIMARY KEY (ID_Usuario,Telefone)  
);  
  
CREATE TABLE Localidade( 
    ID int(32) NOT NULL AUTO_INCREMENT,  
    Nome varchar(30) NOT NULL,  
    Tempo_min int(32) NOT NULL,  
    ID_Equipamento int(32) NOT NULL,  
    PRIMARY KEY (ID,Nome)  
);  
  
CREATE TABLE Equipamento(  
    ID int(32) NOT NULL AUTO_INCREMENT,  
    Codigo varchar(50) NOT NULL,  
    DataFabricacao DATETIME(6) NOT NULL,  
    Cor varchar(30) NOT NULL DEFAULT 'Verde',
    Tipo enum('Iluminacao', 'Televisao','SomEstereo', 'Despertador','Porta', 'Arcondicionado'),
 
    PRIMARY KEY (ID,Codigo)  
);  
  
CREATE TABLE Solicitacoes (  
    ID int(32) NOT NULL AUTO_INCREMENT,   
    ID_Equipamento int(32) NOT NULL,  
    ID_Usuario int(32) NOT NULL,  
    Horario DATETIME(6) NOT NULL,  
    StatusAcesso enum('Permitido','Negado') NOT NULL,  
    PRIMARY KEY (ID)  
);  
  
CREATE TABLE Acessos (  
    ID_Usuario int(32) NOT NULL,  
    ID_Local int(32) NOT NULL,  
    ID_Horarios int(32) NOT NULL,  
    PRIMARY KEY (ID_Usuario,ID_Local,ID_Horarios)  
);  
  
CREATE TABLE Horarios (
    ID int(32) NOT NULL AUTO_INCREMENT,  
    PRIMARY KEY (ID)  
);  
  
CREATE TABLE Dia_Hora (  
    ID_Horarios int(32) NOT NULL,  
    Dia int(2) NOT NULL,  
    Hora int(4) NOT NULL,  
    PRIMARY KEY (ID_Horarios,Dia,Hora)  
);  

########################################################  
#--------- Adicionando as chaves estrangeiras ---------  
########################################################  

ALTER TABLE Usuario ADD CONSTRAINT Usuario_fk0 FOREIGN KEY (ID_Endereco) REFERENCES Endereco(ID);  
  
ALTER TABLE Usuario ADD CONSTRAINT Usuario_fk1 FOREIGN KEY (ID_Admin) REFERENCES Usuario(ID);  
  
ALTER TABLE Telefone_Usuarios ADD CONSTRAINT Telefone_Usuarios_fk0 FOREIGN KEY (ID_Usuario) REFERENCES Usuario(ID);  
  
ALTER TABLE Localidade ADD CONSTRAINT Local_fk0 FOREIGN KEY (ID_Equipamento) REFERENCES Equipamento(ID);  
  
ALTER TABLE Solicitacoes ADD CONSTRAINT Solicitacoes_fk0 FOREIGN KEY (ID_Usuario) REFERENCES Usuario(ID);  
  
ALTER TABLE Solicitacoes ADD CONSTRAINT Solicitacoes_fk1 FOREIGN KEY (ID_Equipamento) REFERENCES Equipamento(ID);  
  
ALTER TABLE Acessos ADD CONSTRAINT Acessos_fk0 FOREIGN KEY (ID_Usuario) REFERENCES Usuario(ID);  
  
ALTER TABLE Acessos ADD CONSTRAINT Acessos_fk1 FOREIGN KEY (ID_Local) REFERENCES Localidade(ID);  
  
ALTER TABLE Acessos ADD CONSTRAINT Acessos_fk2 FOREIGN KEY (ID_Horarios) REFERENCES Horarios(ID);  
  
ALTER TABLE Dia_Hora ADD CONSTRAINT Dia_Hora_fk0 FOREIGN KEY (ID_Horarios) REFERENCES Horarios(ID);  

########################################################  
#----------------- Inserção de Usuarios ----------------  
########################################################  

START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
 VALUES ('Rua de Samuel','Floriano Peixoto','Campina Grande', '111','58261398','Paraiba','Brasil');      
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco)   
        VALUES ('Samuel Melo','samuel.barros@ee.ufcg.edu.br','115210484','me da 10 pf', @a);  
COMMIT;  

update usuario
set ID_Admin = 1
where nome = 'Samuel Melo';

  
START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
 VALUES ('Rua de Raphael','Residence Prive','Campina Grande', '222','58567902','Paraiba','Brasil');   
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco, ID_Admin)   
   VALUES ('Raphael Santana', 'raphael.galdino@ee.ufcg.edu.br', '11321099999', 'whitepeople', @a, 1);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
 VALUES ('Rua de Julio','Cabo Branco','João Pessoa', '333','58261398','Paraiba','Brasil');     
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco, ID_Admin)   
        VALUES ('Julio Cesar', 'julio.cesar@ee.ufcg.edu.com.br', '11311099999', 'jiboia', @a, 1);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
 VALUES ('Rua HP','Bairro HP','Hogwarts', '234', '34576123','California','EUA');     
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco, ID_Admin)   
        VALUES ('Harry Potter','harry_trouxa@coruja.com', '11111111111', 'voldemort', @a, 1);  
COMMIT;   
 
START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
        VALUES ('Rua SBT','Bairro SBT','SBT','100','58776345','São Paulo','Brasil');      
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco,ID_Admin)   
      VALUES ('Silvio Santos','silvio.santos@sbt.com','12345678901','jequiti', @a,1);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Cidade,Numero,CEP,Estado,Pais)   
  VALUES ('Rua Fumo','Bairro Maquinas','Edgarlândia','666', '58261398','Paraiba','Brasil');    
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco, ID_Admin)   
        VALUES ('Edgar','edgar@fuminho.com','66666666666','socorro_deus', @a, 1);  
COMMIT;   

START TRANSACTION;  
    INSERT INTO Endereco(Rua,Bairro,Numero,Cidade,CEP,Estado,Pais)   
        VALUES ('Rua Pedro Soares','Catole', '777','Campina Grande', '58414525',’Paraiba’,’Brasil’);     
    SELECT @a:=ID FROM Endereco ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Usuario(Nome,Email,CPF,Senha,ID_Endereco, ID_Admin)   
    VALUES ('Joana','Joana.eletrica@ee.ufcg.edu.br','64782938275','reprovada', @a, 0),     
              
COMMIT;   
  
  
  
########################################################  
#------------ Inserção de Modulos e Locais--------------  
########################################################  
  
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo, Cor, DataFabricacao, Tipo)   
        VALUES ('0000', 'Cinza','2015-05-21', 'Despertador');    
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Quarto','5', @a);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo,DataFabricacao,Tipo)   
        VALUES ('1111','2020-10-02','Porta');     
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Banheiro','20', @a);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo,Cor,DataFabricacao,Tipo)   
        VALUES ('2222','Branco','2014-05-12','iluminacao');    
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Cozinha','120', @a);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo,DataFabricacao,Tipo)   
        VALUES ('3333','2012-12-12','Televisao');     
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Sala de Estar','50', @a);  
COMMIT;   
  
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo,Cor,DataFabricacao,Tipo)   
        VALUES ('4444','Ciano','2010-05-19','Arcondicionado');     
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Quarto','300', @a);  
COMMIT;   
    
START TRANSACTION;  
    INSERT INTO Equipamento(Codigo,Cor,DataFabricacao,Tipo)   
        VALUES ('5555','Branco','2008-11-20','SomEstereo');     
    SELECT @a:=ID FROM Equipamento ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Localidade(Nome, Tempo_min, ID_Equipamento)   
        VALUES ('Terraço','60', @a);  
COMMIT; 
  
  
########################################################  
#------------ Inserção de Horários ----------------  
########################################################  
START TRANSACTION;  
    INSERT INTO Horarios  
        VALUES();  
    SELECT @horarioID:=ID FROM Horarios ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Dia_Hora(Dia,Hora,ID_Horarios)  
        VALUES  ('1','3',@horarioID),               
                ('2','6',@horarioID),                          ,             
                ('3','3',@horarioID),                         
                        
COMMIT;  
START TRANSACTION;  
    INSERT INTO Horarios  
        VALUES();  
    SELECT @horarioID:=ID FROM Horarios ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Dia_Hora(Dia,Hora,ID_Horarios)  
        VALUES  ('1','8',@horarioID),            
                ('2','1',@horarioID),            
                ('3','2',@horarioID),            
              
COMMIT;  
 START TRANSACTION;  
    INSERT INTO Horarios  
        VALUES();  
    SELECT @horarioID:=ID FROM Horarios ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Dia_Hora(Dia,Hora,ID_Horarios)  
        VALUES  ('1','4',@horarioID),            
                ('2','9',@horarioID),            
                ('3','2',@horarioID),            
              
COMMIT;  
START TRANSACTION;  
    INSERT INTO Horarios  
        VALUES();  
    SELECT @horarioID:=ID FROM Horarios ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Dia_Hora(Dia,Hora,ID_Horarios)  
        VALUES  ('1','6',@horarioID),            
                ('2','3',@horarioID),            
                ('3','3',@horarioID),            
              
COMMIT;  
START TRANSACTION;  
    INSERT INTO Horarios  
        VALUES();  
    SELECT @horarioID:=ID FROM Horarios ORDER BY ID DESC LIMIT 1;      
    INSERT INTO Dia_Hora(Dia,Hora,ID_Horarios)  
        VALUES  ('1','1',@horarioID),            
                ('2','1',@horarioID),            
                ('3','0',@horarioID),            
              
COMMIT;  
  
########################################################  
#------------ Inserção de Acessos ----------------  
########################################################  
START TRANSACTION;  
    INSERT INTO Acessos(ID_Usuario,ID_Local,ID_Horarios)  
        VALUES  ('1','1','3'),            
                ('2','1','2'),            
                ('3','5','2'),             
                ('4','5','3'),            
                ('5','3','5'),            
                ('6','3','5');                    
COMMIT;  
  
  
########################################################  
#------------ Inserção de Solicitacoes ----------------  
########################################################  
START TRANSACTION;    
    INSERT INTO Solicitacoes(ID_Usuario,ID_Equipamento,Horario,StatusAcesso)  
        VALUES  ('1','4','2017-12-18 08:00:00','Permitido'),  
                ('1','1','2017-12-18 15:00:00','Permitido'),  
                ('3','5','2017-12-18 10:00:00','Negado'),  
                ('1','3','2017-12-19 08:00:00','Permitido'),  
                ('4','5','2017-12-20 10:00:00','Negado'),  
                ('2','3','2017-12-21 08:00:28','Permitido'),  
                ('1','2','2017-12-21 10:00:00','Permitido'),  
                ('3','5','2017-12-22 09:00:00','Negado'),  
                ('6','3','2017-12-22 10:00:00','Negado'),  
                ('4','5','2017-12-22 11:00:00','Negado'),  
                ('2','1','2017-12-22 13:00:00','Permitido');  
COMMIT; 


 
#               Projeto de Banco de Dados  
#     
#     
#       Alunos: Raphael Santana Galdino  
#               Samuel de Melo Barros 
#  
# ////////////////////////////////////////////////////  
 
# -- CONSULTA 1 --  
  
# Retorne a lista de ID de equipamentos por nome e ID de sala.  
 
SELECT E.Codigo,E.Tipo, L.Nome   
FROM Localidade as L  
INNER JOIN Equipamento as E   
ON L.ID_Equipamento = E.ID;  
  
# -- CONSULTA 2 --  
  
# Retorne a lista de usuários que usufruiram da Sala X (defina um nome X).  
 
SELECT users.Nome AS Usuario, locale.Nome,   
    DATE_FORMAT(s.Horario, "%d-%m-%Y as %Hh:%imin") AS Data_de_Acesso  
FROM Solicitacoes AS s  
INNER JOIN Localidade as locale  
ON s.ID_Equipamento = locale.ID_Equipamento   
INNER JOIN Usuario AS users  
ON users.ID = s.ID_Usuario  
     AND s.statusAcesso = 'Permitido'    
     AND locale.Nome = 'Quarto';  
  ORDER BY s.Horario DESC;  
 
# -- CONSULTA 3 --  
  
# Retorne a lista de usuários que moram na cidade de Campina Grande.   
 
SELECT Count(*) AS Usuarios_Campina_Grande  
FROM Usuario AS users  
INNER JOIN Endereco AS addrs   
ON users.ID_Endereco = addrs.ID AND addrs.Cidade = "Campina Grande";  

=======


  

