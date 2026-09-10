# 3. Segunda Forma Normal (2FN)

## Regra aplicada

Uma tabela em 1FN está em 2FN quando todos os atributos não-chave dependem
da **chave inteira**, e não apenas de uma parte dela (isto só é um problema
possível quando a chave é composta).

## Onde é que existe uma dependência parcial aqui?

`FUNCIONARIO_1FN` já usa o `NUIT` (uma chave simples) como identificador —
por si só, essa tabela não tem uma chave composta, pelo que não sofre
diretamente de dependência parcial.

O caso claro de dependência parcial aparece se pensarmos em como a 1FN teria
sido feita "à letra", **sem separar logo os filhos/contactos em tabelas
próprias**: manter tudo numa única tabela larga, com uma linha por cada
`(funcionário, filho)`, teria uma chave composta `(NUIT, Nome_Filho)` — e
**todos os outros atributos** (`Nome`, `Data Nasc.`, `Cargo`, `Endereço`,
etc.) dependeriam apenas do `NUIT` (parte da chave), nunca do `Nome_Filho`.
O mesmo aconteceria com uma tabela larga `(NUIT, Numero_Celular, ...)`.

**A 2FN resolve exatamente isto:** ao separar os dados do funcionário
(dependentes só de `NUIT`) dos dados dos filhos e dos contactos (dependentes
da chave composta completa), já garantimos, desde a fase de 1FN deste
trabalho, que:

- `FUNCIONARIO_1FN(NUIT, Nome, Data_Nasc, BI, Email, Endereço, Cidade,
  Província, País, Cargo, Cód_Cargo, Função, Cód_Função, Posto_Trabalho,
  Data_Admissão)` — chave simples `NUIT`, sem dependência parcial possível.
- `FUNCIONARIO_FILHO(NUIT, Nome_Filho)` — a chave é `(NUIT, Nome_Filho)` e
  **não existe nenhum atributo não-chave** nesta tabela (é uma relação
  "toda chave"), logo não há nada que possa depender parcialmente dela.
- `FUNCIONARIO_CONTACTO(NUIT, Numero_Celular)` — o mesmo raciocínio.

## Conclusão da fase

Como a separação dos grupos repetitivos já foi feita corretamente na 1FN
(cada grupo multivalorado na sua própria tabela, com a chave do funcionário
como parte da chave composta), **não restam dependências parciais** neste
esquema. As três tabelas resultantes da 1FN já satisfazem também a 2FN.

Nenhuma tabela nova foi criada nesta fase — o trabalho de identificar e
evitar a dependência parcial já tinha sido feito ao construir corretamente a
1FN. O que muda a partir daqui (3FN) é outro tipo de dependência:
**a transitiva**, presente em `FUNCIONARIO_1FN` através de `Cód_Cargo`,
`Cód_Função` e `Cidade`.
