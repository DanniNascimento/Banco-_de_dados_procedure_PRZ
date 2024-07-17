# Praticando SQL Curso Proz Banco de dados Simples...

Lembre-se, cada desafio é uma chance de crescer. Não se desanime com os erros; eles são degraus no caminho do aprendizado. E acima de tudo, divirta-se! 
O aprendizado mais eficaz acontece quando nos engajamos e nos interessamos pelo que estamos fazendo.

Vamos criar um procedimento armazenado (stored procedure) que gere um relatório diário da quantidade de produtos comprados por dia. Primeiro, criaremos as tabelas necessárias para armazenar 
as informações dos produtos e das compras. Depois, criaremos a procedure para gerar o relatório...

#### Criando uma base de banco de dados
```sql
CREATE DATABASE EmpresaVendas;

```

#### Criando tabelas na base de dados;
```sql
CREATE TABLE Produtos (
    ProdutoID INT PRIMARY KEY AUTO_INCREMENT,
    NomeProduto VARCHAR(100),
    Preco DECIMAL(10, 2)
);

```
#### Criando tabela (produtos)
```sql
CREATE TABLE Produtos (
    ProdutoID INT PRIMARY KEY AUTO_INCREMENT,
    NomeProduto VARCHAR(100),
    Preco DECIMAL(10, 2)
);

```

#### Criando tabela (compras)
```sql
CREATE TABLE Compras (
    CompraID INT PRIMARY KEY AUTO_INCREMENT,
    ProdutoID INT,
    Quantidade INT,
    DataCompra DATE,
    FOREIGN KEY (ProdutoID) REFERENCES Produtos(ProdutoID)
);
```

#### Vamos Inserir dados nas tabelas (produtos)
```sql
INSERT INTO Produtos (NomeProduto, Preco) VALUES
('Notebook', 3000.00),
('Mouse', 50.00),
('Teclado', 100.00);

```

#### Inserindo dados na tabela (compras)
```sql
INSERT INTO Compras (ProdutoID, Quantidade, DataCompra) VALUES
(1, 2, '2024-07-14'),
(2, 5, '2024-07-14'),
(3, 3, '2024-07-15'),
(1, 1, '2024-07-15'),
(2, 2, '2024-07-15');

```

#### Criando a procedure para gerar o relatorio diario
```sql
DELIMITER //

CREATE PROCEDURE GerarRelatorioDiario()
BEGIN
    SELECT DataCompra, ProdutoID, SUM(Quantidade) AS TotalQuantidade
    FROM Compras
    GROUP BY DataCompra, ProdutoID
    ORDER BY DataCompra, ProdutoID;
END //

DELIMITER ;
```

#### Vamos executar a procedure para gerar o relarotorio diario
```sql
CALL GerarRelatorioDiario();

```

#### Resultado

Ao executarmos a procedure GerarRelatorioDiario ela irar gerar um relatório com a quantidade total de produtos comprados por dia, agrupados por data e por produto.

```sql
+------------+-----------+----------------+
| DataCompra | ProdutoID | TotalQuantidade|
+------------+-----------+----------------+
| 2024-07-14 |         1 |              2 |
| 2024-07-14 |         2 |              5 |
| 2024-07-15 |         1 |              1 |
| 2024-07-15 |         2 |              2 |
| 2024-07-15 |         3 |              3 |
+------------+-----------+----------------+

```
