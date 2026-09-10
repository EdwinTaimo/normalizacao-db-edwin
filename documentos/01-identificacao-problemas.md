# 1. Identificação dos problemas na tabela original (0FN)

**Autor:** Edwin Jerry Joaquim Taimo Gema — Licenciatura em Informática, Universidade Licungo

A tabela `Dados_Nao_Normalizados_Funcionarios` junta, numa única folha, cinco
"assuntos" diferentes: dados pessoais do funcionário, morada, dados
profissionais, filhos e contactos telefónicos. Isto está na origem de todos
os problemas identificados abaixo.

## 1.1 Grupos repetitivos (a razão de a tabela não estar sequer em 1FN)

| Grupo | Colunas | Problema |
|---|---|---|
| Filhos | `Filho 1`, `Filho 2`, `Filho 3` | Atributo multivalorado "espalhado" em 3 colunas. Um funcionário pode ter 0 a 3 filhos registados (ex.: Celina Sitoe tem 0; Bernardo Machava tem 3) — e se um dia existir um funcionário com 4 filhos, a estrutura não tem como o representar sem alterar o esquema da tabela. |
| Contactos telefónicos | `Celular 1`, `Celular 2`, `Celular 3` | O mesmo problema: um número variável de telefones (0 a 3) forçado em colunas fixas. |

Sempre que um funcionário tem menos de 3 filhos ou menos de 3 contactos, as
colunas a mais ficam vazias (NULL) — espaço desperdiçado e uma indicação
clara de que estes dados deviam estar numa tabela à parte, com uma linha por
cada filho/contacto.

## 1.2 Dados não atómicos

- **`Endereço`** mistura, num único campo de texto, o tipo de via (Av./Rua), o
  número de polícia e o bairro (ex.: *"Av. Julius Nyerere, n.º 245,
  Sommerschield"*). Não é possível, por exemplo, pesquisar "todos os
  funcionários do bairro Alto Maé" sem processar texto livre.
- **`Filho 1/2/3`** e **`Celular 1/2/3`**, além de serem grupos repetitivos,
  também não são atómicos no sentido em que a *posição* da coluna (1.ª, 2.ª
  ou 3.ª) não tem qualquer significado — dois funcionários com o mesmo
  conjunto de filhos, mas registados por ordens diferentes, seriam tratados
  como "diferentes" numa pesquisa direta à coluna.

*(O `Nome` do funcionário foi mantido como um único atributo atómico "nome
completo", tal como consta na folha original — a divisão em nome
próprio/apelido não foi pedida no enunciado nem influencia a normalização
pedida, pelo que não foi alterada.)*

## 1.3 Dependências parciais (em relação a uma chave composta)

Se, ao aplicar a 1FN, "desdobrarmos" os grupos repetitivos **dentro da mesma
tabela** (uma linha por cada filho, ou por cada contacto, repetindo os
restantes dados do funcionário), a chave deixa de poder ser só o `NUIT` — passa
a ser composta, por exemplo `(NUIT, Nome do Filho)`. Nessa tabela intermédia:

- `Nome`, `Data Nasc.`, `BI`, `Email`, `Endereço`, `Cidade`, `Província`,
  `País`, `Cargo`, `Cód. Cargo`, `Função`, `Cód. Função`, `Posto de
  Trabalho` e `Data Admissão` dependem **apenas do `NUIT`** — a parte
  "funcionário" da chave — e não do nome do filho.

Isto é uma **dependência parcial**: um atributo não-chave depende só de uma
parte da chave composta, não da chave inteira. É precisamente o problema que
a 2FN resolve (ver `documentos/03-segunda-forma-normal-2FN.md`).

## 1.4 Dependências transitivas

Depois de já termos uma chave simples (`NUIT`) para o funcionário, ainda
sobram dependências **indiretas**, em cadeia:

- `NUIT → Cód. Cargo → Cargo` — o nome do cargo não depende diretamente do
  funcionário, depende do código do cargo (o enunciado até chama a atenção
  para isto: *"poderá reparar que os mesmos códigos se repetem entre vários
  funcionários"* — por exemplo, o código `C01` = "Técnico de Informática"
  repete-se para Amélia Cossa e para Ivete Chirindza).
- `NUIT → Cód. Função → Função` — o mesmo problema para as funções.
- `NUIT → Cidade → Província → País` — a província de um funcionário é
  determinada pela cidade onde mora (ex.: toda a gente de "Beira" está em
  "Sofala"), e não diretamente pelo funcionário; o país é, por sua vez,
  determinado pela província.

Estas são **dependências transitivas**: um atributo não-chave (`Cargo`,
`Função`, `Província`, `País`) depende de outro atributo não-chave
(`Cód. Cargo`, `Cód. Função`, `Cidade`), que é quem realmente depende da
chave. A 3FN elimina este tipo de dependência.

## 1.5 Dependências multivaloradas

`Filhos` e `Contactos telefónicos` são dois factos **independentes** sobre o
mesmo funcionário: o número de filhos de alguém não tem qualquer relação com
o número de telefones que essa pessoa tem. Se, por engano, estes dois grupos
fossem colocados juntos numa única tabela de detalhe (ex.:
`(NUIT, Nome_Filho, Numero_Celular)`), seríamos obrigados a criar uma linha
para **cada combinação** de filho × telefone, o que introduz redundância e
uma relação falsa entre filhos e números de telefone (como se cada filho
"tivesse" um telemóvel específico). Este é o tipo de anomalia que a 4FN
resolve, mantendo os dois grupos em tabelas separadas.

## 1.6 Anomalias resultantes (inserção, atualização, remoção)

| Tipo de anomalia | Exemplo concreto na tabela original |
|---|---|
| **Inserção** | Não é possível registar um novo Cargo (ex.: "Analista de Dados", `C09`) enquanto não existir pelo menos um funcionário nesse cargo — o cargo só "existe" dentro da linha de um funcionário. |
| **Atualização** | Se o nome oficial do cargo `C01` mudasse de "Técnico de Informática" para "Técnico de TI", seria preciso atualizar a linha de **todos** os funcionários com esse código (Amélia Cossa e Ivete Chirindza), sob risco de a informação ficar inconsistente entre linhas. |
| **Remoção** | Se o único funcionário com o cargo `C09` fosse despedido e a sua linha apagada, perder-se-ia também a informação de que esse cargo existe — mesmo que a empresa continue a precisar dele no futuro. |
