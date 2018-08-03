# Banco_de_Dados_AutomatHouse
Trabalho do 3° estágio de informatica industrial
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
