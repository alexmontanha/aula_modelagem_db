# Atividade B – Modelagem de Banco de Dados (ER) e SQL no DB Fiddle

**Objetivo**
Implementar o modelo relacional definido no Diagrama ER do sistema “CryptoWallet Pro” criando as tabelas e populando-as com dados de exemplo via DDL e DML em um ambiente como o DB Fiddle (MySQL 8.0).

---

## 1. DDL – Criação das tabelas

```sql
-- 1. Tabela Usuário
CREATE TABLE Usuario (
  usuario_id   INT AUTO_INCREMENT PRIMARY KEY,
  nome         VARCHAR(100) NOT NULL,
  email        VARCHAR(150) NOT NULL UNIQUE,
  criado_em    DATETIME    NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- 2. Tabela Carteira
CREATE TABLE Carteira (
  carteira_id  INT AUTO_INCREMENT PRIMARY KEY,
  usuario_id   INT NOT NULL,
  nome         VARCHAR(100) NOT NULL,
  saldo        DECIMAL(18,8) NOT NULL DEFAULT 0.0,
  criado_em    DATETIME       NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (usuario_id) REFERENCES Usuario(usuario_id)
);

-- 3. Tabela Criptomoeda
CREATE TABLE Criptomoeda (
  cripto_id    INT AUTO_INCREMENT PRIMARY KEY,
  nome         VARCHAR(50)  NOT NULL,
  simbolo      VARCHAR(10)  NOT NULL UNIQUE
);

-- 4. Tabela Transação
CREATE TABLE Transacao (
  transacao_id    INT AUTO_INCREMENT PRIMARY KEY,
  carteira_id     INT NOT NULL,
  cripto_id       INT NOT NULL,
  quantidade      DECIMAL(18,8) NOT NULL,
  valor_total     DECIMAL(18,2) NOT NULL,  -- em moeda local
  data_transacao DATETIME      NOT NULL,
  FOREIGN KEY (carteira_id) REFERENCES Carteira(carteira_id),
  FOREIGN KEY (cripto_id)   REFERENCES Criptomoeda(cripto_id)
);
```

---

## 2. DML – Inserção de dados de exemplo

```sql
-- 1. Usuários
INSERT INTO Usuario (nome, email) VALUES
  ('Ana Silva',    'ana.silva@example.com'),
  ('Bruno Costa',  'bruno.costa@example.com'),
  ('Carla Pereira','carla.pereira@example.com');

-- 2. Carteiras
INSERT INTO Carteira (usuario_id, nome, saldo) VALUES
  (1, 'Carteira Pessoal',  0.0),
  (1, 'Carteira de Trading', 0.0),
  (2, 'Carteira Principal', 0.0),
  (3, 'Carteira Reserva',   0.0);

-- 3. Criptomoedas
INSERT INTO Criptomoeda (nome, simbolo) VALUES
  ('Bitcoin',  'BTC'),
  ('Ethereum', 'ETH'),
  ('Ripple',   'XRP');

-- 4. Transações
INSERT INTO Transacao (carteira_id, cripto_id, quantidade, valor_total, data_transacao) VALUES
  (1, 1, 0.02500000,  1250.00, '2025-05-20 09:15:00'),
  (1, 2, 0.10000000,   300.00, '2025-05-21 14:30:00'),
  (2, 1, 0.01000000,   500.00, '2025-05-22 11:45:00'),
  (3, 3, 50.00000000,  750.00, '2025-05-23 16:00:00'),
  (4, 2, 0.05000000,   150.00, '2025-05-24 10:20:00');
```

---

## 3. Consultas de Verificação (exemplos)

```sql
-- a) Listar todas as transações de Ana Silva (usuário_id = 1), mostrando data e valor:
SELECT t.data_transacao, c.simbolo, t.quantidade, t.valor_total
FROM Transacao AS t
JOIN Carteira   AS w ON t.carteira_id = w.carteira_id
JOIN Usuario    AS u ON w.usuario_id    = u.usuario_id
JOIN Criptomoeda AS c ON t.cripto_id     = c.cripto_id
WHERE u.usuario_id = 1
ORDER BY t.data_transacao;

-- b) Total investido por carteira:
SELECT w.nome AS carteira, SUM(t.valor_total) AS total_investido
FROM Transacao AS t
JOIN Carteira   AS w ON t.carteira_id = w.carteira_id
GROUP BY w.carteira_id;

-- c) Quantidade total de cada criptomoeda em todas as carteiras:
SELECT c.simbolo, SUM(t.quantidade) AS total_quantidade
FROM Transacao   AS t
JOIN Criptomoeda AS c ON t.cripto_id = c.cripto_id
GROUP BY c.cripto_id;
```

---

## 4. Instruções de Entrega

1. **Link do DB Fiddle** com o script completo (DDL + DML + Queries).
2. **Diagrama ER** atualizado (PDF ou PNG).
3. **README.md** explicando:

   * Significado de cada tabela e coluna
   * Como reproduzir o ambiente no DB Fiddle
   * Exemplos de resultado das consultas

Nomeie sua pasta de entrega:

``` plaintext
A3_BancoDados_SQL_<SeuNomeOuGrupo>
```

---

**Critérios de Avaliação**

* Correção sintática e semântica do DDL e DML
* Consistência com o modelo ER proposto
* Coerência e relevância das consultas SQL
* Organização e clareza na documentação entregue

Bom trabalho!
