# 4. Terceira Forma Normal (3FN)

## Regra aplicada

Uma tabela em 2FN está em 3FN quando nenhum atributo não-chave depende de
outro atributo não-chave (só pode depender da chave, diretamente).

## Dependências transitivas identificadas em `FUNCIONARIO_1FN`

| Cadeia | Explicação |
|---|---|
| `NUIT → Cód_Cargo → Cargo` | O nome do cargo é determinado pelo código do cargo, não pelo funcionário em si. Prova disso: `C01` = "Técnico de Informática" tanto para Amélia Cossa como para Ivete Chirindza — o valor de `Cargo` **repete-se** sempre que `Cód_Cargo` se repete. |
| `NUIT → Cód_Função → Função` | O mesmo raciocínio: `F01` = "Tecnologias de Informação" repete-se para os mesmos dois funcionários. |
| `NUIT → Cidade → Província` | A província é determinada pela cidade (ex.: todos os funcionários de "Beira" estão sempre em "Sofala"; "Maputo" cidade e "Matola" pertencem a províncias diferentes — "Maputo Cidade" e "Maputo Província" — mas cada cidade só pertence a uma província). |
| `NUIT → Cidade → Província → País` | O país é determinado pela província (todas, neste conjunto de dados, são "Moçambique"). |

Em todos os casos, o atributo à direita não descreve o funcionário
diretamente — descreve o **cargo**, a **função** ou a **cidade/província**.
Mantê-los na tabela do funcionário obriga a repetir esse texto em todas as
linhas que partilham o mesmo código/cidade, e cria o risco de
inconsistência (ex.: escrever "Sofala" numa linha e "sofala" ou "Solafa"
noutra).

## O que foi feito

Foram extraídas quatro tabelas de referência, e a tabela do funcionário
passou a guardar apenas os **códigos** (chaves estrangeiras), não os nomes
por extenso:

### `CARGO`
Chave: `Cod_Cargo`

| Cod_Cargo | Nome_Cargo |
|---|---|
| C01 | Técnico de Informática |
| C02 | Contabilista |
| C03 | Engenheiro Civil |
| C04 | Enfermeiro |
| C05 | Professor |
| C06 | Motorista |
| C07 | Gestor de Recursos Humanos |
| C08 | Assistente Administrativo |

### `FUNCAO`
Chave: `Cod_Funcao`

| Cod_Funcao | Nome_Funcao |
|---|---|
| F01 | Tecnologias de Informação |
| F02 | Finanças |
| F03 | Engenharia |
| F04 | Saúde |
| F05 | Educação |
| F06 | Logística |
| F07 | Recursos Humanos |
| F08 | Administração |

### `PROVINCIA`
Chave: `Nome_Provincia`

| Nome_Provincia | Pais |
|---|---|
| Maputo Cidade | Moçambique |
| Maputo Província | Moçambique |
| Gaza | Moçambique |
| Inhambane | Moçambique |
| Sofala | Moçambique |
| Manica | Moçambique |
| Tete | Moçambique |
| Zambézia | Moçambique |
| Nampula | Moçambique |
| Cabo Delgado | Moçambique |

### `CIDADE`
Chave: `Nome_Cidade` · Chave estrangeira: `Nome_Provincia → PROVINCIA`

| Nome_Cidade | Nome_Provincia |
|---|---|
| Maputo | Maputo Cidade |
| Matola | Maputo Província |
| Chókwè | Gaza |
| Maxixe | Inhambane |
| Beira | Sofala |
| Chimoio | Manica |
| Tete | Tete |
| Quelimane | Zambézia |
| Nampula | Nampula |
| Pemba | Cabo Delgado |

### `FUNCIONARIO` (versão em 3FN)

Chave: `id_funcionario` (chave substituta/*surrogate*, ver justificação
abaixo) · Chaves estrangeiras: `Nome_Cidade → CIDADE`, `Cod_Cargo → CARGO`,
`Cod_Funcao → FUNCAO`

| id_funcionario | Nome | Data_Nasc | NUIT | BI | Email | Endereço | Nome_Cidade | Cod_Cargo | Cod_Funcao | Posto_Trabalho | Data_Admissão |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Amélia Fernanda Cossa | 1985-03-12 | 100234567 | 110100123456A | amelia.cossa@empresa.co.mz | Av. Julius Nyerere, n.º 245, Sommerschield | Maputo | C01 | F01 | Sede Maputo | 2015-02-05 |
| … | … | … | … | … | … | … | … | … | … | … | … |

*(tabela completa em `sql/02_insert_dados.sql`)*

## Porquê uma chave substituta (`id_funcionario`) em vez do `NUIT`?

O `NUIT` e o `BI` continuam a identificar univocamente cada funcionário (são
mantidos como `UNIQUE` na tabela), mas optou-se por introduzir uma chave
substituta numérica (`id_funcionario`, auto-incremento) como chave primária,
por duas razões práticas:

1. **Estabilidade** — um NUIT ou BI pode, na vida real, ser corrigido (erro
   de digitação, duplicação) sem que isso deva obrigar a atualizar a chave
   em todas as tabelas relacionadas (`FUNCIONARIO_FILHO`,
   `FUNCIONARIO_CONTACTO`).
2. **Desempenho** — chaves numéricas simples são mais eficientes para o
   SGBD indexar e relacionar do que texto (`VARCHAR`/`CHAR`).

Isto não introduz nenhuma nova dependência transitiva: `NUIT` e `BI`
continuam a depender apenas de `id_funcionario` (são atributos-chave
alternativos, 1 para 1 com a chave primária).

## Conclusão da fase

Com estas quatro tabelas de referência extraídas, `FUNCIONARIO` só guarda
agora dados que descrevem diretamente o próprio funcionário — todo o resto
passou a ser consultado através de uma chave estrangeira. Já não existem
dependências transitivas no esquema.
