# dbOficina
## Crição de um simples banco de dados para um oficina.


### Tabelas
* Cliente
* Veiculo
* OrdemServico
* Servico

### Esquema Lógico (Modelo Relacional)

CLIENTE(
  id_cliente PK,
  nome
)
  
VEICULO (
    id_veiculo PK,
    modelo,
    id_cliente FK → CLIENTE.id_cliente
)

SERVICO (
    id_servico PK,
    descricao,MONT
    valor
)

ORDEMSERVICO (
    id_os PK,
    data,
    id_veiculo FK → VEICULO.id_veiculo,
    id_servico FK → SERVICO.id_servico
)


### SCRIPT PARA CRIAÇÃO DAS TABELAS

CREATE TABLE Cliente (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);

CREATE TABLE Veiculo (
    id_veiculo INT PRIMARY KEY AUTO_INCREMENT,
    modelo VARCHAR(50),
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente)
);

CREATE TABLE Servico (
    id_servico INT PRIMARY KEY AUTO_INCREMENT,
    descricao VARCHAR(100),
    valor DECIMAL(10,2)
);

CREATE TABLE OrdemServico (
    id_os INT PRIMARY KEY AUTO_INCREMENT,
    data DATE,
    id_veiculo INT,
    id_servico INT,
    FOREIGN KEY (id_veiculo) REFERENCES Veiculo(id_veiculo),
    FOREIGN KEY (id_servico) REFERENCES Servico(id_servico)
);


### INSERÇÃO DE DADOS DE TESTE

INSERT INTO Cliente (nome) VALUES ('João'), ('Maria');

INSERT INTO Veiculo (modelo, id_cliente) VALUES
('Civic', 1),
('Gol', 2);

INSERT INTO Servico (descricao, valor) VALUES
('Troca de óleo', 100),
('Alinhamento', 150);

INSERT INTO OrdemServico (data, id_veiculo, id_servico) VALUES
('2026-03-01', 1, 1),
('2026-03-02', 2, 2),
('2026-03-03', 1, 2);



### COMANDOS

Select
------
SELECT * FROM Client;

Where
------
SELECT * FROM Servico
WHERE valor > 100;

Atributo derivado
------
SELECT descricao, valor, valor * 1.1 AS valor_com_taxa
FROM Servico;

Order by
------
SELECT * FROM OrdemServico
ORDER BY data DESC;

Having
-----
SELECT c.nome, COUNT(os.id_os) AS total
FROM Cliente c
JOIN Veiculo v ON c.id_cliente = v.id_cliente
JOIN OrdemServico os ON v.id_veiculo = os.id_veiculo
GROUP BY c.nome
HAVING COUNT(os.id_os) > 1;

Join
-----
SELECT c.nome, v.modelo, s.descricao, os.data
FROM OrdemServico os
JOIN Veiculo v ON os.id_veiculo = v.id_veiculo
JOIN Cliente c ON v.id_cliente = c.id_cliente
JOIN Servico s ON os.id_servico = s.id_servico;
