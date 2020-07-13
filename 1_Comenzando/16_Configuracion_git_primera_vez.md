## Configuración de Git por primera vez

Ahora que tien Git en su sistema, querrá hacer algunas cosas para comenzar para personalizar su entorno Git. Debería hacer estas cosas solo una vez en cualquier computadora; se quedarán entre las actualizaciones. También puede cambiarlos en cualquier momento ejecutando nuevamente los comandos.

Git viene con una herramienta llamada `git config` que le permite obtener y establecer variables de configuración que controlan todos los aspectos de cómo se ve y opera Git. Estas variables se pueden almacenar en tres lugares diferentes:

1. `/etc/gitconfig` archivo: contiene valores aplicados a cada usuario en el sistema y todos sus repositorios. Si pasa la opción `--system` para `git config`, lee y escribe específicamente en este archivo. Como se trata de un archivo de configuración del sistema, necesitaría privilegios administrativos o de superusuario para realizar cambios en él.
2. `~/.gitconfig` o `~/.congif/git/config` archivo: valores específicos para usted, el usuario. Puede hacer que Git lea y escriba en este archivo especificamente pasando la opción `--global` y esto afecta a todos los repositorios con los que trabaja en su sistema.

3. `config` archivo en el directorio Git (es decir, `,git/config`) de cualquier repositorio que esté utilizando actualmente: específico para ser repositorio único. Puede forzar a Git a leer y escribir en este archivo con la opcion `--local`, pero de hecho es el valor predeterminado. Como era de esperar, debe estar ubicado en algún lugar de un repositorio de Git para que esta opción funcione correctamente.

Cada nivel anula los valores en el nivel anterior, por lo que los valores en `.git/config` triunfa sobre aquellos en `/etc/gitconfig`

En los sistemas Windows, Git busca el `.gitconfig` archivo en el directorio `$HOME` (`C:\Users\$USER` para la mayoría de las personas). También sigue buscando `/etc/gitconfig`. aunque es realtivo a la raíz de MSys, que es donde decidas instalar Git en tu sistema Windows cuando ejecute el instalador. Si está utilizando la version 2.x o posterior de Git para Windows, también hay un archivo de configuración de nivel de sistema en `C:\Documents and Settings\All Users\Application Data\Git\config` en Windows XP y en `C:\ProgramData\Git\config` en Windws Vista y en versiones posteriores. Este archivo de configuración `git config -f <file>`solo se puede cambiar administrador.

Puede ver todas sus configuraciones y de dónde provienen usando:

    $ git config --list --show-origin

### __Tu identidad__

Lo primero que debe hacer al instalar Git es configurar su __nombre de usuario__ y __dirección de correo electrónico__. Esto es importante porque cada confirmación de Git usa esta información, y está inmutablemente incorporada en las confirmaciones que comienza a crear:

    $ git config --global user.name "John Doe"
    $ git config --global user.email "johndoe@example.com"

Nuevamente, debe hacer esto una vez si pasa la opción `--global`, porque Git siempre usará esa información para cualquier cosa que haga en ese sistema. Si desea anular esto con un nombre o dirección de correo electrónico diferente para proyectos específicos, puede ejecutar el comando sin la opción `--global` cuando esté en ese proyecto.

Muchas de las herramientas de la GUI lo ayudarán a hacer esto cuando ejecute por primera vez.

### __Su editor__

Ahora que su identidad está configurada, puede configurar el editor de texto predeterminado que se usará cuando Git necestie que escriba un mensaje. Si no está configurado, Git usa el editor predeterminado de su sistema.

Si deseas utilizar un editor de texto diferente, como Emacs, puede hacer lo siguiente:

    $ git config --global core.editor emacs

En un sistema Windows, si desea utilizar un editor de texto diferente, debe especificar la ruta completa a su archivo ejecutable. Esto puede ser diferente dependiendo de cómo esté empaquetado su editor.

En el caso de Notepad ++, un editor de programación popular, es probable que dese utilizar la version de 32 bits, ya que al momento de escribir la versión de 64 bits no es compatible con todos los complementos. Si está en un sistema Windows de 32 bits, o tiene un editor de 64 bits en un sistema de 64 bits, escribirá algo como eso:

    $ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

<i>__Nota:__ Vim, Emacs y Notepad ++ son editores de texto populares que los desarrolladores utilizan a menudo en sistemas basados ​​en Unix como Linux y macOS o un sistema Windows. Si está utilizando otro editor, o una versión de 32 bits, encuentre instrucciones específicas sobre cómo configurar su editor favorito con Git. [core.editor](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config#_core_editor)<i>

<i>__Advertencia__: Es posible que, si no configura su editor de esta manera, se encuentra en un estado realmente confuso cuando Git intenta iniciarlo. Un ejemplo en un sistema Windows puede incluir un operación Git terminada prematuramente durante una edición iniciada por Git</i>

### __Comprobación de su configuración__

Si desea verificar su configuración, puede usar el comando `git config --list` para enumerar todas las configuraciones que Git puede encontrar en ese punto:

    $ git config --list
    core.symlinks=false
    core.autocrlf=true
    core.fscache=true
    color.diff=auto
    color.status=auto
    color.branch=auto
    color.interactive=true
    help.format=html
    rebase.autosquash=true
    http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
    http.sslbackend=openssl
    diff.astextplain.textconv=astextplain
    filter.lfs.clean=git-lfs clean -- %f
    filter.lfs.smudge=git-lfs smudge -- %f
    filter.lfs.process=git-lfs filter-process
    filter.lfs.required=true
    credential.helper=manager
    user.email=franceskolyperez@gmail.com
    user.name=pedritoeldulce
    core.repositoryformatversion=0
    core.filemode=false
    core.bare=false
    core.logallrefupdates=true
    core.symlinks=false
    core.ignorecase=true
    remote.origin.url=https://github.com/pedritoeldulce/git-book-2014.git
    remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
    branch.master.remote=origin
    branch.master.merge=refs/heads/master

Puede ver las claves más de una vez, porque Git lee la misma clave de diferentes archivos (`/etc/gitconfigy ~/.gitconfig`, por ejemplo). En este caso, Git usa el último valor para cada clave única que ve.

También puede verificar qué Git cree que esel valor de una clave específica escribiendo `git config <key>`

    $ git config user.name
    John Doe

<i>Nota: Dado que Git podría leer el mismo valor de variable de configuración de más de un archivo, es posible que tenga un valor inesperado para uno de estos valores y no sepa por qué. En casos como ese, puede consultar a Git sobre el __origen__ de ese valor, y le dirá qué archivo de configuración tuvo la última palabra al establecer ese valor:</i>

    $ git config --show-origin rerere.autoUpdate
    file:/home/johndoe/.gitconfig	false