# 5. Quarta Forma Normal (4FN)

## Regra aplicada

Uma tabela em 3FN (e em BCNF) está em 4FN quando não contém duas ou mais
dependências multivaloradas independentes entre si. Por outras palavras:
factos multivalorados que não têm relação lógica entre si não podem
partilhar a mesma tabela.

## Onde é que esta regra se aplicaria mal, neste caso?

Os "filhos" e os "contactos telefónicos" de um funcionário são dois factos
**multivalorados e completamente independentes** um do outro:

- `NUIT →→ Nome_Filho` (dependência multivalorada: para cada NUIT existe um
  conjunto de nomes de filhos, independente de qualquer outra coisa)
- `NUIT →→ Numero_Celular` (o mesmo, para os números de telefone)

Se estes dois grupos tivessem sido colocados **na mesma tabela** — por
exemplo, uma tabela larga `FUNCIONARIO_FILHO_CONTACTO(NUIT, Nome_Filho,
Numero_Celular)`, mesmo já sem repetir os outros dados do funcionário —
seríamos obrigados a criar uma linha para **cada combinação possível** de
filho × número de telefone, só para representar dados que não têm nenhuma
relação real entre si. Por exemplo, para o funcionário Bernardo Machava
(3 filhos × 2 contactos), essa tabela teria **6 linhas** só para representar
3 filhos e 2 números — uma redundância artificial, e um convite a
inconsistências (que aconteceria se só um dos 6 pares fosse atualizado?).

## O que foi feito

Exatamente para evitar este problema, os dois grupos multivalorados foram
mantidos **desde a 1FN** em duas tabelas independentes, cada uma "toda
chave" (sem atributos não-chave), cada uma relacionada com `FUNCIONARIO`
através de uma dependência multivalorada trivial (1 grupo por tabela):

### `FUNCIONARIO_FILHO`
Chave: `(id_funcionario, Nome_Filho)`

| id_funcionario | Nome_Filho |
|---|---|
| 1 | Cátia Cossa |
| 2 | Nelson Machava |
| 2 | Ivete Machava |
| 2 | Suzana Machava |
| … | … |

### `FUNCIONARIO_CONTACTO`
Chave: `(id_funcionario, Numero_Celular)`

| id_funcionario | Numero_Celular |
|---|---|
| 1 | 841234567 |
| 1 | 821234567 |
| 2 | 845678901 |
| … | … |

Como cada uma destas tabelas representa **um único** facto multivalorado
sobre o funcionário (e não uma combinação de dois factos independentes),
cada uma delas satisfaz trivialmente a 4FN — a dependência multivalorada
`id_funcionario →→ Nome_Filho` (respetivamente `→→ Numero_Celular`) é a
**única** existente em cada tabela, logo não há duas dependências
multivaloradas independentes a coexistir na mesma relação.

## Conclusão da fase

O esquema resultante das fases anteriores (1FN a 3FN) já respeitava, na
prática, a 4FN — porque a separação de `Filhos` e `Contactos` em tabelas
distintas foi feita logo na 1FN, em vez de serem combinados numa única
tabela de detalhe. Esta fase serviu para **verificar e justificar
formalmente** essa decisão à luz da definição de 4FN, confirmando que o
modelo final não sofre da anomalia de "produto cartesiano" entre grupos
multivalorados independentes.
