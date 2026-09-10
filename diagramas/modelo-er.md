# Modelo Entidade-Relacionamento (MER) — esquema final (pós-4FN)

Ferramenta usada: **Mermaid** (sintaxe de diagrama em texto). Foi a
ferramenta escolhida porque o próprio GitHub interpreta e desenha este
diagrama automaticamente ao abrir este ficheiro no browser — não é preciso
exportar nenhuma imagem à parte para o repositório ficar com um diagrama
legível....

```mermaid
erDiagram
    PROVINCIA ||--o{ CIDADE : "tem"
    CIDADE ||--o{ FUNCIONARIO : "reside em"
    CARGO ||--o{ FUNCIONARIO : "ocupado por"
    FUNCAO ||--o{ FUNCIONARIO : "exercida por"
    FUNCIONARIO ||--o{ FUNCIONARIO_FILHO : "tem"
    FUNCIONARIO ||--o{ FUNCIONARIO_CONTACTO : "tem"

    PROVINCIA {
        string nome_provincia PK
        string pais
    }

    CIDADE {
        string nome_cidade PK
        string nome_provincia FK
    }

    CARGO {
        string cod_cargo PK
        string nome_cargo
    }

    FUNCAO {
        string cod_funcao PK
        string nome_funcao
    }

    FUNCIONARIO {
        int id_funcionario PK
        string nome
        date data_nasc
        string nuit UK
        string bi UK
        string email UK
        string endereco
        string nome_cidade FK
        string cod_cargo FK
        string cod_funcao FK
        string posto_trabalho
        date data_admissao
    }

    FUNCIONARIO_FILHO {
        int id_funcionario PK_FK
        string nome_filho PK
    }

    FUNCIONARIO_CONTACTO {
        int id_funcionario PK_FK
        string numero_celular PK
    }
```

## Leitura das cardinalidades no diagrama

A notação `||--o{` lê-se, da esquerda para a direita, como **"1 para 0 ou
muitos"** — por exemplo, `PROVINCIA ||--o{ CIDADE` quer dizer: uma província
tem zero ou muitas cidades; cada cidade pertence exatamente a uma província.
O detalhe de cada cardinalidade está justificado, entidade a entidade, em
`documentos/06-cardinalidades.md`.
