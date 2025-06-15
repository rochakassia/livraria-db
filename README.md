# livraria-db
# Modelagem de Dados (DER Simplificado)
Clientes (id_cliente, nome, email)
Livros (id_livro, titulo, autor, preco, estoque)
Pedidos (id_pedido, id_cliente, data_pedido)
ItensPedido (id_pedido, id_livro, quantidade, preco_unitario)
# Relacionamentos:
#Um cliente pode fazer vários pedidos.
#Um pedido pode conter vários livros.
#Um livro pode estar em vários pedidos.

#SQL: Criação das Tabelas
CREATE TABLE clientes (
    id_cliente INTEGER PRIMARY KEY,
    nome TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);

CREATE TABLE livros (
    id_livro INTEGER PRIMARY KEY,
    titulo TEXT NOT NULL,
    autor TEXT NOT NULL,
    preco REAL NOT NULL,
    estoque INTEGER NOT NULL
);

CREATE TABLE pedidos (
    id_pedido INTEGER PRIMARY KEY,
    id_cliente INTEGER,
    data_pedido DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente)
);

CREATE TABLE itens_pedido (
    id_pedido INTEGER,
    id_livro INTEGER,
    quantidade INTEGER NOT NULL,
    preco_unitario REAL NOT NULL,
    PRIMARY KEY (id_pedido, id_livro),
    FOREIGN KEY (id_pedido) REFERENCES pedidos(id_pedido),
    FOREIGN KEY (id_livro) REFERENCES livros(id_livro)
);

#SQL: Manipulação de Dados
INSERT INTO clientes VALUES (1, 'João Silva', 'joao@mail.com');
INSERT INTO livros VALUES (1, 'Dom Quixote', 'Cervantes', 15.00, 30);

INSERT INTO pedidos (id_pedido, id_cliente) VALUES (1, 1);
INSERT INTO itens_pedido VALUES (1, 1, 2, 15.00);
#Atualização
UPDATE livros SET estoque = estoque - 2 WHERE id_livro = 1;
#Remoção
DELETE FROM clientes WHERE id_cliente = 1;
#Consulta
SELECT c.nome, p.data_pedido, l.titulo, i.quantidade
FROM pedidos p
JOIN clientes c ON p.id_cliente = c.id_cliente
JOIN itens_pedido i ON p.id_pedido = i.id_pedido
JOIN livros l ON i.id_livro = l.id_livro;
