## Obteniendo ayuda

Si alguna vez necesita ayuda mientras usa Git, hay tres formas equivalentes de obtener la ayuda completa de la página del manual (página de manual) para cualquiera de los comandos de Git:

    $ git help <verb>
    $ git <verb> --help
    $ man git -<verb>

Por ejemplo, puede obtener la ayuda de la página de manual para el comando `git config` ejecutando esto:

    $ git help config

Estos comandos son buenos porque puedes acceder a ellos desde cualquier lugar, incluso sin conexión. Si las páginas de manual y este libro no son suficientes y necesita ayuda en persona, puede probar el canal `#git` o `#github` en el servidor IRC de Freenode, que se puede encontrar en [ https://freenode.net]( https://freenode.net).
Estos canales se llenan regularmente con cientos de personas que están muy bien informadas sobre Git y que a menudo están dispuesto a ayudar.

Además, si no necesita la ayuda completa de la página de manual, pero solo necesita una actualización rápida de las opciones disponibles para un comando Git, puede solicitar la salida de  "ayuda" más concisa con la opción `-h`, como en:

    $ git add -h
    usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch           select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    --renormalize         renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod <(+/-)x>      override the executable bit of the listed files


