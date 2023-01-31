

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
    Person references 1 Person.(name,age),
    salary number
)
create form Store(
    address text,
    Employee references 0..many Employees
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

| ID | LABEL   |COLUMN_NAME |FORM_ID           |TYPE_ID|DATA_ORDER|UNIQUE_DATA|NULLABLE |VISIBLE|DATA_ID|FULL_REF|UNIQUE_REF|TOTALLY|MIN|MAX|
|--  |--       |--          |--                |--     |--        |--         |--       |--     |--     |--      |--        |--     |-- |-- |
|0   |name     |name        |0(Person)         |2(text)|--        |f(false)   |f(false) |t(true)|--     |--      |--        |--     |-- |-- |
|1   |age      |age         |0(Person)         |1(num) |--        |f(false)   |t(true)  |t(true)|--     |--      |--        |--     |-- |-- |
|2   |title    |title       |1(Project)        |2(text)|--        |t(true)    |t(true)  |t(true)|--     |--      |--        |--     |-- |-- |
|3   |Person   |Person(*)   |1(Project)        |0(ref) |--        |t(true)    |f(false) |t(true)|--     |t(true) |--        |--     |-- |-- |
|4   |Person   |Person(name)|2(Employee)       |0(ref) |--        |t(true)    |f(false) |t(true)|0(name)|f(false)|--        |--     |-- |-- |
|5   |Person   |Person(age) |2(Employee)       |0(ref) |--        |f(false)   |t(true)  |t(true)|1(age) |f(false)|--        |--     |-- |-- |
|6   |salary   |salary      |2(Employee)       |1(num) |--        |f(false)   |t(true)  |t(true)|--     |--      |--        |--     |-- |-- |
|7   |address  |address     |3(Store)          |2(text)|--        |f(false)   |t(true)  |t(true)|--     |--      |--        |--     |-- |-- |
|8   |Store    |Store(*)    |4(Store_Employees)|0(ref) |--        |t(true)    |f(false) |t(true)|--     |t(true) |--        |--     |-- |-- |
|9   |Employee |Employee(*) |4(Store_Employees)|0(ref) |--        |f(false)   |t(true)  |t(true)|--     |t(true) |--        |--     |-- |-- |

#### Preguntas:
- En qué se diferencia LABEL de COLUMN_NAME? Lo que yo creo es que tiene que ver con los campos a los que se referencia
R. El label es lo que el PresentationServer muestra en la UI. El column_name es el nombre de la columna en la tabla. 
   Esto sirve si en el futuro se hace un import de un esquema de BD ya existente.
   También sirve para casos donde hay un form Personas y otro Países y ambos tienen un dato "nombre". 

- Cual es el significado de DATA_ORDER? Lo que creo es que representa el orden con el cual se muestran los datos. En caso de que sea así, que instrucción puede variar ese orden?
R. Tal cual. Por ahora no hay un statement definido, no es una prioridad. 

- Los campos UNIQUE_DATA y UNIQUE_REF son iguales para las referencias?, Creo que el primero tiene que ver con la cardinalidad de la referencia y el segundo con RefConstraint que puede tomar los valores UNIQUE y TOTALLY UNIQUE.
R. UNIQUE_DATA crea una UNIQUE_KEY con ese (o esos) dato(s). Su ocurrencia no se repite en la tabla.
   UNIQUE_REF puede tener tres valores. Vacío, no es una referencia única. Y los otros dos indican si es una referencia única para ese caso (por ejemplo, un crítico de arte puede visitar varias obras, pero cada una de ellas una única vez ... aunque la misma obra puede ser visitada por varios críticos). Y en caso de ser TOTALLY UNIQUE, quiere decir que esa referencia es única para todos los casos (por ejemplo, si en el caso anterior, no más de un crítico puede visitar la misma obra de teatro). 

- Cuando un campo no es VISIBLE?, tengo claro que dentro de las referencias si referencio a Person.(name,age), esos campos son visibles Para Employees, pero tengo que agregar los restantes campos de Person(en caso de que existiesen) como no VISIBLE?
R. No, para nada. Ese era un caso especial, cuando hay referencias sobre referencias y algunos datos intermedios no son visibles. 

- Cuando referencio a todo un formulario el campo DATA_ID se mantiene vacío?
R. Si.

- Las columnas MIN y MAX tienen que ver con rangos? 
R. Si. Es el mínimo y máximo número de casos para la referencia. 

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
