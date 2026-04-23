<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/7bd9c725-4dd6-47b9-90ce-246e925fc389" /># Atividade_ConsultaSQL_FICR
Realização de Atividade em Sala de Aula 

## 🙎Usuários 

### 1 - Quantos usuários estão cadastrados no sistema?
SELECT  COUNT(*) AS total_usuarios from usuarios ;

### 2- Qual o peso médio dos usuários?
SELECT AVG(peso) AS peso_medio FROM usuarios;

### 3 - Qual usuário possui a maior meta diária?
SELECT nome, meta_diaria_ml
FROM usuarios
ORDER BY meta_diaria_ml DESC
LIMIT 1;

### 4 - Qual usuário possui a menor meta diária?
SELECT nome, meta_diaria_ml
FROM usuarios
ORDER BY meta_diaria_ml ASC
LIMIT 1;

## 💧Consumo de Água

###  5- Qual o total de água ingerido por cada usuário?
SELECT u.nome, SUM(i.quantidade_ml) AS total_ml
FROM usuarios u
JOIN ingestao_agua i ON u.id = i.usuario_id
GROUP BY u.nome;

### 6 - Qual o total de água ingerido em um dia específico?
SELECT SUM(quantidade_ml) AS total_ml
FROM ingestao_agua
WHERE DATE(data_hora) = ' 2026-04-15'

### 7 - Qual o usuário que mais bebeu água no período?
SELECT u.nome, SUM(i.quantidade_ml) AS total_ml
FROM ingestao_agua i
JOIN usuarios u ON u.id = i.usuario_id
WHERE i.data_hora >= '2026-04-15 16:00:00'
  AND i.data_hora <  '2026-04-19 20:00:00'
GROUP BY u.nome
ORDER BY total_ml DESC
LIMIT 1;

### 8 - Qual o usuário que menos bebeu água no período?
SELECT u.nome, SUM(i.quantidade_ml) AS total_ml
FROM ingestao_agua i
JOIN usuarios u ON u.id = i.usuario_id
WHERE i.data_hora >= '2026-04-15 16:00:00'
  AND i.data_hora <  '2026-04-19 20:00:00'
GROUP BY u.nome
ORDER BY total_ml ASC
LIMIT 1;
.
### 9 - Qual o consumo médio diário por usuário?
SELECT u.nome, AVG(r.total_ml) AS media_diaria_ml
FROM resumo_diario_agua r
JOIN usuarios u ON u.id = r.usuario_id
GROUP BY u.nome;

### 10 - Qual o consumo médio por ingestão (copo)?
SELECT ROUND(AVG(quantidade_ml), 2) AS media_por_copo
FROM ingestao_agua;

- ROUND (Arredonda o valor decimal).

## 📅 Análise por Dia 

### 11- Quanto cada usuário bebeu? 
SELECT 
    usuario_id,
    data,
    total_ml
FROM resumo_diario_agua
ORDER BY data, usuario_id;

### 12 - Qual foi o dia com maior consumo total de água?
SELECT 
    DATE(data_hora) AS data,
    SUM(quantidade_ml) AS total_ml
FROM ingestao_agua
GROUP BY DATE(data_hora)
ORDER BY total_ml DESC
LIMIT 1;

### 13 - Qual foi o dia com menor consumo total?
SELECT 
    DATE(data_hora) AS data,
    SUM(quantidade_ml) AS total_ml
FROM ingestao_agua
GROUP BY DATE(data_hora)
ORDER BY total_ml ASC
LIMIT 1;

### 14 - Quantos registros de ingestão existem por dia?
SELECT 
    DATE(data_hora) AS data,
    COUNT(*) AS total_registros
FROM ingestao_agua
GROUP BY DATE(data_hora)
ORDER BY data;

## 🎯 Metas 

### 15 - Qual usuário atingiu a meta diária em cada dia?
SELECT 
    r.usuario_id,
    r.data,
    r.total_ml,
    m.meta_ml,
    (r.total_ml >= m.meta_ml) AS atingiu_meta
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id 
    AND r.data = m.data;

### 16 - Quantos dias cada usuário bateu a meta?
SELECT 
    r.usuario_id,
    COUNT(*) AS dias_meta_atingida
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id 
    AND r.data = m.data
WHERE r.total_ml >= m.meta_ml
GROUP BY r.usuario_id
ORDER BY dias_meta_atingida DESC;

### 17 - Qual a porcentagem da meta atingida por dia por usuário?
SELECT 
    r.usuario_id,
    r.data,
    r.total_ml,
    m.meta_ml,
    ROUND((r.total_ml * 100.0 / m.meta_ml), 2) AS percentual_meta
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id 
    AND r.data = m.data;

### 18 - Qual usuário tem melhor desempenho (mais dias com meta atingida)?
SELECT 
    r.usuario_id,
    COUNT(*) AS dias_meta_atingida
FROM resumo_diario_agua r
JOIN metas_diarias m 
    ON r.usuario_id = m.usuario_id 
    AND r.data = m.data
WHERE r.total_ml >= m.meta_ml
GROUP BY r.usuario_id
ORDER BY dias_meta_atingida DESC
LIMIT 1;

## 🧠Inteligência/Produto

### 19 - Em quais horários ocorre maior consumo de água?
SELECT 
    EXTRACT(HOUR FROM data_hora) AS hora,
    SUM(quantidade_ml) AS total_ml
FROM ingestao_agua
GROUP BY hora
ORDER BY total_ml DESC
;

### 20 - Qual o intervalo médio entre ingestões por usuário?
SELECT 
    usuario_id,
    AVG(intervalo) AS intervalo_medio
FROM (
    SELECT 
        usuario_id,
        data_hora,
        data_hora - LAG(data_hora) OVER (
            PARTITION BY usuario_id 
            ORDER BY data_hora
        ) AS intervalo
    FROM ingestao_agua
) sub
WHERE intervalo IS NOT NULL
GROUP BY usuario_id;


