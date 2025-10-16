# Jenkins - Platzi

## Instalación Linux

`sudo apt update`

`sudo apt install openjdk-8-jdk wget gnupg`

`wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -`

`sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'`

`sudo apt update`

`sudo apt install jenkins`

`systemctl status jenkins`

## Instalación Windows

https://jenkins.io/download/thank-you-downloading-windows-installer-stable/

* Cuando pregunte por la url de jenkins (localhost...) se puede cambiar el puerto que tiene por defecto

* Adicional se puede cambiar la ip, puerto, etc en el archivo C:\Program Files (x86)\Jenkins\jenkins.xml

Ejemplo:

`<arguments>-Xrs -Xmx256m -Dhudson.lifecycle=hudson.lifecycle.WindowsServiceLifecycle -jar "%BASE%\jenkins.war" --httpPort=8082 --webroot="%BASE%\war"</arguments>`

* Para iniciar / detener / conocer status (en modo administrador):

`cd C:\Program Files (x86)\Jenkins`

`jenkins.exe start / stop / status`

> **Importante:** Cada quien deberia tener su propio usuario, para las auditorías.


## Conexion con git

1. En "Configurar el origen del código fuente" seleccionar git, colocar el repositorio.

Si se quiere que los build se hagan desde cualquier branch, dejar el campo branches to build vacio

2. En la seccion "Disparadores de ejecuciones" seleccionar GitHub hook trigger for GITScm polling

3. Recordar que si se va a utilizar nodejs para el build, se debe seleccionar en "Entorno de ejecución" la opción Provide Node & npm bin/ folder to PATH para que el build tome la version que especifiquemos

4. En la sección ejecutar:

`cd jenkins-tests && npm install`

`cd jenkins-tests && npm test`

5. En github, en el proyecto vamos a settings > webhooks

Debemos exponer la ip de la maquina. En windows se puede hacer con ngrok: `./ngrok http 8082`

Vamos a http://localhost:4040 y ahi aparecen las url.

Copiamos y colocamos en payload URL (github): https://<URL-NGROK>/github-webhook/

## Manejar usuarios

Manage Jenkins > Manage Users

Create user

Hay plugin para manejar los usuarios a través de git o google

## Jobs

Trabajos que ejecuta jenkins 

- Freestyle project: se configura el job "a mano"
	- En build se especifican las tareas a ejecutar, según el tipo
	- Hay muchas cosas a configurar dependiendo de los plugins que se instalan
	
- Jenkins por defecto toma las instalaciones que hay en la maquina (por ejemplo nodejs o git) solo si puede acceder a ellas. Pero tambien Jenkins puede instalar sus propias herramientas, teniendo como ventaja hacerlo más portable

## Plugins 

Manage jenkins > Manage plugins