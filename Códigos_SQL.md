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

## E-mail do professor: dario.nascimento@p.ficr.edu.br

