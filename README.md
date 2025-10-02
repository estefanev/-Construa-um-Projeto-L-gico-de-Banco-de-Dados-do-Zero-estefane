1. Criação do Script SQL do Esquema do Banco de Dados
   
-- Criação do banco
CREATE DATABASE Livraria;
USE Livraria;

-- Tabela de Clientes
CREATE TABLE Cliente (
    ClienteID INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    DataCadastro DATE DEFAULT CURRENT_DATE
);

-- Tabela de Autores
CREATE TABLE Autor (
    AutorID INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(100) NOT NULL
);

-- Tabela de Livros
CREATE TABLE Livro (
    LivroID INT PRIMARY KEY AUTO_INCREMENT,
    Titulo VARCHAR(150) NOT NULL,
    AutorID INT NOT NULL,
    Preco DECIMAL(10,2) NOT NULL,
    Estoque INT DEFAULT 0,
    FOREIGN KEY (AutorID) REFERENCES Autor(AutorID)
);

-- Tabela de Pedidos
CREATE TABLE Pedido (
    PedidoID INT PRIMARY KEY AUTO_INCREMENT,
    ClienteID INT NOT NULL,
    DataPedido DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (ClienteID) REFERENCES Cliente(ClienteID)
);

-- Tabela de Itens do Pedido
CREATE TABLE ItemPedido (
    ItemPedidoID INT PRIMARY KEY AUTO_INCREMENT,
    PedidoID INT NOT NULL,
    LivroID INT NOT NULL,
    Quantidade INT NOT NULL,
    PrecoUnitario DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (PedidoID) REFERENCES Pedido(PedidoID),
    FOREIGN KEY (LivroID) REFERENCES Livro(LivroID)
);

2. Persistência de Dados (Exemplo)
   
-- Clientes
INSERT INTO Cliente (Nome, Email) VALUES
('Ana Silva', 'ana@email.com'),
('Carlos Souza', 'carlos@email.com'),
('Beatriz Lima', 'beatriz@email.com');

-- Autores
INSERT INTO Autor (Nome) VALUES
('Machado de Assis'),
('Paulo Coelho'),
('Clarice Lispector');

-- Livros
INSERT INTO Livro (Titulo, AutorID, Preco, Estoque) VALUES
('Dom Casmurro', 1, 45.00, 10),
('O Alquimista', 2, 35.00, 15),
('A Hora da Estrela', 3, 40.00, 8);

-- Pedidos
INSERT INTO Pedido (ClienteID, DataPedido) VALUES
(1, '2025-10-01'),
(2, '2025-10-02');

-- Itens do Pedido
INSERT INTO ItemPedido (PedidoID, LivroID, Quantidade, PrecoUnitario) VALUES
(1, 1, 2, 45.00),
(1, 2, 1, 35.00),
(2, 3, 1, 40.00);

3. Queries SQL
   
3.1 Recuperações simples com SELECT Statement

-- Todos os clientes cadastrados
SELECT * FROM Cliente;

3.2 Filtros com WHERE Statement

-- Clientes cadastrados após 01/10/2025
SELECT Nome, Email, DataCadastro
FROM Cliente
WHERE DataCadastro > '2025-10-01';

3.3 Expressões para gerar atributos derivados

-- Total gasto por cada pedido
SELECT PedidoID, SUM(Quantidade * PrecoUnitario) AS TotalPedido
FROM ItemPedido
GROUP BY PedidoID;

3.4 Ordenações dos dados com ORDER BY

-- Livros mais caros primeiro
SELECT Titulo, Preco
FROM Livro
ORDER BY Preco DESC;

3.5 Condições de filtros aos grupos – HAVING Statement

-- Pedidos com valor total acima de 80
SELECT PedidoID, SUM(Quantidade * PrecoUnitario) AS TotalPedido
FROM ItemPedido
GROUP BY PedidoID
HAVING SUM(Quantidade * PrecoUnitario) > 80;

3.6 Junções entre tabelas

-- Lista de pedidos com nome do cliente e título do livro
SELECT p.PedidoID, c.Nome AS Cliente, l.Titulo AS Livro, i.Quantidade, i.PrecoUnitario
FROM Pedido p
JOIN Cliente c ON p.ClienteID = c.ClienteID
JOIN ItemPedido i ON p.PedidoID = i.PedidoID
JOIN Livro l ON i.LivroID = l.LivroID;

-- Total gasto por cada cliente
SELECT c.Nome, SUM(i.Quantidade * i.PrecoUnitario) AS TotalGasto
FROM Cliente c
JOIN Pedido p ON c.ClienteID = p.ClienteID
JOIN ItemPedido i ON p.PedidoID = i.PedidoID
GROUP BY c.Nome
ORDER BY TotalGasto DESC;

4. Perguntas que podem ser respondidas pelas consultas

Quais clientes estão cadastrados no sistema?

Quais clientes se cadastraram recentemente?

Qual é o total de cada pedido?

Quais são os livros mais caros?

Quais pedidos ultrapassaram R$ 80,00?

Quais clientes compraram quais livros e em qual quantidade?

Qual cliente gastou mais na livraria?
