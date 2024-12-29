# Projeto de Banco de Dados para E-commerce

Este projeto tem como objetivo a modelagem e implementação de um banco de dados para um sistema de **e-commerce**, abrangendo as funcionalidades de clientes, pedidos, pagamentos, entregas, produtos e fornecedores. A modelagem lógica foi refinada com base em um modelo conceitual e inclui relacionamentos entre as entidades, como a possibilidade de um cliente ser pessoa física (PF) ou jurídica (PJ), múltiplas formas de pagamento e o acompanhamento de entregas por meio de status e código de rastreio.

## Descrição do Projeto Lógico

O esquema lógico do banco de dados foi projetado para atender às necessidades de um sistema de e-commerce, com as seguintes principais entidades:

### Tabelas:

1. **Cliente**: Contém informações dos clientes, tanto pessoa física quanto jurídica.
2. **Pagamento**: Armazena as formas de pagamento de pedidos, permitindo múltiplas opções de pagamento.
3. **Entrega**: Armazena informações sobre a entrega de pedidos, incluindo status e código de rastreio.
4. **Pedido**: Registra os pedidos feitos pelos clientes.
5. **Produto**: Contém os produtos disponíveis no estoque.
6. **Fornecedor**: Armazena dados dos fornecedores dos produtos.
7. **Vendedor**: Relaciona vendedores que também são fornecedores.

### Relacionamentos:

- **Cliente** pode ser Pessoa Física (PF) ou Pessoa Jurídica (PJ).
- **Pedido** pode ter múltiplos **Pagamentos** associados.
- **Pedido** pode ter múltiplas **Entregas** associadas, cada uma com um status e código de rastreio.
- **Produto** pode ser fornecido por vários **Fornecedores**.
- **Vendedor** pode ser também um **Fornecedor**.

### Refinamentos no Modelo:

- Um cliente pode ser uma pessoa física (PF) ou jurídica (PJ), mas não ambos.
- Um **Pagamento** pode ter mais de uma forma de pagamento registrada para um mesmo pedido.
- **Entrega** tem status (Em Processamento, Enviado, Entregue) e código de rastreio.

## Estrutura do Banco de Dados

![e-commerce](https://github.com/user-attachments/assets/9854e5ea-037d-4d6a-b2c1-c56153f31c00)

# Consultas SQL

## 1. Quantos pedidos foram feitos por cada cliente?

SELECT cliente_id, COUNT(pedido_id) AS total_pedidos
FROM Pedido
GROUP BY cliente_id;

## 2. Algum vendedor também é fornecedor?

SELECT v.nome AS vendedor, f.nome AS fornecedor
FROM Vendedor v
JOIN Fornecedor f ON v.fornecedor_id = f.fornecedor_id;

## 3. Relação de produtos, fornecedores e estoques:

SELECT p.nome AS produto, f.nome AS fornecedor, p.estoque
FROM Produto p
JOIN Produto_Fornecedor pf ON p.produto_id = pf.produto_id
JOIN Fornecedor f ON pf.fornecedor_id = f.fornecedor_id;

## 4. Relação de nomes dos fornecedores e nomes dos produtos:

SELECT f.nome AS fornecedor, p.nome AS produto
FROM Produto p
JOIN Produto_Fornecedor pf ON p.produto_id = pf.produto_id
JOIN Fornecedor f ON pf.fornecedor_id = f.fornecedor_id;

## 5. Produtos com preço acima de 100:

SELECT nome, preco
FROM Produto
WHERE preco > 100;

## 6. Junção entre clientes, pedidos e pagamentos:

SELECT c.nome AS cliente, p.data_pedido, pg.tipo_pagamento, pg.valor
FROM Cliente c
JOIN Pedido p ON c.cliente_id = p.cliente_id
JOIN Pagamento pg ON p.pedido_id = pg.pedido_id;

## 7. Clientes com mais de 5 pedidos:

SELECT cliente_id, COUNT(pedido_id) AS total_pedidos
FROM Pedido
GROUP BY cliente_id
HAVING COUNT(pedido_id) > 5;
