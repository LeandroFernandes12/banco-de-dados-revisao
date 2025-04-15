# Resolução

## Implementação SQL

### 1. Criação do Banco de Dados

```sql
-- Criação do banco de dados
CREATE DATABASE events CHARACTER SET utf8mb4;

-- Seleciona o banco de dados

SHOW DATABASES;

```

### 2. Criação das Tabelas

```sql
-- Tabela de palestrantes
CREATE TABLE palestrantes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    especialidade VARCHAR(100),
    email VARCHAR(100) NOT NULL;
);

-- Tabela de eventos
CREATE TABLE eventos (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    data_evento DATE NOT NULL,
    local VARCHAR(200) NOT NULL,
    capacidade INT NOT NULL,
    palestrantes_id INT;
);

-- Tabela de inscrições
CREATE TABLE inscricoes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    eventos_id INT NOT NULL,
    nome_participante VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    data_inscricao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    presente TINYINT;
);

```

### 3. Inserção de Dados Iniciais

```sql
-- Inserir palestrantes
INSERT INTO palestrantes (id, nome, especialidade, email)
VALUES (1, 'Manoel Gomes', 'canto', 'canetaazul@gmail.com');

INSERT INTO palestrantes (id, nome, especialidade, email)
VALUES (2, 'Ednaldo Pereira', 'música', 'edinaldovaletudo@gmail.com');



-- Inserir eventos
INSERT INTO eventos (id, titulo, data_evento, local, capacidade, palestrantes_id)
VALUES (1, 'Caneta Azul Show', '2012-12-12', 'Brumadinho', 600, 1);

INSERT INTO eventos (id, titulo, data_evento, local, capacidade, palestrantes_id)
VALUES (2, 'Vale tudo Vale nada', '2007-10-15', 'Beira-mar', 700, 2);



-- Inserir algumas inscrições
INSERT INTO inscricoes (id, eventos_id, nome_participante, email, presente)
VALUES (1, 1, 'CalangoPVP', 'pantaloreza@gmail.com', TRUE);

INSERT INTO inscricoes (id, eventos_id, nome_participante, email, presente)
VALUES (2, 2, 'ZeroBadass', 'alcinojunior420@gmail.com', TRUE);

```

### 4. View para Consulta

```sql
-- View para listar eventos com detalhes
CREATE VIEW vw_eventos_detalhados AS
SELECT 
    e.id,
    e.titulo,
    e.data_evento,
    e.local,
    i.nome_participante,
    e.capacidade,
    e.palestrantes_id,
    p.nome AS palestrante,
    p.especialidade,
    COUNT(i.id) AS total_inscritos,
    e.capacidade - COUNT(i.id) AS vagas_disponiveis
FROM 
    eventos e
LEFT JOIN 
    palestrantes p ON e.palestrantes_id = p.id
LEFT JOIN 
    inscricoes i ON e.id = i.eventos_id
GROUP BY 
    e.id, e.titulo, e.data_evento, e.local, e.capacidade, p.nome, p.especialidade, i.nome_participante

-- Consultar a view
SELECT * FROM vw_eventos_detalhados;
```

### 5. Consultas Úteis

```sql
-- 1. Listar todos os eventos com suas respectivas informações
SELECT id, titulo, data_evento, local, capacidade 
FROM vw_eventos_detalhados;


-- 2. Mostrar apenas eventos com vagas disponíveis (usando a view criada)
SELECT id, titulo, data_evento, local, vagas_disponiveis 
FROM vw_eventos_detalhados
WHERE vagas_disponiveis > 0;

-- 3. Listar participantes inscritos em um evento específico
SELECT id, nome_participante, titulo 
FROM vw_eventos_detalhados
WHERE id = 1;



```
