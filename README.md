Taller uso de Git y GitHub (Comunidad del controlador de versiones "GIT")

1- Instalar el controlador de versiones "git" con el manejador de paquetes que poseas ()En mi caso dnf de fedora 23:

	$ sudo dnf install git

2- Crear cuenta en GitHub.com Suministrando "username", "email", y "password"

3- Crear un Repositorio de Prueba (Lo cual es una carpeta al final), elegir tipo "Public" o sea gratis (Para las cuentas privadas el contenido del/los repositorios no es visible para la comunidad, solo para el creador de este y a quien haya dado permiso. Este tipo de cuenta es comercial, con cuartos).

	-Antes de presionar el boton "Create Repository" tachamos la opcione "Initialize 	this Repository With a Readme" para poder utilizarlo o clonarlo a la maquina local, 		y por supuesto subir contenido a este desde la mi maquina local"

4- Suministrar al ambiente de git en la (maquina local) el username y email: [Creo que esto es opcional]:

	$ git config --global user.name "yugularx"
	$ git config --global user.email "fdomin24@gmail.com"

5- Crear un directorio con el nombre igual o parecido al nombre del repositorio recien creado (Esto para evitar confuciones futuras manejando varios Repositorios.)

	$ mkdir git_learning

6- Para que la cuenta en GitHub.com reconozca mi host local debo suministrarle el contenido de una ssh llave publica que vamos a generar con:

	$ ssh-keygen -t ecdsa -b 521		//Se puede utilizar solo el comando ssh-keygen sin opciones, las use para crear una con estas caracteristicas (521 bits).

	- Se crea una llave publica y otra privada en ~./ssh
	- Vemos el contenido de la llave publica, la copiamos
	- Nos dirigimos a nuestra cuenta de Github.com > Perfil > Settings > SSH and GPG keys > Y presionamos el boton "New SSH Key"
	- Colocamos un titulo en el texbox "Title"
	- Pegamos el contenido de nuestra llave publica en el Texbox "Key"
	- Por ultimo damos click en el boton "Add SSH Key"
	- En nuestra maquina local anadimos la llave privada:
		
		$ eval `ssh-agent -s`
		$ ssh-add ~/.ssh/clave_privada_rsa
	
	- Editar en el archivo "~/nombrerepositorio/.git/config" la URL del respositorio remoto (origin) para que use 
	protocolo ssh en vez de http:

		$ vim git_learning/.git/config

		[remote "origin"]
		#url = http://github.com/yugularx/git_learning		
			Cambiar a:

 		url = git@github.com:yugularx/git_learning.git	//Asi debe quedar

		O Emitir el comando:

		$ git remote set-url origin git@github.com:yugularx/git_learning.git		//El cual hace la misma funcion

NOTA: Esto nos sirve paque que no nos pida las credenciales de GitHub cada vez que hagamos push

7- Iniciamos nuestro repositorio local con el comando "git init" dentro del directorio recien creado;

		$ cd git_learning
		$ git init
	Initialized empty Git repository in /home/yugularx/git_learning/.git/

		ls .git/
	branches  config  description  HEAD  hooks  index  info  objects  refs

8- Agregar archivos (Nuevos o Existentes) al control de versiones git:
	
	$ touch README2
	$ git add README2

9- Para ver el status de los archivos trakeados o agregados al repositorio usamos la opcion "status":

	$ git status
	On branch master

	Initial commit

	Changes to be committed:
	  (use "git rm --cached <file>..." to unstage)

		new file:   GitHub_Taller_Uso.txt
		new file:   README2

	Untracked files:
	  (use "git add <file>..." to include in what will be committed)

		cpucheck.sh
		diskcheck.sh
		lastlogin.sh
		memcheck.sh

Nota: Podemos ver archivos que se encuentran en el directorio pero aun no se han agregados al repositorio para seguir sus versiones.


10 - Hacer el primer commit del repositorio (Commit = Registro de Cambio):

	$ git commit -m "Primer Registro de cambio, commit al Repositorio local"
	[master (root-commit) 0d211cd] Primer Registro de cambio, commit al Repositorio local
	 2 files changed, 33 insertions(+)
	 create mode 100755 GitHub_Taller_Uso.txt
	 create mode 100644 README2

	Solo se commitearon los 2 archivos que agregue al repositorio (Estos cambios solo se reflejan todavia en el repositorio local)	

	$ git status
	On branch master
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)

		modified:   GitHub_Taller_Uso.txt

Ya que el repositorio remoto y Local no se han sincronizado (Diferentes Repositorios con el mismo nombre), hacemos lo siguiente
	
11- Nos conectamos con el Repositorio:

	$ git remote add origin git@github.com/yugularx/git_learning.git

12- Probamos jalando (Pull) el contenido del repositorio remoto al repositorio local:

	$ git pull origin master

- Si aparece el error:

	fatal: 'git@github.com/yugularx/git_learning.git' does not appear to be a git repository
	fatal: Could not read from remote repository.

- Colocamos la URL del repositorio directamente:

	$ git remote set-url origin https://github.com/yugularx/git_learning

- E intentamos de nuevo:

	$ git pull origin masterwarning: no common commits
	remote: Counting objects: 3, done.
	remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
	Unpacking objects: 100% (3/3), done.
	From https://github.com/yugularx/git_learning
	 * branch            master     -> FETCH_HEAD
	 * [new branch]      master     -> origin/master
	Merge made by the 'recursive' strategy.
	 README.md | 2 ++
	 1 file changed, 2 insertions(+)
	 create mode 100644 README.md


- Ahora se nos añade el unico archivo del Repositorio Remoto (Readme.md) a nuestro repositorio local


- Si emitimos un status podremos ver que un archivo (Este mismo) del repositorio local ha sido modificado:
	
	$ git status
	On branch master
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)

		modified:   GitHub_Taller_Uso.txt

	no changes added to commit (use "git add" and/or "git commit -a")


13- Actualizar Archivo para un posterior commit:

	Este archivo actualizado no sera rastreado a menos que actulicemos con git o emitamos un "commit" para todos los archivos des repositorio:

		$ git add .		//Actualiza todos los archivos del repositorio local para que luego se les pueda dar commit 
					(Aceptar el cambio dentro del Repositorio).

		$ git add GitHub_Taller_Uso.txt 	//Actualiza el archivo "GitHub_Taller_Uso.txt " en el repositorio local, 
							para que luego se le pueda dar commit (Aceptar el cambio dentro del Repositorio).


		$ git commit -a		// Acepta todas las ultimas midificaciones de todos los archivos dentro del repositorio
		[master 682d435] 4to Commit
		1 file changed, 23 insertions(+)

14- Enviar el contenido del repositorio local al remoto (Hacer Push o enviar commits al remoto) dentro de la rama(Branch, por el momento solo tenemos este branch) master:
		
		$ git push origin master
		
		Counting objects: 18, done.
		Delta compression using up to 4 threads.
		Compressing objects: 100% (17/17), done.
		Writing objects: 100% (18/18), 4.57 KiB | 0 bytes/s, done.
		Total 18 (delta 6), reused 0 (delta 0)
		remote: Resolving deltas: 100% (6/6), done.
		To https://github.com/yugularx/git_learning
		   8f131e6..682d435  master -> master


		Nota: El/Los programador/es, o usuario de Git puede tener varias ramas (Branch), una para el servidor de desarrollo
		en si, otra para los diseñadores, otro branch para el servidor de produccion(Que puede ser master), etc.



HELP:

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Find by binary search the change that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status

grow, mark and tweak your common history
   branch     List, create, or delete branches
   checkout   Switch branches or restore working tree files
   commit     Record changes to the repository
   diff       Show changes between commits, commit and working tree, etc
   merge      Join two or more development histories together
   rebase     Forward-port local commits to the updated upstream head
   tag        Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch      Download objects and refs from another repository
   pull       Fetch from and integrate with another repository or a local branch
   push       Update remote refs along with associated objects
	
