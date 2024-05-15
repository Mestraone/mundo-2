Primeira etapa: DBDesigner 
Diagrama ER (Entidade-Relacionamento) que representa o modelo de dados:
===============================================================
-- Tabela de Usuários
CREATE TABLE Usuarios (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 email VARCHAR(100),
 senha VARCHAR(100)
);
-- Tabela de Pessoas
CREATE TABLE Pessoas (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 tipo ENUM('Física', 'Jurídica'),
 cpf VARCHAR(14),
 cnpj VARCHAR(18),
 endereco VARCHAR(255),
 telefone VARCHAR(20)
);
-- Tabela de Produtos
CREATE TABLE Produtos (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 quantidade INT,
 preco DECIMAL(10, 2)
);
-- Tabela de Compras
CREATE TABLE Compras (
 id INT PRIMARY KEY,
 id_operador INT,
 id_fornecedor INT,
 id_produto INT,
 quantidade INT,
 preco_unitario DECIMAL(10, 2),
 FOREIGN KEY (id_operador) REFERENCES Usuarios(id),
 FOREIGN KEY (id_fornecedor) REFERENCES Pessoas(id),
 FOREIGN KEY (id_produto) REFERENCES Produtos(id)
);
-- Tabela de Vendas
CREATE TABLE Vendas (
 id INT PRIMARY KEY,
 id_operador INT,
 id_cliente INT,
 id_produto INT,
 quantidade INT,
 preco_unitario DECIMAL(10, 2),
 FOREIGN KEY (id_operador) REFERENCES Usuarios(id),
 FOREIGN KEY (id_cliente) REFERENCES Pessoas(id),
 FOREIGN KEY (id_produto) REFERENCES Produtos(id)


Segunda etapa: SQL Server Management Studio. 
1) Utilização do editor de SQL para a criação das estruturas do modelo 
-- Criar a tabela de Usuários
CREATE TABLE Usuarios (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 email VARCHAR(100),
 senha VARCHAR(100)
);
-- Criar a tabela de Pessoas
CREATE TABLE Pessoas (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 tipo VARCHAR(20),
 cpf VARCHAR(14),
 cnpj VARCHAR(18),
 endereco VARCHAR(255),
 telefone VARCHAR(20)
);
-- Criar a tabela de Produtos
CREATE TABLE Produtos (
 id INT PRIMARY KEY,
 nome VARCHAR(100),
 quantidade INT,
 preco DECIMAL(10, 2)
);
-- Criar a tabela de Compras
CREATE TABLE Compras (
 id INT PRIMARY KEY,
 id_operador INT,
 id_fornecedor INT,
 id_produto INT,
 quantidade INT,
 preco_unitario DECIMAL(10, 2),
 FOREIGN KEY (id_operador) REFERENCES Usuarios(id),
 FOREIGN KEY (id_fornecedor) REFERENCES Pessoas(id),
 FOREIGN KEY (id_produto) REFERENCES Produtos(id)
);
-- Criar a tabela de Vendas
CREATE TABLE Vendas (
 id INT PRIMARY KEY,
 id_operador INT,
 id_cliente INT,
 id_produto INT,
 quantidade INT,
 preco_unitario DECIMAL(10, 2),
 FOREIGN KEY (id_operador) REFERENCES Usuarios(id),
 FOREIGN KEY (id_cliente) REFERENCES Pessoas(id),
 FOREIGN KEY (id_produto) REFERENCES Produtos(id)
);


INSERT INTO Produtos (nome, quantidade, preco)
VALUES ('Produto A', 100, 10.99),
 ('Produto B', 50, 20.50),
 ('Produto C', 200, 5.75);
```
=============================================
2) Criar pessoas físicas e jurídicas na base de dados:
```sql
-- Obter o próximo id de pessoa a partir da sequence
DECLARE @proxId INT;
SET @proxId = NEXT VALUE FOR PessoaSequence;
-- Incluir na tabela pessoa os dados comuns para pessoa física
INSERT INTO Pessoas (id, nome, tipo, endereco, telefone)
VALUES (@proxId, 'João da Silva', 'Física', 'Rua A, 123', 
'12345678901');
-- Incluir em pessoa física o CPF, efetuando o relacionamento 
com pessoa
INSERT INTO PessoasFisicas (id, cpf)
VALUES (@proxId, '123.456.789-01');
```
```sql
-- Obter o próximo id de pessoa a partir da sequence
DECLARE @proxId INT;
SET @proxId = NEXT VALUE FOR PessoaSequence;
-- Incluir na tabela pessoa os dados comuns para pessoa jurídica
INSERT INTO Pessoas (id, nome, tipo, endereco, telefone)
VALUES (@proxId, 'Empresa XYZ', 'Jurídica', 'Av. Principal, 
456', '98765432109876');
-- Incluir em pessoa jurídica o CNPJ, relacionando com pessoa
INSERT INTO PessoasJuridicas (id, cnpj)
VALUES (@proxId, '98.765.432/1098-76');
```
=============================================
3) Criar algumas movimentações na base de dados:
```sql
-- Exemplo de movimentação de entrada (Compra)
INSERT INTO Compras (id_operador, id_fornecedor, id_produto, 
quantidade, preco_unitario)
VALUES (1, 2, 1, 50, 8.99); -- Operador 1 (op1) faz uma compra 
de 50 unidades do Produto A da Empresa XYZ
-- Exemplo de movimentação de saída (Venda)
INSERT INTO Vendas (id_operador, id_cliente, id_produto, 
quantidade, preco_unitario)
VALUES (2, 3, 2, 20, 25.99); -- Operador 2 (op2) realiza uma 
venda de 20 unidades do Produto B para João da Silva
```
=============================================
4) Efetuar as consultas solicitadas sobre os dados inseridos:
=============================================
```sql
-- 6) Dados completos de pessoas físicas
SELECT * FROM Pessoas WHERE tipo = 'Física';
-- 7) Dados completos de pessoas jurídicas
SELECT * FROM Pessoas WHERE tipo = 'Jurídica';
-- 8) Movimentações de entrada
SELECT p.nome AS Produto, pj.nome AS Fornecedor, c.quantidade, 
c.preco_unitario, (c.quantidade * c.preco_unitario) AS Total
FROM Compras c
JOIN Produtos p ON c.id_produto = p.id
JOIN Pessoas pj ON c.id_fornecedor = pj.id;
-- 9) Movimentações de saída
SELECT p.nome AS Produto, pf.nome AS Comprador, v.quantidade, 
v.preco_unitario, (v.quantidade * v.preco_unitario) AS Total
FROM Vendas v
JOIN Produtos p ON v.id_produto = p.id
JOIN Pessoas pf ON v.id_cliente = pf.id;
-- 10) Valor total das entradas agrupadas por produto
SELECT p.nome AS Produto, SUM(c.quantidade * c.preco_unitario) 
AS TotalEntradas
FROM Compras c
JOIN Produtos p ON c.id_produto = p.id
GROUP BY p.nome;
-- 11) Valor total das saídas agrupadas por produto
SELECT p.nome AS Produto, SUM(v.quantidade * v.preco_unitario) 
AS TotalSaidas
FROM Vendas v
JOIN Produtos p ON v.id_produto = p.id
GROUP BY p.nome;
-- 12) Operadores que não efetuaram movimentações de entrada 
(compra)
SELECT u.nome AS Operador
FROM Usuarios u
LEFT JOIN Compras c ON u.id = c.id_operador
WHERE c.id_operador IS NULL;
-- 13) Valor total de entrada, agrupado por operador
SELECT u.nome AS Operador, SUM(c.quantidade * c.preco_unitario) 
AS TotalEntradas
FROM Compras c
JOIN Usuarios u ON c.id_operador = u.id
GROUP BY u.nome;
-- 14) Valor total de saída, agrupado por operador
SELECT u.nome AS Operador, SUM(v.quantidade * v.preco_unitario) 
AS TotalSaidas
FROM Vendas v
JOIN Usuarios u ON v.id_operador = u.id
GROUP BY u.nome;
-- 15) Valor médio de venda por produto, utilizando média 
ponderada
SELECT p.nome AS Produto, SUM(v.quantidade * v.preco_unitario) / 
SUM(v.quantidade) AS MediaVenda
FROM Vendas v
JOIN Produtos p ON v.id_produto = p.id
GROUP BY p.nome;
```
=============================================
