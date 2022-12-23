
Indice

[[#Introducción]]
1) [[#Modelos]]
	1.1 Crear modelos
	1.2 Renombrar modelos
	1.3 Agregar columnas a un modelo
	1.4 Eliminar columnas a un modelo
	1.5 Reorganizar las columnas de un modelo
	1.6 Agregar claves foráneas a un modelo
	1.7 [[#Eliminar modelos]]
2) [[#Migraciones]]
	- [[#Crear migraciones]]
	- [[#Revertir migraciones]]

# Introducción

Active Record es una librería de Ruby que forma parte del framework de aplicaciones web Rails. Se encarga de manejar la persistencia de datos en la base de datos, y proporciona una capa de abstracción para acceder y manipular los datos de la base de datos de manera sencilla y consistente.

Una de las principales ventajas de Active Record es que permite definir modelos de datos en forma de clases de Ruby, lo que hace que sea muy fácil de usar y entender para los desarrolladores de Ruby. Cada modelo de Active Record corresponde a una tabla de la base de datos, y cada instancia del modelo representa una fila de la tabla.

Active Record proporciona una serie de métodos y macros para definir y gestionar las relaciones entre los modelos, así como para validar y transformar los datos antes de ser guardados en la base de datos. También incluye una serie de métodos de consulta para acceder y filtrar los datos de la base de datos de manera sencilla.

En resumen, Active Record es una herramienta muy útil para manejar la persistencia de datos en aplicaciones web basadas en Rails, ya que proporciona una capa de abstracción sencilla y potente para acceder y manipular los datos de la base de datos.






# Migraciones

En el marco de trabajo de Rails, una migración es una clase especial que se utiliza para modificar la estructura de la base de datos de una aplicación Rails. Por ejemplo, puedes utilizar una migración para crear una nueva tabla en la base de datos, agregar una columna a una tabla existente, o eliminar una columna de una tabla. Las migraciones son una parte importante del proceso de desarrollo de una aplicación Rails, ya que te permiten modificar la estructura de la base de datos de manera controlada y documentada. 

	
## Crear migraciones

Para crear una migración en Rails, puedes utilizar el comando `rails generate migration` seguido del nombre de la migración y una descripción de lo que hará la migración. Por ejemplo:

```sh
rails generate migration AddEmailToUsers email:string
```

Esto creará una nueva clase de migración que agregará una columna `email` de tipo `string` a la tabla `users`. Una vez creada la migración, puedes ejecutarla con el comando `rails db:migrate`. Esto aplicará los cambios de la migración a la base de datos, modificando su estructura de acuerdo a lo especificado en la migración.

Es importante tener en cuenta que las migraciones son una herramienta muy poderosa y deben utilizarse con cuidado. Una vez que se ejecutan, no pueden deshacerse fácilmente, por lo que es importante asegurarse de que las migraciones sean precisas y estén bien probadas antes de aplicarlas a la base de datos en producción.

## Revertir migraciones

Para revertir una migración, puedes utilizar el siguiente comando:

```sh
rails db:migrate:down VERSION=20221020102047
```

Donde `20221020102047` representa el identificador numérico de la migración que se desea eliminar

**Nota**: Algunas migraciones no son reversibles.


# Modelos

## Eliminar modelos
	
Para eliminar un modelo se debe correr el siguiente comando:

```sh
rails destroy model donor_to_signatures
```

Este comando eliminará todos los archivos relacionados con ese modelo (controladores, archivos de test, migraciones y el modelo)

Seguramente, también se desee eliminar la tabla de la base de datos, para ello es necesario crear una nueva migración y en el archivo generado agregar algo similar a lo siguiente:


```Ruby
class DeleteTransactionOptionsModel < ActiveRecord::Migration[7.0]
	def up
		drop_table :transaction_options
	end

	def down
		raise ActiveRecord::IrreversibleMigration
	end
end
```






Renombrar una tabla:
	rename_table :status_transactions, :transactions_status

Renombrar columna:
	rename_column :table_name, :old_column, :new_column

Agregar columna:
	add_column :table_name, :column_name, :type

Eliminar un indice:
remove_index :completions, name: "index_completions_on_survey_id_and_user_id"
Resolver problema de casteo de tipo de dato: HINT:  Specify a USING expression to perform the conversion.
		def down
			change_column :brokers, :dtc_number, "integer USING CAST(dtc_number AS integer)"
		end


### Relación entre modelos

#####  1) Uno a Uno

`belongs_to` es una relación de asociación en Rails que se utiliza para establecer una conexión entre dos modelos de base de datos. Por ejemplo, si tienes un modelo de `Person` y otro modelo de `Company`, puedes usar `belongs_to` para establecer una relación uno-a-uno entre ellos de la siguiente manera:

 ```Ruby
	class Person < ActiveRecord::Base 
		 belongs_to :company 
	end 
	class Company < ActiveRecord::Base 
		has_one :person 
	end
```


Esto significa que cada persona pertenece a una empresa y cada empresa tiene una persona asociada a ella. Esta relación se reflejará en la base de datos mediante la creación de una columna `company_id` en la tabla de `Person` que hace referencia a la tabla de `Company`.

Puedes usar `belongs_to` para establecer relaciones uno-a-uno, uno-a-muchos y muchos-a-muchos entre distintos modelos de datos en tu aplicación de Rails.

##### 2) Muchos a Muchos

Para establecer una relación muchos-a-muchos entre dos modelos de datos en Rails, puedes usar la asociación `has_and_belongs_to_many`. Esta asociación se usa para establecer una relación muchos-a-muchos entre dos modelos de datos, lo que significa que cada uno de los modelos puede tener muchos registros relacionados con el otro modelo.

Por ejemplo, si tienes dos modelos, `Person` y `Group`, y quieres establecer una relación muchos-a-muchos entre ellos, puedes hacerlo de la siguiente manera:

```Ruby
class Person < ActiveRecord::Base
  has_and_belongs_to_many :groups
end

class Group < ActiveRecord::Base
  has_and_belongs_to_many :people
end
```

##### 3) Uno a muchos

Para establecer una relación uno-a-muchos entre dos modelos de datos en Rails, puedes usar la asociación `has_many`. Esta asociación se usa para establecer una relación uno-a-muchos entre dos modelos de datos, lo que significa que un modelo tiene muchos registros relacionados con el otro modelo.

Por ejemplo, si tienes dos modelos, `Person` y `Post`, y quieres establecer una relación uno-a-muchos entre ellos, puedes hacerlo de la siguiente manera:


```Ruby
class Person < ActiveRecord::Base
  has_many :posts
end

class Post < ActiveRecord::Base
  belongs_to :person
end

```

Esto creará una columna `person_id` en la tabla de `Post` que hace referencia a la tabla de `Person`. Esta columna se utilizará para almacenar la relación entre personas y publicaciones.

Puedes usar la asociación `has_many` para crear relaciones uno-a-muchos entre cualquier par de modelos de datos en tu aplicación de Rails. También puedes personalizar la tabla de relación y agregar atributos adicionales a ella si es necesario.