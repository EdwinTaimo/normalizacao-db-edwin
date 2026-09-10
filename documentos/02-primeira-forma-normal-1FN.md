# 2. Primeira Forma Normal (1FN)

## Regra aplicada

Uma tabela está em 1FN quando: (a) todos os atributos têm valores atómicos, e
(b) não existem grupos repetitivos (colunas do tipo `X 1`, `X 2`, `X 3`).

## O que foi feito

Os dois grupos repetitivos identificados (`Filho 1/2/3` e `Celular 1/2/3`)
foram retirados da tabela de funcionários e transformados em **duas novas
tabelas**, cada uma com uma linha por valor (em vez de uma coluna por
posição):

- `FUNCIONARIO_FILHO` — uma linha por cada filho de cada funcionário.
- `FUNCIONARIO_CONTACTO` — uma linha por cada número de telefone de cada
  funcionário.

A tabela de funcionários mantém-se, por agora, "larga" (ainda com `Cargo`,
`Função`, `Cidade`, `Província`, `País` por extenso) — essa redundância só é
tratada nas fases seguintes (2FN e 3FN). O importante nesta fase é garantir
que já não há colunas repetidas nem grupos multivalorados dentro de uma
célula.

## Resultado: `FUNCIONARIO_1FN`

Chave (nesta fase intermédia): `NUIT`

| NUIT | Nome | Data Nasc. | BI | Email | Endereço | Cidade | Província | País | Cargo | Cód. Cargo | Função | Cód. Função | Posto de Trabalho | Data Admissão |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 100234567 | Amélia Fernanda Cossa | 1985-03-12 | 110100123456A | amelia.cossa@empresa.co.mz | Av. Julius Nyerere, n.º 245, Sommerschield | Maputo | Maputo Cidade | Moçambique | Técnico de Informática | C01 | Tecnologias de Informação | F01 | Sede Maputo | 2015-02-05 |
| 100345678 | Bernardo Alfredo Machava | 1979-07-22 | 110100234567B | bernardo.machava@empresa.co.mz | Rua da Resistência, n.º 8, Polana Caniço | Maputo | Maputo Cidade | Moçambique | Contabilista | C02 | Finanças | F02 | Sede Maputo | 2010-09-14 |
| … | … | … | … | … | … | … | … | … | … | … | … | … | … | … |

*(tabela completa com os 16 funcionários em `sql/02_insert_dados.sql`)*

## Resultado: `FUNCIONARIO_FILHO`

Chave: `(NUIT, Nome_Filho)` — composta, porque só o `NUIT` já não identifica
uma linha (um funcionário pode ter várias).

| NUIT | Nome_Filho |
|---|---|
| 100234567 | Cátia Cossa |
| 100345678 | Nelson Machava |
| 100345678 | Ivete Machava |
| 100345678 | Suzana Machava |
| 100567890 | Paulo Nhantumbo Jr |
| 100567890 | Alzira Nhantumbo |
| … | … |

*(20 filhos no total — lista completa em `sql/02_insert_dados.sql`)*

> Nota: `100456789` (Celina Sitoe) e `101678901` (Paulina Uache) não têm
> nenhuma linha nesta tabela, porque não têm filhos registados. Isto já
> mostra uma vantagem imediata da 1FN: deixamos de gastar espaço com colunas
> vazias (`Filho 2`, `Filho 3` a NULL) para estes casos.

## Resultado: `FUNCIONARIO_CONTACTO`

Chave: `(NUIT, Numero_Celular)`.

| NUIT | Numero_Celular |
|---|---|
| 100234567 | 841234567 |
| 100234567 | 821234567 |
| 100345678 | 845678901 |
| 100345678 | 861122334 |
| … | … |

*(26 contactos no total — lista completa em `sql/02_insert_dados.sql`)*

## O que já ficou resolvido

- Já não há colunas `Filho 1/2/3` nem `Celular 1/2/3` — cada linha destas
  duas novas tabelas representa **um único valor**, atómico.
- Um funcionário pode agora ter qualquer número de filhos ou de contactos
  (0, 1, 5, 10...) sem alterar a estrutura da base de dados.

## O que ainda falta resolver

- `FUNCIONARIO_1FN` ainda tem uma dependência parcial embutida na sua própria
  história (explicada no documento anterior) e, mais visivelmente, ainda
  repete texto (`Cargo`, `Função`, `Província`, `País`) que devia estar em
  tabelas de referência → resolvido nas fases 2FN e 3FN.
