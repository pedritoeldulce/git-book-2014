## Conceptos Básicos de Git: Registro de Cambios en el repositorio

### Registro de cambios  en el repositorios
En este punto, debe tener un repositorio Git de buena fe en su máquina local, y un pago o copia de trabajo de todos sus archivos frente a usted. Por lo general, querrá comenzar a realizar cambios y enviar instantáneas de esos cambios a su repositorio cada vez que el proyecto alcance el estado que desea registrar.

Recuerde que cada archivo en su directorio de trabajo puede estar en uno de dos estados: __con seguimiento__ o __sin seguimiento__ . Los archivos rastreados son archivos que estaban en la última instantánea; pueden ser no modificados, modificados o escenificados. En resumen, los archivos rastreados son archivos que Git conoce.

Los archivos sin seguimiento son todo lo demás: cualquier archivo en su directorio de trabajo que no estaba en su última instantánea y no está en su área de preparación. Cuando clones un repositorio por primera vez, todos tus archivos serán rastreados y no modificados porque Git simplemente los revisó y no has editado nada.

A medida que edita archivos, Git los ve como modificados, porque los ha cambiado desde su última confirmación. Mientras trabaja, organiza selectivamente estos archivos modificados y luego confirma todos los cambios por etapas, y el ciclo se repite.

![El ciclo de vida del estado de sus archivos](./img/ciclo_vida.png)

Figura 8. El ciclo de vida del estado de sus archivos

### Comprobación del estado de sus archivos
La herramienta principal que utiliza para determinar qué archivos están en qué estado es el comando `git status`. 
Si ejecuta este comando directamente después de un clon, debería ver algo como esto:

    $ git status
    On your master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

Esto significa que tiene un directorio de trabajo limpio; en otras palabras, ninguno de sus archivos rastreados se modifica. 
Git tampoco ve ningún archivo sin seguimiento, o se enumerarían aquí. Finalmente, el comando le dice en qué rama se 
encuentra y le informa que no ha divergido de la misma rama en el servidor. Por ahora, esa rama es siempre `master`, que es la predeterminada; No te preocupes por eso aquí. [Git Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell#ch03-git-branching) repasará las ramas y referencias en detalle.

Digamos que agrega un nuevo archivo a su proyecto, un `README` archivo simple . Si el archivo no existía antes, y usted 
ejecuta `git status`, verá su archivo no rastreado así:

    $ echo 'My Project' > README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
        (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)

Puede ver que su nuevo archivo `README` no tiene seguimiento, porque está debajo del encabezado "Archivos sin 
seguimiento" en su salida de estado. Sin seguimiento básicamente significa que Git ve un archivo que no tenía en la 
instantánea anterior (commit); Git no comenzará a incluirlo en sus instantáneas de confirmación hasta que se lo indique 
explícitamente. Hace esto para que no comience accidentalmente a incluir archivos binarios generados u otros archivos que no quiso incluir. Desea comenzar a incluir `README`, así que comencemos a rastrear el archivo.

### __Seguimiento de nuevos archivos__
Para comenzar a rastrear un nuevo archivo, utilice el comando `git add`. Para comenzar a rastrear el `README` archivo, puede ejecutar esto:

    $ git add README

Si ejecuta su comando de estado nuevamente, puede ver que su `README` archivo ahora es rastreado y preparado para ser confirmado:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
        (use "git restore --staged <file>..." to unstage)

        new file:   README

Puede decir que está organizada porque está bajo el encabezado "Cambios para confirmar". Si se compromete en este punto, 
la versión del archivo en el momento en que se ejecutó `git add` es la que se incluirá en la instantánea histórica 
posterior. Puede recordar que cuando corrió `git init` antes, corrió `git add <files>` , eso fue para comenzar a rastrear 
archivos en su directorio. El comando `git add` toma un nombre de ruta para un archivo o un directorio; si es un directorio, el comando agrega todos los archivos en ese directorio de forma recursiva.

### __Almacenamiento de archivos modificados__

Cambiemos un archivo que ya fue rastreado. Si cambia un archivo rastreado anteriormente llamado `CONTRIBUYING.md` y luego 
ejecuta su comando `git status` nuevamente, obtendrá algo similar a esto:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
        (use "git reset HEAD <file>..." to unstage)

        new file:   READM
        
    Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md
        
El archivo `CONTRIBUYING.md` aparece en una sección llamada "Cambios no organizado para la confirmación", lo que significa 
que un archivo que se rastrea se ha modificado en el directorio de trabajo pero aún no se ha organizado. Para prepararlo, ejecute el comando, es un comando multipropósito: se utiliza para comemzar a rastrear nuevos archivos, para organizar archivos y para hacer otras cosas como marcar archivos en conflictos de combinación como resueltos. Puede ser útil pensar en ello más como "añadir precisamente este contenido a la siguiente confirmación" en lugar de "añadir este archivo proyecto". Vamos a ejecutar `git add` ahora para preparar el archivo `CONTRIBUTING.md`y, a continuación, ejecutar `git status`de nuevo: 


    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'
    Changes to be committed
        (use "git reset HEAD <files>..." to unstage)

        new file: README
        modified: CONTRIBUTING.md

Ambos archivos están organizados y entrarán en su próxima confirmación. En este punto, supongamos que recuerda un pequeño cambio que desea realizar en `CONTRIBUTING.md` antes de comitearlo. Lo abres de nuevo y haces cambio, y está listo para comprometerlo. Sin embargo, vamos a ejecutar una vez más `git status`:


    $ vim CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
        (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md

    Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

¿Qué demonios? Ahora `CONTRIBUTING.md` aparece en la lista como preparada y __no__ preparada. ¿Cómo es posible esto? Resulta que Git organiza un archivo exactamente como está cuando ejecuta el comando `git add`. Si se compromete ahora, la versión de `CONTRIBUTING.md` como era la útilma vez que ejecutó el comando `git add` es como entrará en la confirmación, no la versión del archivo como se en su directorio de trabajo cuando se ejecuta `git commit`. Si modifica un archivo despues de ejecutarlo y despues ejecuta `git add`, debe ejecutarlo nuevamente `git add` para organizarlo como la última versión del archivo.

    $ git add CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

        new file:   README
        modified:   CONTRIBUTING.md

### ___Estado Corto___

Si bien la salida `git status` es bastante completa, también es bantante prolija. Git también tiene un indicador de estado corto para que pueda ver sus cambios de una manera mas compacta. Si ejecuta `git status -s` o `git status --short` obtiene un resultado mucho más simplificado del comando:

    $ git status -s
        M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt

Los archivos nuevos que no se rastrean tiene un lado `?`, los archivos nuevos que se han agragado al área de preparación tienen un `A`, archivos modificados un `M`y otros. La salida tiene 2 columnas: La columna izquierda indica el estado del área de preparación y la columna de la derecha indica el estado del árbol de trabajo. Entonces, por ejemplo, en esa salida. el archivo `README` se modifica y está en etapas. Se modificó `Rakefile`, se organizó y luego se modificó nuevamente, por lo que hay cambios que se organizan y no se organizan.


### ___Archivos ignorados___

A menudo, tendrá una clase de archivos que no desea que Git agregue automaticamente o incluso que muestre como no rastreada. Estos son generalmente archivos generados automáticamente, como archivos de registro o archivos producidos por su sistema de compilación. En tales casos, puede crear un archivo `.gitignore` de ejemplo:a

    $ cat . gitignore
    *.[oa]
    *~

La primera línea le dice a Git que ignore cualquier archivo que termine en ".o" o ".a" - objetos y archivos de archivados que pueden ser el producto de construir su código. La segunda línea le dice a Git que ignore todos los archivos cuyos nombres terminan en tilde (~), que es utilizado por muchos editores de texto como Emacs para marcar archivos temporales. También  puede incluir  un directorio log, tmp o pod; documentación generada automaticamente; y así. Configurar un archivo `gitignore` para su nuevo repositorio antes de comenzar es generalmente una buena idea para que no confirme accidentalmente archivos que realmente no desea en su repositorio.

Las reglas para los patrones que puede poner en el archivo `.gitignore`  son las siguientes:

* La línea en blanco o las líneas que comienzan con __`#`__ se ignoran.
* Los patrones de globos estandar funcionan y se aplicarán de forma recursiva en todo el árbol de trabajo.
* Puede iniciar patrones con una barra diagonal (`/`) para evitar la recursidad.
* Puede finalizar patrones con una barra diagonal (`/`) para especificar el directorio.
* Puede negar un patrón comenzándolo con un signo de exclamación (`!`).


Los patrones globales son como expresiones regulares simplificadas que usan las corchetes. Un asterisco (`*`) coincide con cero o más caracteres; [abc] coincide con cualquier caracter dentro de los corchetes (en este caso a, b ó c); un signo de interrogación (`?`) coincide con un solo carácter; y los corchetes que encierran caracteres separados por un guión ([0-9]) coinciden con cualquier carácter entre ellos (en este caso del 0 hasta el 9). También puede usar dos asteriscos para unir directorios anidados; `a/**/` coincidiría con `a/z` `a/b/z` `a/b/c/z` y así sucesivamente.

Aquí hay otro archivo `.gitignore` de ejemplo:

    # ignora todos los archivos .a
    *.a

    # pero rastree lib.a, a pesar de que ignora los archivos .a anteriores.
    !lib.a

    # solo ignora el archivo TODO en el directorio actual, no subdir/TODO
    /TODO

    # Ignora todos los archivos en cualquier directorio llamado build
    build/

    # ignore doc/notes.txt pero no doc/server/arch.txt
    doc/*.txt

    # ignora todos los archivos .pdf en el directorio doc/ y cualquiera de sus subdirectorios.
    doc/**/*.pdf

