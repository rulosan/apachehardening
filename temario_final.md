Temario

====  Lunes    =====

- Hardening SSH

	- Editar el archivo /etc/ssh/sshd_config
		 # Para que no se conecte el root
		 - PermitRootLogin no.
		 # Para denegar la coneccion por password y limitarla a las keys
		 - PasswordAuthentication no. 
		 # Permitir los usuarios que se puedan conectar por ssh
		 - AllowUsers deploy.
		 # Limitar la version de protocolo.
		 - Protocol 2.
		 # Cambiar el puerto si no van a utilizar el port knocking.
		 - Port 22.
		 # Definir una ip escuchando , la asociada a la tarjeta de red. (hablando de varias tarjeta).
		 - ListenAddress 192.168.0.1
	- Port Knocking
		- Knockd -> demonio que va estar escuchando en Syslog, envio de paquetes SYN para poder ejecutar una regla de iptables (ufw) y abrir los puertos.
		- nmap -sS -p22 <ip> --> filtered.
		- knock ip 7000 8000 9000
		- nmap -sS -p22 <ip> --> open.
		- Conectar por ssh.

====  Martes   =====
	- Instalacion apache2
		- yum update
		- yum install httpd php mariadb.

	- Hardening php
		- php.ini
			- disable_function = exec,system,shell_exec,passthru,eval
			- html_errors = off
			- expose_php = off
			- track_errors = off.
			- magic_quotes_gpc = off.
			- display_errors = off.
			- session.name = CUSTOMID (le puede pegar a las aplicacion) (opcional).
	- Harding Apache2
		- ServerTokens Pro (Quitaba informacion del servidor Apache)
		- ServerSignature Off
		- TraceEnable Off
		- Header always unset "X-Powered-By"
		- Header always unset "x-powered-by"
		- Header unset "X-Powered-By"
		- Header unset "x-powered-by"
	- VirtualHost.
		- <VirtualHost *:80> 
			NameServer midominio.com
		  </VirtualHost>
		- Modificar DNS
			- midomino.com A 192.168.0.6.
		- vim /etc/hosts
			- 192.168.0.6 midominio.com
			- Dentro del protocolo http le modifica el encabezado de Host.
	- comandos a recordar.
		- apachectl configtest.
		- httpd -M | grep <nombre modulo>
		- sysctemctl start httpd
		- service httpd start
		- /etc/init.d/httpd start
		- Debian / ubuntu
			- a2ensite -> archivo configuracion sitio.
			- a2enmod -> agrega un modulo
			- a2dismod -> elimina un modulo de la configuracion
			- a2dissite -> elimina el sitio.
			- a2 <tab>
	- Comandos
		- stat file
	- directiva para dar formato al log LogFormat.

==== Miercoles =====
	- Primero crear la estructura de directorios, revisar los permisos.
	- Chown && chmod
		- Centos
			- chown apache:apache -R /apachejail
			- find /apachejail -name "*.php" -type f -exec chmod 755 {} \;
			- find /apachejail -type d -exec chmod 644 {} \;
		- yum install tree
		- tree .
		/apachejail/
			|-- tmp
			`-- var
			    |-- lib
			    |   `-- php
			    |       `-- sessions
			    |-- run
			    |   `-- apache2
			    |       `-- apache2.pid
			    `-- www
			        |-- admin.peluchon.net
			        |   `-- index.php
			        |-- html
			        |   `-- index.php
			        `-- www.peluches.com
			            |-- docs
			            |   `-- index.php
			            `-- index.php
	- Jails
		- ChrootDirectory /apachejail
	- Enjaular el proceso de httpd en un directorio, y no permitir el acceso a directorios mas sensibles.

====  Jueves   =====
	- SELinux.
		- getenforce -> estado selinux
		- /etc/selinux/config -> configurar parametros Enforcing / Permissive / Disable.
		- restorecon -vR /var/www/html # sincronizar las configuraciones de los permisos con Selinux.
		- semanage -> manerar de administrar selinux.

	- fail2bann
	
	- Mod_Security (Seguridad del protocolo HTTP), Mod_Evasive (Diagnostico de DOS).
		- yum install mod_security.
		- descargar las reglas OWASP.
		- Instalarlas
	- apachectl configtest.
==== Viernes   =====
	- lanzamos nikto, zaproxy para ver como trabaja mod_security.
	- Reviso las bitacoras de modsec_audit.log y access_log
