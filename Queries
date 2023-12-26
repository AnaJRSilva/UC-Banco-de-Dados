---------- Cinco consultas com operadores básicos e junção de, no mínimo, duas tabelas ----------
-- 1. Consultar nome, classe e uso dos mateirias que geram resíduos que precisa de tratamento especial
SELECT M.Tipo, R.Classe, U.Inicial
FROM Materiais M
JOIN Uso U ON M.CodMaterial = U.Uso_CodMaterial
JOIN Residuos R ON R.Classe = U.Uso_Classe
WHERE U.Subsequente IS NULL;

-- 2. Consultar os materiais, as classes e reusos dos materiais que geram resíduos recicláveis ou reutilizavéis
SELECT M.Tipo, R.Classe, U.Subsequente
FROM Materiais M
JOIN Uso U ON M.CodMaterial = U.Uso_CodMaterial
JOIN Residuos R ON U.Uso_Classe = R.Classe
WHERE U.Subsequente IS NOT NULL;

-- 3. Consultar nome e em qual obra os funcionários trabalham
SELECT MO.Nome AS NomeFuncionario, O.CodObra AS Obra
FROM Mao_de_Obra MO
JOIN Cadastro C ON MO.CodFuncionario = C.Cad_CodFuncionario
JOIN Obra O ON C.Cad_CodObra = O.CodObra
GROUP BY NomeFuncionario, Obra;

-- 4. Consultar responsável, data de conclusão (da data mais próxima para a menos próxima) e observação das obras
SELECT P.NomeProp AS Responsavel, O.Data_previsao AS 'Data de Conclusão', D.Obs_Geral
FROM Proprietario P
JOIN Contrato C ON P.CodProp = C.Cont_CodProp
JOIN Obra O ON C.Cont_CodObra = O.CodObra
JOIN Trabalho T ON O.CodObra = T.Trab_CodObra
JOIN Diario D ON T.Trab_CodDiario = D.CodDiario
ORDER BY O.Data_previsao ASC;

-- 5. Consultar quais obras não foram iniciadas, mostrar seu código e seu endereço
SELECT O.CodObra, O.Endereco
FROM Obra O
LEFT JOIN Cadastro CAD ON O.CodObra = CAD.Cad_CodObra
WHERE CAD.Cad_CodObra IS NULL;

---------- Duas consultas com LEFT JOIN ----------
-- 6. Consultar obras não iniciadas e qual a previsão da finalização delas, caso a obra não tenha previsão, deve aparecer no retorno mesmo assim.
SELECT O.Endereco AS Obra, O.Data_previsao AS 'Data de Previsão'
FROM Obra O
LEFT JOIN Cadastro CAD ON CAD.CodObra = O.CodObra
WHERE CAD.Cad_CodObra IS NULL; -- Obras sem cadastro respresentam obras não iniciadas

-- 7. Consultar o tipo de equipamento, marca e aluguel em ordem decrescente (maior para menor). Caso não tenha registro de aluguel, manter no retorno
SELECT E.Tipo, E.Marca, A.Valor
FROM Equipamentos E
LEFT JOIN Aluguel A ON E.CodEquipamento = A.CodEquipamento
ORDER BY A.Valor DESC;

----------  5 consultas com os operadores (avg, sum, etc.) usando group by, having e order by ----------
-- 8. Consultar o maior e o menor salário com o nome do respectivo funcionário e em qual obra ele atua
SELECT MO.Nome AS NomeFuncionario, O.CodObra AS Obra, MAX(MO.Salario) AS MaiorSalario, MIN(MO.Salario) AS MenorSalario
FROM Mao_de_Obra MO
JOIN Cadastro C ON MO.CodFuncionario = C.Cad_CodFuncionario
JOIN Obra O ON C.Cad_CodObra = O.CodObra
GROUP BY NomeFuncionario, Obra;

-- 9. Consultar a quantidade de funcionários em cada obra e o endereço de cada obra, ordenando da maior quantidade de funcionarios até a menor
SELECT O.Endereco, COUNT(MO.CodFuncionario) AS QtdFunc
FROM Obra O, Mao_de_Obra MO, Cadastro C
WHERE O.CodObra = C.Cad_CodObra
AND C.Cad_CodFuncionario = MO.CodFuncionario
GROUP BY O.Endereco
ORDER BY QtdFunc DESC;


-- 10. Consultar a média de preço dos materiais usados nas obras, e depois mostrar os materiais estão acima da média
SELECT M.Tipo, M.Custo
FROM Materiais M
WHERE M.Custo > (
    SELECT AVG(M2.Custo)
    FROM Materiais M2
);

-- 11. Consultar cada tipo de equipamento, a do valor de aluguel de cada equipamento e sua media periodo
SELECT E.Tipo, AVG(A.Valor) AS MediaValor, AVG(A.Periodo) AS MediaPeriodo
FROM Equipamentos E
JOIN Aluguel A ON E.CodEquipamento = A.Alug_CodEquipamento
GROUP BY E.Tipo;


-- 12. Consultar a quantidade de materiais que estão nas classes A e C, e mostrar a quantidade de cada classe
SELECT R.Classe, COUNT(*) AS Quantidade
FROM Residuos R
WHERE R.Classe IN ('Classe A', 'Classe C')
GROUP BY R.Classe;
