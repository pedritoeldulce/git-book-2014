## Conceptos Básicos de Git: Registro de Cambios en el repositorio

### Registro de cambios  en el repositorios
En este punto, debe tener un repositorio Git de buena fe en su máquina local, y un pago o copia de trabajo de todos sus archivos frente a usted. Por lo general, querrá comenzar a realizar cambios y enviar instantáneas de esos cambios a su repositorio cada vez que el proyecto alcance el estado que desea registrar.

Recuerde que cada archivo en su directorio de trabajo puede estar en uno de dos estados: con seguimiento o sin seguimiento . Los archivos rastreados son archivos que estaban en la última instantánea; pueden ser no modificados, modificados o escenificados. En resumen, los archivos rastreados son archivos que Git conoce.

Los archivos sin seguimiento son todo lo demás: cualquier archivo en su directorio de trabajo que no estaba en su última instantánea y no está en su área de preparación. Cuando clones un repositorio por primera vez, todos tus archivos serán rastreados y no modificados porque Git simplemente los revisó y no has editado nada.

A medida que edita archivos, Git los ve como modificados, porque los ha cambiado desde su última confirmación. Mientras trabaja, organiza selectivamente estos archivos modificados y luego confirma todos los cambios por etapas, y el ciclo se repite.

![El ciclo de vida del estado de sus archivos](./img/ciclo_vida.png)

Figura 8. El ciclo de vida del estado de sus archivos

### Comprobación del estado de sus archivos
La herramienta principal que utiliza para determinar qué archivos están en qué estado es el `git status` comando. Si ejecuta este comando directamente después de un clon, debería ver algo como esto:

    $ git status
    On your master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

Esto significa que tiene un directorio de trabajo limpio; en otras palabras, ninguno de sus archivos rastreados se modifica. Git tampoco ve ningún archivo sin seguimiento, o se enumerarían aquí. Finalmente, el comando le dice en qué rama se encuentra y le informa que no ha divergido de la misma rama en el servidor. Por ahora, esa rama es siempre `master`, que es la predeterminada; No te preocupes por eso aquí. [Git Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell#ch03-git-branching) repasará las ramas y referencias en detalle.

Digamos que agrega un nuevo archivo a su proyecto, un `README` archivo simple . Si el archivo no existía antes, y usted ejecuta `git status`, verá su archivo no rastreado así:

    $ echo 'My Project' > README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
        (use "git add <file>..." to include in what will be committed)

        README

    nothing added to commit but untracked files present (use "git add" to track)

Puede ver que su nuevo `README` archivo no tiene seguimiento, porque está debajo del encabezado "Archivos sin seguimiento" en su salida de estado. Sin seguimiento básicamente significa que Git ve un archivo que no tenía en la instantánea anterior (commit); Git no comenzará a incluirlo en sus instantáneas de confirmación hasta que se lo indique explícitamente. Hace esto para que no comience accidentalmente a incluir archivos binarios generados u otros archivos que no quiso incluir. Desea comenzar a incluir `README`, así que comencemos a rastrear el archivo.

### __Seguimiento de nuevos archivos__
Para comenzar a rastrear un nuevo archivo, utilice el comando `git add`. Para comenzar a rastrear el `README` archivo, puede ejecutar esto:

    $ git add README

Si ejecuta su comando de estado nuevamente, puede ver que su `README` rchivo ahora es rastreado y preparado para ser confirmado:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
        (use "git restore --staged <file>..." to unstage)

        new file:   README

Puede decir que está organizada porque está bajo el encabezado "Cambios para confirmar". Si se compromete en este punto, la versión del archivo en el momento en que se ejecutó `git add` es la que se incluirá en la instantánea histórica posterior. Puede recordar que cuando corrió `git init` antes, corrió `git add <files>` , eso fue para comenzar a rastrear archivos en su directorio. El `git add` comando toma un nombre de ruta para un archivo o un directorio; si es un directorio, el comando agrega todos los archivos en ese directorio de forma recursiva.

### __Almacenamiento de archivos modificados__