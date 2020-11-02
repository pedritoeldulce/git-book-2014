## 2.3 Conceptos básicos de Git: Visualización del historial de confirmaciones.

### Ver historial de confirmaciones
Después de haber creado varias confirmaciones, o si ha clonado un repositorio con un historial de confirmaciones 
existente, probablemente querrá mirar hacia atrás para ver qué sucedió. La herramienta más básica y poderosa para hacer 
esto es el comando `git log`.

Estos ejemplos utilizan un proyecto muy simple llamado `simplegit`. Para obtener el proyecto, ejecute: 
    
    $ git clone https://github.com/schacon/simplegit-progit
    
Cuando se ejecuta en este proyecto `git log`, debería obtener un resultado parecido a esto:

    $ git log
    commit ca82a6dff817ec66f44342007202690a93763949
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Mon Mar 17 21:52:11 2008 -0700

    Change version number

    commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 16:40:33 2008 -0700

    Remove unnecessary test

    commit a11bef06a3f659402fe7563abf99ad00de2209e6
    Author: Scott Chacon <schacon@gee-mail.com>
    Date:   Sat Mar 15 10:31:28 2008 -0700

    Initial commit