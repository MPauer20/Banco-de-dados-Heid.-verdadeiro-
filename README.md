# Banco-de-dados-Heid.-verdadeiro-
## Encontrar a nota máxima, a nota minima e a média de horas de sono

    SELECT
        MAX(nota_exame) AS notas_maxima,
        MIN(nota_exame) AS nota_minima,
        AVG(horas_sono) AS media_horas_sono
    FROM notas_alunos;

## Contar quantos alunos tiram menos de 25.0 no exame:

    SELECT COUNT(id_aluno) AS total_alunos_baixo_desempenho
    FROM notas_alunos
    WHERE nota_exame < 25.0;

## Listar os 5 alunos com as maiores notas no exame (nota_exame), mostrando a nota e as horas estudadas:

    SELECT id_aluno, nota_exame, horas_estudadas
    FROM notas_alunos
    ORDER BY nota_exame DESC
    LIMIT 5;

## listar todos os alunos ordenados do menor para o maior percentual de presença:

    SELECT id_aluno, percentual_presenca
    FROM notas_alunos
    ORDER BY percentual_presenca ASC;

    -- Filtragem avançada (group by, having)
    -- Classificar os alunos por faixas de notas_anteriores
    -- ex Faixa A:>90, Faixa B:>70-90, Faixa C <70 e calcular a nota_exame média para cada faixa

    SELECT
    CASE
        WHEN notas_anteriores > 90 THEN 'Faixa A (>90)'
        WHEN notas_anteriores BETWEEN 70 AND 90 THEN 'Faixa B (70-90)'
    ELSE 'Faixa C (<70)'
    END AS faixa_notas_anteriores,
    AVG(nota_exame) AS media_nota_exame,
    COUNT(id_aluno) AS total_alunos
    FROM notas_alunos
    GROUP BY faixa_notas_anteriores
    ORDER BY media_nota_exame DESC;

## Encontrar a média de nota_exame apenas os alunos que tiverem mais de 90% de presença (percentual_presenca):

    SELECT AVG(nota_exame) AS media_exame_alta_presenca
    FROM notas_alunos
    WHERE percentual_presenca > 90.0;

## Qual é o mínimo de horas de estudo e de sono que a galera que tirou nota acima de 35.0 (uma boa nota) costuma ter?

    SELECT id_aluno, percentual_presenca, horas_sono,MIN(horas_estudadas,)
    FROM notas_alunos
    WHERE nota_exame > 35.0
    ORDER BY nota_exame DESC;

## Quem tem a presença mais baixa (menos de 60%)? Eles compensam isso com horas de estudo?

    SELECT id_aluno, MIN(percentual_presenca), horas_estudadas
    FROM notas_alunos
    WHERE percentual_presenca < 60.0;

## Quem é o aluno Gênio (a maior nota_exame) e qual foi o segredo dele? (Quanto ele estudou e dormiu?)

    SELECT id_aluno, nota_exame, horas_estudadas, horas_sono
    FROM notas_alunos
    ORDER BY nota_exame DESC
    LIMIT 1;

## Quem estudou mais de 10 horas e qual foi o resultado no exame (o sacrifício valeu a pena)?

    SELECT id_aluno, nota_exame, horas_estudadas
    FROM notas_alunos
    WHERE horas_estudadas > 10.0;

## Se sua média anterior é muito alta (acima de 90), você pode "tirar o pé do acelerador" e ainda se sair bem? Qual é a média de estudo desse grupo?

    SELECT
    CASE
        WHEN notas_anteriores > 90 THEN 'Faixa A (>90)'
        WHEN notas_anteriores BETWEEN 70 AND 90 THEN 'Faixa B (70-90)'
    ELSE 'Faixa C (<70)'
    END AS faixa_notas_anteriores,
    AVG(nota_exame) AS media_nota_exame,
    COUNT(id_aluno) AS total_alunos,
    AVG(notas_anteriores) AS media_notas_anteriores,
    AVG(notas_anteriores + nota_exame)/2 AS media_final
    FROM notas_alunos
    GROUP BY faixa_notas_anteriores
    ORDER BY media_nota_exame DESC;

    # #A maioria dos alunos bem-sucedidos tem presença acima de 85%? Ou é possível faltar muito e ainda assim tirar nota alta?

    SELECT id_aluno, AVG(nota_exame), percentual_presenca
    FROM notas_alunos
    ORDER BY nota_exame DESC;

## A maioria dos alunos bem-sucedidos tem presença acima de 85%? Ou é possível faltar muito e ainda assim tirar nota alta?

    SELECT 
    COUNT(id_aluno)
    FROM notas_alunos
    WHERE percentual_presenca < 85.0 AND nota_exame > 35.0;

    SELECT 
    COUNT(id_aluno)
    FROM notas_alunos
    WHERE percentual_presenca > 85.0 AND nota_exame > 35.0;
