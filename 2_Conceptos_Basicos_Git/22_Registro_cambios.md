## __Conceptos Básicos de Git: Registro de Cambios en el repositorio__

### __Registro de cambios  en el repositorios__
En este punto, debe tener un repositorio Git de buena fe en su máquina local, y un pago o copia de trabajo de todos sus archivos frente a usted. Por lo general, querrá comenzar a realizar cambios y enviar instantáneas de esos cambios a su repositorio cada vez que el proyecto alcance el estado que desea registrar.

Recuerde que cada archivo en su directorio de trabajo puede estar en uno de dos estados: __con seguimiento__ o __sin seguimiento__ . Los archivos rastreados son archivos que estaban en la última instantánea; pueden ser no modificados, modificados o escenificados. En resumen, los archivos rastreados son archivos que Git conoce.

Los archivos sin seguimiento son todo lo demás: cualquier archivo en su directorio de trabajo que no estaba en su última instantánea y no está en su área de preparación. Cuando clones un repositorio por primera vez, todos tus archivos serán rastreados y no modificados porque Git simplemente los revisó y no has editado nada.

A medida que edita archivos, Git los ve como modificados, porque los ha cambiado desde su última confirmación. Mientras trabaja, organiza selectivamente estos archivos modificados y luego confirma todos los cambios por etapas, y el ciclo se repite.

![El ciclo de vida del estado de sus archivos](./img/ciclo_vida.png)

Figura 8. El ciclo de vida del estado de sus archivos

### __Comprobación del estado de sus archivos__
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
que un archivo que se rastrea se ha modificado en el directorio de trabajo pero aún no se ha organizado. Para prepararlo, ejecute el comando, es un comando multipropósito: se utiliza para comemzar a rastrear nuevos archivos, para organizar archivos y para hacer otras cosas como marcar archivos en conflictos de combinación como resueltos. Puede ser útil pensar en ello más como "añadir precisamente este contenido a la siguiente confirmación" en lugar de "añadir este archivo proyecto". Vamos a ejecutar `git add` ahora para preparar el archivo `CONTRIBUTING.md` y, a continuación, ejecutar `git status` de nuevo: 


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

<!-- |Header | Derecha | Izquierda |
| ------ | ------- | ------- |
| celda | celda 1 | celda 2 | -->



| Tip | GitHub mantiene una lista bastante completa de buenos ejemplos de archivos `.gitignore` para docenas de proyectos e idioma en [https://github.com/github/gitignore](https://github.com/github/gitignore) si desea un punto de partida para su proyecto.|
| ------ | ------- |
| Nota | En el caso simple, un repositorio puede tener un solo archivo `.gitignore` en su directorio raíz, que se aplica de forma recursiva a todo el repositorio. Sin embargo, también es posible tener archivos `.gitignore` adicionales en subdirectorios. Las reglas de estos archivos `.gitignore` anidados se apican solo a los archivos del directorio donde se encuentran. El repositorio de origen del kernel de Linux tiene 206 archivos `.gitignore`.    Está más allá del alcance del libro entrar en los detalles de varios archivos `.gitignore`, consulte los `man gitignore` detalles. | 

### __Ver sus cambios por etapas y sin etapas__

Si el comando `git status` es demasiado vago para usted, desea saber exactemente qué cambió, no solo qué archivos se cambiaron, puede usar el comando  `git diff`. Lo cubriremos `git diff` con más detalles más adelante, pero probablemente lo usará con más frecuencia para responder estas dos preguntas: ¿Qué ha cambiado, pero aún no ha organizado? ¿Y qué has montado que estás a punto de hacer commit? Aunque responde `git status` a esas preguntas de manera muy general al enumerar los nombres de los archivos, le muestra `git diff` las líneas exactas agregadas y eliminadas, el parche, por así decirlo.

Digamos que editas y preparas el archivo `README` nuevamente y luego lo editas el archivo `CONTRIBUYING.md` sin prepararlo. Si ejecuta su comando `git status`, una vez más verá algo como esto:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
        (use "git reset HEAD <file>..." to unstage)

        modified:   README

    Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

Para ver lo que ha cambiado pero que aún no ha preparado, escriba `git diff` sin otros argumentos:

    $ git diff
    diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
    index 8ebb991..643e24f 100644
    --- a/CONTRIBUTING.md
    +++ b/CONTRIBUTING.md
    @@ -65,7 +65,8 @@ branch directly, things can get messy.
        Please include a nice description of your changes when you submit your PR;
        if we have to read the whole diff to figure out why you're contributing
        in the first place, you're less likely to get feedback and have your change
    -merged in.
    +merged in. Also, split your changes into comprehensive chunks if your patch is
    +longer than a dozen lines.

     If you are starting to work on a particular area, feel free to submit a PR
     that highlights your work in progress (and note in the PR title that it's

Ese comando compara lo que está en su directorio de trabajo con lo que está en su área de preparación. El resultado le indica los cambios que ha realizado y que aún no ha realizado.

Si quieres ver lo que has organizado ques se incluirá en tu próxima confirmación, puedes usar `git diff --staged`. Este comando compara sus cambios por etapas con su última confirmación.

    $ git diff --staged
    diff --git a/README b/README
    new file mode 100644
    index 0000000..03902a1
    --- /dev/null
    +++ b/README
    @@ -0,0 +1 @@
    +My Project

Es importante tener en cuanta que por sí solo `git diff` no muestra todos los cambios realizados desde su última confirmación. solo los cambios que aún no están preparados. Si ha organizado todo sus cambios, `git diff` no obtendrá resultados.

Para otro ejemplo, si organiza el archivo `CONTRIBUYING.md` y luego lo edita, puede usar `git diff` para ver los cambios en los archivos que están organizados y los cambios  que no lo están. Si nuestro entorno se ve así:

    $ git add CONTRIBUTING.md
    $ echo '# test line' >> CONTRIBUTING.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

     modified:   CONTRIBUTING.md

    Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

Ahora puede usar `git diff` para ver lo que aún no esta en escena:

    $ git diff
    diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
    index 643e24f..87f08c8 100644
    --- a/CONTRIBUTING.md
    +++ b/CONTRIBUTING.md
    @@ -119,3 +119,4 @@ at the
     ## Starter Projects

     See our [projects list](https://github.com/libgit2/libgit2/blob/development/PROJECTS.md).
    +# test line

y `git diff --cached` para ver lo que has puesto en escena hasta ahora (`--staged` y `--cached` son sinónimos):

    $ git diff --cached
    diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
    index 8ebb991..643e24f 100644
    --- a/CONTRIBUTING.md
    +++ b/CONTRIBUTING.md
    @@ -65,7 +65,8 @@ branch directly, things can get messy.
     Please include a nice description of your changes when you submit your PR;
     if we have to read the whole diff to figure out why you're contributing
     in the first place, you're less likely to get feedback and have your change
    -merged in.
    +merged in. Also, split your changes into comprehensive chunks if your patch is
    +longer than a dozen lines.

     If you are starting to work on a particular area, feel free to submit a PR
     that highlights your work in progress (and note in the PR title that it's

|Nota   | Git Diff en una herramienta externa,  Continuaremos usando el comando `git diff` de varias formas a lo largo del resto del libro. Hay otra forma de ver estas diferencias si prefiere un programa de visualización de diferencias gráfico o externo. Si ejecuta `git difftool` en lugar de `git diff`, puede ver cualquiera de estas diferencias en software como emerge, vimdiff y muchos más (incluidos los productos comerciales). Ejecute `git difftool --tool-help` para ver que hay disponible en su sistema.|
| ------ | ------- |


### __Confirmación de sus cambios__
Ahora que su área de preparación está configurada de la manera que desea, puede confirmar sus cambios. Recuerde que cualquier cosa que aún no esté preparada, cualquier archivo que haya creado o modificado y que no haya ejecutado `git add` desde que lo editó, no entrará en esta confirmación. Permanecerán como archivos modificados en su disco, En este caso, digamos que la última vez que ejecutó `git status`, vio que todo estaba preparado, por lo que está listo para confirmas sus cambios. La forma más sencilla de comprometerse es escribir `git commit`:

    $ git commit

Al hacerlo, se inicia el editor de su elección.

| Nota    | Esto lo establece la variable `EDITOR` de entorno de su shell, generalmente vim o emacs, aunque puede configurarlo con lo que quiera usando el comando `git config --global core.editor` como vio en [Getting Started](https://git-scm.com/book/en/v2/ch00/ch01-getting-started)|
| ------ | ---------- |


El editor muestra el siguiente texto (este ejemplo es una pantalla de Vim)

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # On branch master
    # Your branch is up-to-date with 'origin/master'.
    #
    # Changes to be committed:
    #	new file:   README
    #	modified:   CONTRIBUTING.md
    #
    ~
    ~
    ~
    ".git/COMMIT_EDITMSG" 9L, 283C


Puedes ver que el mensaje de confirmación predeterminado contiene la última salida del comando `git status` comentado y una línea vacía en la parte superior. Puede eliminar estos comentarios y escribir su mensaje de confirmacion, o puede dejarlos allí para ayudarlo a recordar lo que está haciendo commit.


| Nota | Para un recordatorio aún más explícito de lo que ha modificado, puede pasar la opción `-v` a `git commit`. Hacerlo también coloca la diferencia de su cambio en el editor para que pueda ver exactamente qué cambios está realizando|
| ------ | ------- |


Cuando sales del editor. Git crea tu confirmación con ese mensaje de confirmación (con los comentarios y la diferencia eliminados)

Alternativamente, puede escribir su mensaje de confirmación en línea con el comando `commit` especificando después de una bandera `-m`, como esta:

    $ git commit -m "Story 182: fix benchmarks for speed"
    [master 463dc4f] Story 182: fix benchmarks for speed
     2 files changed, 2 insertions(+)
     create mode 100644 README

¡Ahora ha creado su primera confirmación! Puede ver que la confirmación le ha dado resultados sobre sí misma: en qué rama se comprometió (`master`), qué suma de comprobación SHA-1 tiene la confirmación (`463dc4f`), cuántos archivos se cambiaron y estadísticas sobre las líneas agregadas y eliminadas en la confirmación.

### __Saltarse el área de espera__

Aunque puede ser increíblemente útil para crear para crear confirmaciones exactamente como las desea, el área de preparación a veces es un poco más compleja de lo que necesita en su flujo de trabajo. Si desea omitir el área de preparación, Git proporciona un atajo simple. Agregar la opción `-a` al comando `git commit` hace que Git ponga en escena automáticamente cada archivo que ya está rastreado antes de realizar la confirmación, lo que permite omitir la parte `git add`.

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   CONTRIBUTING.md

    no changes added to commit (use "git add" and/or "git commit -a")
    $ git commit -a -m 'Add new benchmarks'
    [master 83e38c7] Add new benchmarks
     1 file changed, 5 insertions(+), 0 deletions(-)

Observer cómo no tiene que ejecutar `git add` en el archivo `CONTRIBUTING.md` en este caso antes de confirmar. Eso es porque mla bandera `-a` incluye todos los archivos modificados. Esto es convenientem pero tenga cuidado; a veces, esta bandera hará que incluya cambios no deseados

### __Eliminar archivos__
Para eliminar un archivo de Git, debe eliminarlo de sus archivos rastreados (más exactamente, eliminarlo de su área de ensayo) y luego confirmar. El comando `git rm` hace eso y también elimina el archivo de su directorio de trabajo para que no lo vea como un archivo sin seguimiento la próxima vez.

Si simplemente elimina el archivo de su directorio de trabajo, aparecerá en el área "Cambios no preparados para confirmar" (es decir, no preparados) de su salida `git status`.

    $ rm PROJECTS.md
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes not staged for commit:
     (use "git add/rm <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

            deleted:    PROJECTS.md

    no changes added to commit (use "git add" and/or "git commit -a")

Luego, si lo ejecuta `git rm`, organiza la eliminación del archivo.

    $ git rm PROJECTS.md
    rm 'PROJECTS.md'
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

        deleted:    PROJECTS.md

La próxima vez que confirme, el archivo desaparecerá y ya no se realizará el seguimiento. Si modificó el archivo o ya lo había agregado al área de preparación, debe forzar la eliminación  con la opción `-f`. Esta es una función de seguridad para evitar la eliminación accidental de datos que aún no se han registrado en una instantánea y que no se pueden recuperar de Git.

otra cosa útil que puede querer hacer es mantener el archivo  en su árbol de trabajo per eliminarlo de su área de preparación. En otras palabras, es posible que desee mantener el archivo en su disco duro pero que Git ya no lo rastree. Esto es particularmente útil si olvidó si olvidó agregar algo a su archivo `.gitignore` y lo organizó accidentalmente, como un archivo de resgistro grande o un montón de archivos `.a` compilados. Para hacer esto, use la opción `--cached`:

    $ git rm --cached README

Puede pasar archivos, directorios y patrones de file-glob al comando `git rm`. Eso significa que puede hacer cosas como:

    $ git rm log/\*.log

Tenga en cuenta la barra diagonal inversa ( \ ) delante de `*`. Esto es necesario por Git hace su propia expansión de nombre de archivos además de la expansión de nombre de archivo de su shell. Este comando elimina todos los archivos que tienen la extensión `.log` en el directorio `log/` O puede hacer algo como esto:

    $ git r, \*~

Este comando elimina todos los archivos cuyos nombres terminan en `~`.

### __Mover archivos__

A diferencia de muchos otros sistemas VCS, Git no rastrea explícitamente el movimiento de archivos. Si cambia el nombre de un archivo en Git, no se almacenan metadatos en Git que le indiquen que cambió el nombre del archivo. Sin embargo, Git es bastante inteligente al darse cuenta de eso después de los hechos; nos ocuparemos de detectar el movimiento de archivos un poco más tarde.

Por lo tanto, es un poco confuso que Git tenga un comando `mv`. Si desea cambiar el nombre de un archivo en Git, puede ejecutar algo como:

    $ git mv file_from file_to

y funciona bien. De hecho, si ejecuta algo como esto y observa el estado, verá que Git lo considera un archivo renombrado:

    $ git mv README.md README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

        renamed:    README.md -> README

Sin embargo, es es equivalente a ejecutar algo como eto:

    $ mv README.md README
    $ git rm README.md
    $ git add README


Git se da cuenta de que es un cambio de nombre implícito, por lo que no importa si cambia el nombre de un archivo de esa manera o con el comando `mv`. La única diferencia real es que `git mv` es un comando en lugar de tres: es una función de conveniencia. Más importante aún, puede usar cualquier herramienta que desee para cambiar el nombre de un archivo y abordar el `add/rm` más tarde, antes de confirmar.