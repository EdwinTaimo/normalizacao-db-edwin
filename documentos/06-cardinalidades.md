# 6. Cardinalidades dos relacionamentos

Esquema final (pós-4FN): 7 entidades — `FUNCIONARIO`, `FUNCIONARIO_FILHO`,
`FUNCIONARIO_CONTACTO`, `CARGO`, `FUNCAO`, `CIDADE`, `PROVINCIA`.

| Relacionamento | Cardinalidade | Justificação |
|---|---|---|
| `CARGO` — `FUNCIONARIO` | **1:N** | Um cargo (ex.: `C01` Técnico de Informática) pode ser ocupado por vários funcionários (neste conjunto de dados, Amélia Cossa e Ivete Chirindza); cada funcionário tem, neste modelo, exatamente um cargo. |
| `FUNCAO` — `FUNCIONARIO` | **1:N** | Uma função pode agrupar vários funcionários (ex.: `F01` tem Amélia e Ivete); cada funcionário pertence a exatamente uma função. |
| `PROVINCIA` — `CIDADE` | **1:N** | Uma província tem várias cidades (ex.: "Sofala" → Beira; poderia ter mais); cada cidade pertence a exatamente uma província. |
| `CIDADE` — `FUNCIONARIO` | **1:N** | Uma cidade pode ser morada de vários funcionários (ex.: "Maputo" tem 4 funcionários); cada funcionário mora em exatamente uma cidade. |
| `FUNCIONARIO` — `FUNCIONARIO_FILHO` | **1:N** | Um funcionário pode ter 0, 1, 2, 3 ou mais filhos registados; cada registo de filho pertence a exatamente um funcionário. |
| `FUNCIONARIO` — `FUNCIONARIO_CONTACTO` | **1:N** | Um funcionário pode ter 0 a N números de telefone; cada contacto pertence a exatamente um funcionário. |

## Porque não existe nenhuma relação N:M neste esquema

Neste conjunto de dados, cada funcionário está associado a **exatamente um**
cargo e a **exatamente uma** função no momento atual (não existe um
histórico de vários cargos por funcionário nem um cargo partilhado
simultaneamente por várias funções). Por isso, `Cod_Cargo` e `Cod_Funcao`
entram em `FUNCIONARIO` como chaves estrangeiras simples, e todos os
relacionamentos do esquema resultam em cardinalidade 1:N (nunca N:M).

> **Nota para extensão futura:** se a empresa precisasse de guardar o
> **histórico** de cargos de um funcionário ao longo do tempo (por exemplo,
> promoções), a relação `FUNCIONARIO` — `CARGO` passaria a ser **N:M**,
> exigindo uma tabela associativa `FUNCIONARIO_CARGO_HISTORICO
> (id_funcionario, cod_cargo, data_inicio, data_fim)`. Esse cenário não se
> aplica aos dados fornecidos, mas fica registado como possível evolução do
> modelo.
