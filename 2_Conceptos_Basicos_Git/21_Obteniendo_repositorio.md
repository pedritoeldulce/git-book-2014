## __Conceptos básicos de Git: Cómo obtener un repositorio de Git__

Si solo puede leer un capítulo para comenzar en Git, este es el momento. En este capítulo cubre todos los comandos básicos que necesita hacer la gran mayoría de las cosas que eventualmente pasará su tiempo haciendo con Git. Al final del capítulo, debería poder configurar e inicializar un repositorio, comenzar y detener los archivos de seguimiento, y organizarlos y confirmar los cambios. También le mostraremos cómo configurar Git para ignorar ciertos archivos y patrones de archivos, cómo deshacer errores rápida y fácilmente, cómo explorar el historial de su proyecto y ver los cambios entre confirmaciones, y cómo empujar y extraer desde repositorios remotos.

### __Obteniendo un repositorio Git__ 

Por lo general, obtiene un repositorio de Git de una de dos maneras:

1. Puede tomar un directorio local que actualmente no está bajo control de versiones y convertirlo en un repositorio de Git, o
2. Puede clonar un repositorio Git existente desde otro lugar.

En cualquier caso, terminará con un repositorio Git en su máquina local, listo para trabajar.

## Inicializar un repositorio en un directorio existente.

Si tiene un directorio de proyecto que actualmente no está bajo control de versiones y desea comenzar a controlarlo con Git, primero debe ir al directorio de ese proyecto. Si nunca ha hecho esto, se ve un poco diferente según el sistema que esté ejecutando.

Para Linux:

    $ cd /home/user/my_project

Para macOS:

    $ cd /Users/user/my_project

Para Windows:

    $ cd C:/Users/user/my_proyect

y escriba: 

    $ git init

Esto crea un nuevo subdirectorio llamado `.git` que contiene todos los archivos de repositorio necesarios: un esqueleto de repositorio de Git. En este punto, todavía no se realiza un seguimiento de nada en su proyecto. Consulte [Git internals](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain#ch10-git-internals) para obtener más información sobre exactamente qué archivos están contenidos en el directorio `.git` que acaba de crear.

Si desea iniciar el control de versiones de los archivos existentes (a diferencia de un directorio vacío), probablemente debería comenzar a rastrear esos archivos y realizar una confirmación inicial. Puede lograrlo con algunos comandos `git add` que especifican los archivos que desea rastrear, seguidos de `git commit`:

    $ git add *.C
    $ git add LICENSE
    $ git commit -m "Initial project version"

Repasaremos lo que hacen estos comandos en un solo minuto, En este capítulo, tiene un repositorio Git con archivos rastreados y una confirmación inicial.

### Clonando un repositorio existente

Si desea obtener una copia de un repositorio Git existente, por ejemplo, un proyecto al que le gustaría contribuir, el comando que necesita es  `git clone`. Si está familizarizado con otros sistemas VCS como Subversión, notará que el comando es __"clone"__ y no __"checkout"__. Esta es una distinción importarte: en lugar de obtener solo una copia de trabajo, Git recibe una copia completa de casi todos los datos que tiene el servidor. Cada versión de cada archivo para el historial del proyecto se despliega de forma predeterminada cuando se ejecuta `git clone`. De hecho, si el disco de su servidor se corrompe, a menudo puede usar casi cualquier de los clones en cualquier cliente para configurar el servidor al estado en que estaba cuando se clonó (puede perder algunos ganchos del lado del servidor y demás, pero todos los datos versionados estarían allí; [Cómo obtener Git en un servidor](https://git-scm.com/book/en/v2/ch00/_getting_git_on_a_server) para más detalles).

Clonas un reporitorio con `git clone <url>`. Por ejemplo, si desea clonar la bilbioteca enlazable Git llamada `libgit2`, puede hacerlo así:

    $ git clone https://github.com/libgit2/libgit2

Esto crea un directorio llamado `libgit2`, inicializa un directorio `git` dentro del él, extrae todos los datos para ese repositorio y extrae una copia de trabajo de la última versión. Si ingresa al nuevo directorio `libgit2` que acaba de crear, verá los archivos del proyecto allí, listos para trabajar o usar.

Si desea clonar el repositorio en un directorio con un nombre diferente `libgit2`, puede especificar el nuevo nombre del directorio como argumento adicional:

    $ git clone https://github.com/libgit2/libgit2 mylibgit

Este comando hace lo mismo que el anterior, pero se llama al directorio de destino `mylibgit`.

Git tiene varios protocolos de transferencia diferentes que puede usar. En el ejemplo anterior usa el protocolo `http://`, pero también puede ver `git://` o `user:@server:path/to/repo.git`, que usa el protocolo de tranasferencia SSH. Obtener Git en un servidor presentará todas las opciones disponibles que el servidor puede configurar para acceder a su repositorio Git y los pros y contras de cada uno.

[anterior](../1_Comenzando/18_Resumen.md) | [siguiente](./22_Registro_cambios.md)