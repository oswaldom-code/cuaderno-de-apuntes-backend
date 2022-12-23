En general, para crear enlaces utilizamos el comando **ln** y la opción **-s** para especificar enlaces simbólicos. 

ln -s [target file] [Symbolic filename]

El comando ln en Linux crea enlaces entre archivos fuente y directorios.

-   **-s**: el comando para enlaces simbólicos.
-   [target file]: nombre del archivo existente para el cual estás creando el enlace
-   [Symbolic filename]: nombre del enlace simbólico.

Los enlaces creados se pueden verificar por listado de directorio utilizando el comando de lista detallada:

ls -l

![Salida de propiedad del enlace simbólico de Linux en Ubuntu](https://www.hostinger.com/tutorials/wp-content/uploads/sites/2/2019/02/linux-symbolic-link-owenership-output.png)

Sin embargo, si no especificas el [Symbolic filename], el comando creará automáticamente un nuevo enlace en el directorio existente.

### Crear enlace simbólico en Linux para carpetas

Crear enlaces simbólicos para carpetas tampoco es difícil. El comando utilizado para crear el enlace simbólico de carpeta es:

ln -s [Specific file/directory] [symlink name]

Por ejemplo, para vincular el directorio **/user/local/downloads/logo** a la carpeta **/devisers**, usa el siguiente comando:

ln -s /user/local/downloads/logo /devisers

Una vez que se crea un enlace simbólico y se adjunta a la carpeta **/devisers**, te llevará a **/user/local/downloads/logo**. Cuando el usuario cambia el directorio **– cd** **–** a **/devisers**, el sistema cambiará automáticamente al archivo específico y lo escribirá en el directorio de comandos.

Las opciones de enlace simbólico se denominan switches de línea de comando. Aquí están los más comunes y sus descripciones:

**Switch de comando**

**Descripción**

-backup[=CONTROL]

copia de seguridad de cada archivo de destino existente

-d, -F, –directory

el superusuario puede intentar un enlace duro

-f, –force

se elimina el archivo de destino existente

-I, –interactive

preguntar antes de eliminar archivos de destino

-L, –logical

objetivos de deferencia que son enlaces simbólicos

-n, –non-dereference

los enlaces simbólicos al directorio se tratan como archivos

-P, –physical

convierte enlaces duros directamente a enlaces simbólicos

-r, –relative

crea enlaces simbólicos relativos a la ubicación del enlace

-s, -symbol

hacer enlaces simbólicos en lugar de enlaces duros

-S, –suffix=SUFFIX

anula el sufijo de copia de seguridad habitual

-v, –verbose

imprime el nombre de cada archivo vinculado

## ¿Cómo cambiar o eliminar un enlace simbólico en Linux?

Puedes eliminar los enlaces existentes adjuntos a archivos o directorios mediante el comando unlink o rm. Así es como puedes hacerlo con el comando unlink:

unlink [symlink to remove]

Eliminar el enlace simbólico usando el comando rm es similar al comando unlink como se ve en seguida:

rm [symlink name]

Por ejemplo:

rm simpleText