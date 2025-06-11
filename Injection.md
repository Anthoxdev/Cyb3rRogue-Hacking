INJECTION.

1.	Inicializar.
   
Para iniciar desplegamos la máquina usando el comando de Kali Linux sudo bash para inicializar los archivos .sh y .tar

 ![image](https://github.com/user-attachments/assets/43fe8e18-093e-404e-a97d-a84a27aae8c1)


2.	Reconocimiento.
   
Una vez inicializada, tomamos la ip y utilizamos la herramienta nmap para revisar los puertos abiertos. (sudo nmap -p- --open -sS -min-rate 5000 -vvv -n -Pn “IP a escanear”)
En este caso estamos usando un reconocimiento activo (escaneo de puertos).
Donde:
-p- sirve para escanear los 65536 puertos TCP 
--open se usa para mostrar solo los puertos abiertos
-sS hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
-min-rate 5000 fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
-vvv Modo verbose x3, o lo máximo detallado posible.
-n elimina la resolución DNS
-Pn Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![image](https://github.com/user-attachments/assets/7cabfa1f-a053-427b-81c2-54e5eed6773d)

Como podemos observas en la imagen, están abiertos los puertos 22 y 80, que corresponden a los protocolos SSH y HTTP, por lo que revisamos si hay alguna página asociada a la ip.

3.	Enumeración.
   
![image](https://github.com/user-attachments/assets/37fa3719-e9ce-4a82-99a9-b0d3a3884132)
 
Como podemos ver existe un dominio asociado a la ip con un cuadro de inicio de sesión.


Usamos Gobuster para realizar fuerza bruta y encontrar direcciones asociadas con un diccionario de direcciones comunes.
(gobuster dir -u http://IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt)
Donde:

dir es el modo de busca por direcciones.
-u es una flag que en este caso se usa para denotar la url a donde vas.
-w es una flag que en este caso se usa para buscar en una wordlist.

![image](https://github.com/user-attachments/assets/9dbb3ff8-a6fc-47fb-91d6-eb8e37904032)

El resultado arroja un directorio llamado /server-status con status 403, es decir acceso prohibido.

![image](https://github.com/user-attachments/assets/cdac1b7d-a943-4b91-9ec8-dc750a3d075d)

Probamos login:admin / password:admin.

![image](https://github.com/user-attachments/assets/55556753-a3ee-4fa7-afa7-9a5d48abe14f)
![image](https://github.com/user-attachments/assets/2a910f33-1d5f-4381-a90b-275fcfa0089b)

Debido a que no podemos ingresar a ese directorio intentamos realizar una inyección SQL para ver si la página posee dicha vulnerabilidad y lograr bypassear el login.

4.	Explotación
   
Usamos la línea de comando ‘or 1=1-- - que básicamente le dice a SQL que si el usuario suministrado es válido, busque en su base de datos el usuario y realice el login, pero también debe hacer esto mismo si 1=1 lo cual se cumple siempre.
(En el apartado de password ponemos cualquier cosa).
 
![image](https://github.com/user-attachments/assets/b131a9d5-9bc3-4453-9a2f-637497cd1233)

Notamos que efectivamente, es vulnerable a SQLi  y pudimos ingresar con las credenciales de un usuario llamado Dylan. 

![image](https://github.com/user-attachments/assets/fb1d6765-442b-4365-ba71-c19ad4de15ab)

Mediante el protocolo ssh con el usuario de Dylan, intentamos ingresar al sistema usando el comando ssh usuario@ip  e ingresando la contraseña encontrada.

![image](https://github.com/user-attachments/assets/b12313bf-2122-44ff-a73e-423b936f43dc)

Revisamos que permisos tiene Dylan con el comando whoami.
 
![image](https://github.com/user-attachments/assets/47b03691-95b0-42f2-8654-97d065254636)

Vemos que no tiene permisos de root (superusuario).

5.	Post- explotación.
   
Intentemos escalar privilegios. Iniciamos buscando binarios SUID para usuarios root (Set User ID).
find / -perm -4000 -user root 2>/dev/null
Donde:
find/ Se usa para buscar en todos los directorios.
-perm-4000 Busca archivos con el bit SUID activo (4000 en octal). El - antes del número significa "al menos estos permisos".
-user root Solo muestra archivos que pertenezcan al usuario root.
2>/dev/null Redirige los errores (stderr) al "basurero" (/dev/null). Esto evita que se llene la pantalla con mensajes de "permiso denegado".

![image](https://github.com/user-attachments/assets/697af43b-c9d3-4cc6-85e7-8385cca703c5)

Vemos que está el binario /usr/bin/env por lo que es sencillo usar 
 
![image](https://github.com/user-attachments/assets/65f7199c-4972-41e1-ab73-cbc471d96450)

Donde -p preserva los privilegios efectivos del usuario (por ejemplo, root) al iniciar el shell.
Y listo, somos usuarios ROOT.

