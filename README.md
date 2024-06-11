# Sistema-Universidade1
CREATE DATABASE universidade;
USE universidade;

CREATE TABLE Areas (
    id_area INT PRIMARY KEY,
    nome_area VARCHAR(50) NOT NULL
);

CREATE TABLE Cursos (
    id_curso INT PRIMARY KEY,
    nome_curso VARCHAR(50) NOT NULL,
    duracao INT NOT NULL,
    id_area INT FOREIGN KEY REFERENCES Areas(id_area)
);

CREATE TABLE Alunos (
    id_aluno INT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    email VARCHAR(50) UNIQUE NOT NULL,
    id_curso INT FOREIGN KEY REFERENCES Cursos(id_curso)
);

2****
CREATE PROCEDURE sp_inserir_curso
    @nome_curso VARCHAR(50),
    @duracao INT,
    @id_area INT
AS
BEGIN
    INSERT INTO Cursos (nome_curso, duracao, id_area)
    VALUES (@nome_curso, @duracao, @id_area)
END
GO

3***
CREATE FUNCTION fn_obter_id_curso
    @nome_curso VARCHAR(50),
    @id_area INT
)
RETURNS INT
AS
BEGIN
    DECLARE @id_curso INT
    SELECT @id_curso = id_curso
    FROM Cursos
    WHERE nome_curso = @nome_curso AND id_area = @id_area
    RETURN @id_curso
END
GO
4***
CREATE PROCEDURE sp_matricular_aluno
    @nome VARCHAR(50),
    @id_curso INT
AS
BEGIN
    DECLARE @email VARCHAR(50)
    SET @email = LOWER(REPLACE(@nome, ' ', '.')) + '@universidade.com'

    IF NOT EXISTS (SELECT 1 FROM Alunos WHERE id_curso = @id_curso AND email = @email)
    BEGIN
        INSERT INTO Alunos (nome, email, id_curso)
        VALUES (@nome, @email, @id_curso)
    END
    ELSE
    BEGIN
        RAISERROR('Aluno já matriculado neste curso.', 16, 1)
    END
END
GO
5***
EXEC sp_inserir_curso 'Engenharia Civil', 4
EXEC sp_inserir_curso 'Direito', 5 
EXEC sp_inserir_curso 'Medicina Veterinária', 1
EXEC sp_inserir_curso 'Arquitetura', 4
EXEC sp_inserir_curso 'Psicologia', 6
EXEC sp_inserir_curso 'Odontologia', 7
EXEC sp_inserir_curso 'Engenharia Mecânica', 4
EXEC sp_inserir_curso 'Economia', 2
EXEC sp_inserir_curso 'Pedagogia', 6
EXEC sp_inserir_curso 'Enfermagem', 7
EXEC sp_inserir_curso 'Engenharia Elétrica', 4
EXEC sp_inserir_curso 'Farmácia', 7
EXEC sp_inserir_curso 'Fisioterapia', 7
EXEC sp_inserir_curso 'Publicidade e Propaganda', 2
EXEC sp_inserir_curso 'Nutrição', 7
EXEC sp_inserir_curso 'Jornalismo', 2
EXEC sp_inserir_curso 'Fonoaudiologia', 7
EXEC sp_inserir_curso 'Design', 4
EXEC sp_inserir_curso 'Relações Internacionais', 2
EXEC sp_inserir_curso 'Terapia Ocupacional', 7
EXEC sp_inserir_curso 'Serviço Social', 6
EXEC sp_inserir_curso 'Educação Física', 6

