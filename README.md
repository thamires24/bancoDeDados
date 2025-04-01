# bancoDeDados

-- Criar tipo ENUM para status da consulta
CREATE TYPE status_consulta AS ENUM('Agendado', 'Cancelado');

-- Criar tabela endereco
CREATE TABLE endereco (
    idEndereco SERIAL PRIMARY KEY,
    UF CHAR(2) NOT NULL,
    CEP CHAR(8) NOT NULL,
    cidade VARCHAR(100) NOT NULL,
    bairro VARCHAR(100) NOT NULL,
    logradouro VARCHAR(100) NOT NULL,
    numero CHAR(10)
);

-- Criar tabela dentista
CREATE TABLE dentista (
    idDentista SERIAL PRIMARY KEY,
    nomeCompleto VARCHAR(200) NOT NULL,
    cpf CHAR(11) UNIQUE NOT NULL,
    cro CHAR(6) UNIQUE NOT NULL,
    especialidade VARCHAR(100) NOT NULL,
    horario TIME NOT NULL
);

-- Criar tabela paciente
CREATE TABLE paciente (
    idPaciente SERIAL PRIMARY KEY,
    nomeCompleto VARCHAR(200) NOT NULL,
    cpf CHAR(11) UNIQUE NOT NULL,
    dataNascimento DATE NOT NULL, 
    telefone CHAR(11) NOT NULL,
    email VARCHAR(100) NOT NULL,
    idEndereco INT NOT NULL,
    FOREIGN KEY (idEndereco) REFERENCES endereco(idEndereco)
);

-- Criar tabela procedimento odontológico
CREATE TABLE procedimento_odontologico (
    idProcOdon SERIAL PRIMARY KEY, 
    nome VARCHAR(200) NOT NULL,
    descricao VARCHAR(200) NOT NULL,
    duracao TIME NOT NULL
);

-- Criar tabela consulta
CREATE TABLE consulta (
    idConsulta SERIAL PRIMARY KEY,
    data DATE NOT NULL,
    horario TIME NOT NULL,
    prescricao VARCHAR(200) NOT NULL,
    idDentista INT NOT NULL,
    idPaciente INT NOT NULL,
    idProcOdon INT NOT NULL,
    status status_consulta DEFAULT 'Agendado' NOT NULL,
    FOREIGN KEY (idDentista) REFERENCES dentista(idDentista) ON DELETE CASCADE,
    FOREIGN KEY (idPaciente) REFERENCES paciente(idPaciente) ON DELETE CASCADE,
    FOREIGN KEY (idProcOdon) REFERENCES procedimento_odontologico(idProcOdon)
);

--INSERÇÃO 
-- Inserindo dados na tabela endereco
INSERT INTO endereco (UF, CEP, cidade, bairro, logradouro, numero) VALUES
('SP', '01001000', 'São Paulo', 'Centro', 'Rua A', '100'),
('SP', '02002000', 'São Paulo', 'Santana', 'Rua B', '200'),
('RJ', '22030000', 'Rio de Janeiro', 'Copacabana', 'Avenida Atlântica', '300'),
('MG', '30140000', 'Belo Horizonte', 'Savassi', 'Rua C', '400'),
('RS', '90010000', 'Porto Alegre', 'Centro', 'Rua D', '500'),
('PR', '80010000', 'Curitiba', 'Batel', 'Rua E', '600'),
('SC', '88010000', 'Florianópolis', 'Centro', 'Rua F', '700'),
('BA', '40010000', 'Salvador', 'Barra', 'Avenida Oceânica', '800'),
('PE', '50010000', 'Recife', 'Boa Viagem', 'Rua G', '900'),
('CE', '60010000', 'Fortaleza', 'Meireles', 'Rua H', '1000');

-- Inserindo dados na tabela dentista
INSERT INTO dentista (nomeCompleto, cpf, cro, especialidade, horario) VALUES
('Dr. João Silva', '12345678901', 'SP1234', 'Ortodontia', '08:00:00'),
('Dra. Maria Souza', '23456789012', 'SP5678', 'Endodontia', '09:00:00'),
('Dr. Pedro Santos', '34567890123', 'RJ9101', 'Implantodontia', '10:00:00'),
('Dra. Ana Lima', '45678901234', 'MG1112', 'Periodontia', '11:00:00'),
('Dr. Carlos Rocha', '56789012345', 'RS1314', 'Clínico Geral', '13:00:00'),
('Dra. Beatriz Alves', '67890123456', 'PR1516', 'Odontopediatria', '14:00:00'),
('Dr. Eduardo Melo', '78901234567', 'SC1718', 'Ortodontia', '15:00:00'),
('Dra. Fernanda Costa', '89012345678', 'BA1920', 'Endodontia', '16:00:00'),
('Dr. Rafael Pinto', '90123456789', 'PE2122', 'Implantodontia', '17:00:00'),
('Dra. Juliana Castro', '01234567890', 'CE2324', 'Periodontia', '18:00:00');

-- Inserindo dados na tabela paciente
INSERT INTO paciente (nomeCompleto, cpf, dataNascimento, telefone, email, idEndereco) VALUES
('Carlos Almeida', '12345678910', '1990-05-15', '11987654321', 'carlos@email.com', 1),
('Fernanda Ribeiro', '23456789101', '1985-07-20', '21987654321', 'fernanda@email.com', 2),
('Ricardo Mendes', '34567891012', '1978-02-11', '31987654321', 'ricardo@email.com', 3),
('Mariana Lopes', '45678910123', '1995-09-30', '41987654321', 'mariana@email.com', 4),
('Gustavo Torres', '56789101234', '1982-06-25', '51987654321', 'gustavo@email.com', 5),
('Patrícia Silva', '67891012345', '2000-01-05', '61987654321', 'patricia@email.com', 6),
('Rafael Nunes', '78910123456', '1993-12-18', '71987654321', 'rafael@email.com', 7),
('Tatiane Oliveira', '89101234567', '1987-04-10', '81987654321', 'tatiane@email.com', 8),
('Rodrigo Martins', '90123456789', '1975-08-22', '91987654321', 'rodrigo@email.com', 9),
('Aline Campos', '01234567891', '1998-03-14', '10123456789', 'aline@email.com', 10);

-- Inserindo dados na tabela procedimento_odontologico
INSERT INTO procedimento_odontologico (nome, descricao, duracao) VALUES
('Limpeza', 'Higienização completa dos dentes', '00:30:00'),
('Canal', 'Tratamento de canal', '01:30:00'),
('Extração', 'Extração de dente', '00:45:00'),
('Clareamento', 'Branqueamento dental', '01:00:00'),
('Aparelho', 'Colocação de aparelho ortodôntico', '02:00:00'),
('Implante', 'Implante dentário', '02:30:00'),
('Raio-X', 'Radiografia odontológica', '00:15:00'),
('Restauração', 'Correção de cárie dentária', '00:40:00'),
('Protese', 'Instalação de prótese dentária', '02:00:00'),
('Consulta Inicial', 'Avaliação odontológica inicial', '00:20:00');

-- Inserindo dados na tabela consulta
INSERT INTO consulta (data, horario, prescricao, idDentista, idPaciente, idProcOdon, status) VALUES
('2025-04-10', '09:00:00', 'Tomar analgésico após o procedimento', 1, 1, 1, 'Agendado'),
('2025-04-11', '10:00:00', 'Uso diário de fio dental', 2, 2, 2, 'Agendado'),
('2025-04-12', '11:00:00', 'Evitar alimentos duros', 3, 3, 3, 'Cancelado'),
('2025-04-13', '12:00:00', 'Escovação 3x ao dia', 4, 4, 4, 'Agendado'),
('2025-04-14', '13:00:00', 'Tomar antibiótico por 7 dias', 5, 5, 5, 'Agendado'),
('2025-04-15', '14:00:00', 'Evitar bebidas geladas', 6, 6, 6, 'Agendado'),
('2025-04-16', '15:00:00', 'Revisão em 30 dias', 7, 7, 7, 'Cancelado'),
('2025-04-17', '16:00:00', 'Aplicar gel dental 2x ao dia', 8, 8, 8, 'Agendado'),
('2025-04-18', '17:00:00', 'Voltar para ajustes no aparelho', 9, 9, 9, 'Agendado'),
('2025-04-19', '18:00:00', 'Tomar analgésico após o procedimento', 10, 10, 10, 'Agendado');

--INDICE:
CREATE INDEX index_nome_paciente ON paciente(nomeCompleto);
CREATE INDEX index_nome_dentista ON dentista(nomeCompleto);
--ATUALIZAÇÃO:
UPDATE paciente 
SET telefone = '11987654322'
WHERE telefone = '11987654321';

UPDATE paciente 
SET telefone = '10123456799'
WHERE telefone = '101234567891';

UPDATE paciente 
SET telefone = '51987654388'
WHERE telefone = '51987654321';

--EXCLUSÃO:
DELETE FROM paciente
WHERE idPaciente = '8';

DELETE FROM paciente
WHERE idPaciente = '9';

DELETE FROM paciente
WHERE idPaciente = '10';

SELECT * FROM CONSULTA;
SELECT * FROM dentista;

--CONSULTAS CONTEXTUALIZADAS:
--1:
SELECT d.especialidade, COUNT(c.iddentista) AS total_consultas
FROM consulta c
JOIN dentista d ON c.iddentista = d.iddentista
GROUP BY d.especialidade;

--2:
SELECT d.nomeCompleto, COUNT(c.idconsulta) AS total_consultas
FROM dentista d
JOIN consulta c ON c.iddentista = d.iddentista
GROUP BY d.nomeCompleto
ORDER BY total_consultas DESC;

--3:
SELECT p.nomeCompleto, COUNT(c.idconsulta) AS total_consultas
FROM paciente p
JOIN consulta c ON c.idpaciente = p.idpaciente
GROUP BY p.nomeCompleto
ORDER BY total_consultas DESC;

--View:
CREATE VIEW vw_consulta_data AS 
SELECT c.idconsulta, p.nomeCompleto AS nome_paciente, d.nomeCompleto AS nome_dentista, c.data, po.nome
FROM consulta c
JOIN paciente p ON c.idpaciente = p.idpaciente
JOIN dentista d ON c.iddentista = d.iddentista
JOIN procedimento_odontologico po ON c.idProcOdon = po.idProcOdon
ORDER BY c.data DESC

SELECT * FROM vw_consulta_data

--MEDIA:
SELECT d.nomecompleto, COUNT(c.idconsulta) AS total_consultas, 
       ROUND(AVG(COUNT(c.idconsulta)) OVER(), 2) AS media_consultas
FROM dentista d
LEFT JOIN consulta c ON d.iddentista = c.iddentista
GROUP BY d.iddentista, d.nomecompleto;

--O sistema deve permitir a atualização ou cancelamento de consultas, respeitando regras de
--prazo mínimo para alterações.
DELIMITER //

CREATE PROCEDURE alteracao_consulta(IN consulta_id INT)
BEGIN
    DECLARE data_consulta DATETIME;
    DECLARE dias_restantes INT;

    -- Buscar a data da consulta
    SELECT data INTO data_consulta 
    FROM consulta 
    WHERE idConsulta = consulta_id;

    -- Calcular dias restantes até a consulta
    SET dias_restantes = DATEDIFF(data_consulta, NOW());

    -- Verificar se faltam menos de 3 dias
    IF dias_restantes < 3 THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Não é possível cancelar a consulta com menos de 3 dias de antecedência!';
    ELSE
        -- Atualiza o status para "Cancelado"
        UPDATE consulta 
        SET status = 'Cancelado' 
        WHERE idConsulta = consulta_id;
        
        SELECT 'Consulta cancelada com sucesso!' AS Resultado;
    END IF;
END //

DELIMITER ;


CREATE OR REPLACE FUNCTION alteracao_consulta(consulta_id INT)
RETURNS TEXT AS $$
DECLARE
    data_consulta TIMESTAMP;
    dias_restantes INT;
BEGIN
    -- Buscar a data da consulta
    SELECT data INTO data_consulta 
    FROM consulta 
    WHERE idConsulta = consulta_id;

    -- Se não encontrar a consulta, retorna erro
    IF NOT FOUND THEN
        RAISE EXCEPTION 'Consulta não encontrada!';
    END IF;

    -- Calcular dias restantes até a consulta
    dias_restantes := (data_consulta::DATE - CURRENT_DATE);

    -- Verificar se faltam menos de 3 dias
    IF dias_restantes < 3 THEN
        RAISE EXCEPTION 'Não é possível cancelar a consulta com menos de 3 dias de antecedência!';
    ELSE
        -- Atualiza o status para "Cancelado"
        UPDATE consulta 
        SET status = 'Cancelado' 
        WHERE idConsulta = consulta_id;

        RETURN 'Consulta cancelada com sucesso!';
    END IF;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM consulta
SELECT alteracao_consulta(11)
