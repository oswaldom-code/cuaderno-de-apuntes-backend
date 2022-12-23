### Pasos para actualizar la copia (fork) de un repositorio de Git:

1.  Agregar la referencia al repositorio remoto original, al cual llamaremos «upstream», esto lo logramos con el comando:`git remote add upstream [https://github.com/whoever/whatever.git](https://github.com/whoever/whatever.git)`
2.  Traernos todas las ramas de dicho repositorio remoto con: `git fetch upstream`
3.  Irnos a la rama que queremos actualizar, por ejemplo master: `git checkout master`
4.  Reescribir nuestra rama master con los commits nuevos de la rama master del repositorio original con:  
    `git rebase upstream/master`
5.  Finalmente si queremos actualizar nuestro fork remoto, lo haremos ejecutando `git push -f origin master`

Recuerda que estos cambios no afectarán al repositorio original, sólo tu fork (sobretodo si no tienes acceso de escritura al mismo).