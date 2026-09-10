Trabalho II — Normalização de Base de Dados (Sistema de Gestão de
Funcionários), da disciplina de Base de Dados, Curso de Licenciatura em
Informática, Universidade Licungo.
Autor: Edwin Jerry Joaquim Taimo Gema
Sobre o projeto
Uma empresa moçambicana guardava todos os dados dos seus funcionários numa
única folha de cálculo (`Dados_Nao_Normalizados_Funcionarios.xlsx`, 16
funcionários), com colunas repetidas para filhos e contactos telefónicos, e
texto duplicado para cargos, funções, cidades e províncias.
Este repositório documenta, passo a passo, a normalização dessa tabela até
à 4.ª Forma Normal (4FN) e apresenta o esquema relacional final, pronto a
usar numa base de dados real.
Como consultar este repositório
O que procuras	Onde encontrar
Identificação dos problemas na tabela original	`documentos/01-identificacao-problemas.md`
1.ª Forma Normal — eliminação dos grupos repetitivos	`documentos/02-primeira-forma-normal-1FN.md`
2.ª Forma Normal — eliminação das dependências parciais	`documentos/03-segunda-forma-normal-2FN.md`
3.ª Forma Normal — eliminação das dependências transitivas	`documentos/04-terceira-forma-normal-3FN.md`
4.ª Forma Normal — dependências multivaloradas	`documentos/05-quarta-forma-normal-4FN.md`
Cardinalidades dos relacionamentos	`documentos/06-cardinalidades.md`
Diagrama do Modelo Entidade-Relacionamento	`diagramas/modelo-er.md` (renderizado automaticamente pelo GitHub)
Script de criação das tabelas (DDL)	`sql/01_ddl_create_tables.sql`
Script de povoamento com os 16 funcionários	`sql/02_insert_dados.sql`
Queries de exemplo (com JOIN)	`sql/03_queries_exemplo.sql`
Esquema final (resumo)
