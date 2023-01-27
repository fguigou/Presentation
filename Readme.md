

# Forms Query Language

## Code Examples

### Create Form

```sql
create form Person(
    name text not null,
    age number
)
create form Project(
    title text unique,
    Person references Person
)
create form Employees(
    Person references Person.(name,age),
    salary number
)
create form Store(
    address text,
    Employee references 1..many Employees
)
```

## Representation
### FQL_FORMS 

| ID | NAME           |TABLE_NAME       |
| -- | --             | --              |
| 0  | Person         | Person          |
| 1  | Project        | Project         |
| 2  | Employees      | Employees       |
| 3  | Store          | Store           |
| 4  | Store_Employees| Store_Employees |


### FQL_TYPES
| ID | NAME     |
| -- | --       |
| 0  | REFERENCE|
| 1  | NUMBER   | 
| 2  | TEXT     |
| 3  | BOOLEAN  |
| 4  | DATE     |
| 5  | TIME     |
| 6  | FILE     |
| 7  | IMAGE    |

### FQL_DATA

| ID | LABEL   |COLUMN_NAME|FORM_ID|TYPE_ID|DATA_ORDER|UNIQUE_DATA|NULLABLE|VISIBLE|DATA_ID|FULL_REF|UNIQUE_REF|TOTALLY|MIN|MAX|
|--  |--       |--         |--     |--     |--        |--         |--      |--     |--     |--      |--        |--     |-- |-- |
|0   |name     |name       |0      |2      |--        |false      |false   |true   |--     |--      |--        |--     |-- |-- |
|1   |age      |age        |0      |1      |--        |false      |true    |true   |--     |--      |--        |--     |-- |-- |
|2   |title    |title      |1      |2      |--        |true       |true    |true   |--     |--      |--        |--     |-- |-- |
|3   |Person   |Person     |1      |0      |--        |false      |true    |true   |--     |true    |--        |--     |-- |-- |
|4   |Person   |Person     |2      |0      |--        |false      |true    |true   |0      |false   |--        |--     |-- |-- |
|5   |Person   |Person     |2      |0      |--        |false      |true    |true   |1      |false   |--        |--     |-- |-- |
|6   |salary   |salary     |2      |1      |--        |false      |true    |true   |--     |--      |--        |--     |-- |-- |
|7   |address  |address    |3      |2      |--        |false      |true    |true   |--     |--      |--        |--     |-- |-- |
|8   |Store    |Store      |4      |0      |--        |false      |true    |true   |--     |true    |--        |--     |-- |-- |
|9   |Employee |Employee   |4      |0      |--        |false      |true    |true   |--     |true    |--        |--     |-- |-- |

#### Preguntas:
- En qué se diferencia LABEL de COLUMN_NAME? Lo que yo creo es que tiene que ver con los campos a los que se referencia
- Cual es el significado de DATA_ORDER? Lo que creo es que representa el orden con el cual se muestran los datos. En caso de que sea así, que instrucción puede variar ese orden?
- Los campos UNIQUE_DATA y UNIQUE_REF son iguales para las referencias?, en que se diferencian?
- Cuando un campo no es VISIBLE?, tengo claro que dentro de las referencias si referencio a Person.(name,age), esos campos son visibles Para Employees, pero tengo que agregar los restantes campos de Person(en caso de que existiesen) como no VISIBLE?
- Cuando referencio a todo un formulario el campo DATA_ID se mantiene vacío?
- Las columnas MIN y MAX tienen que ver con rangos? 

### Person
| fql_id | name |age |
| --     | --   | -- |
| --     | --   | -- |

### Project
| fql_id | title |Person |
| --     | --    | --    |
| --     | --    | --    |


### Employees
| fql_id | Person(name) |Person(age) | salary |
| --     | --           | --         |--      |
| --     | --           | --         |--      |


### Store
| fql_id | address |
| --     | --      |
| --     | --      | 


### Store_Employees
| fql_id | Store |Employee |
| --     | --    | --      |
| --     | --    | --      |
