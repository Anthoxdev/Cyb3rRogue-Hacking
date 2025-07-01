Obsession.

1.	Inicialización 

Para iniciar desplegamos la máquina usando el comando de Kali Linux `sudo bash` para inicializar los archivos .sh y .tar
 
![imagen](https://github.com/user-attachments/assets/f24ce668-724b-4cfa-8c63-9dab07f48399)

2.	Reconocimiento.

Una vez inicializada, tomamos la ip y utilizamos la herramienta `nmap` para revisar los puertos abiertos. (`sudo nmap -p- --open -sS -min-rate 5000 -vvv -n -Pn “IP a escanear” `)
En este caso estamos usando un reconocimiento activo (escaneo de puertos).
Donde:
-`-p-` sirve para escanear los 65536 puertos TCP 
-`--open` se usa para mostrar solo los puertos abiertos
-`-sS` hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
-`-min-rate 5000` fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
-`-vvv` Modo verbose x3, o lo máximo detallado posible.
-`-n` elimina la resolución DNS
-`-Pn` Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![imagen](https://github.com/user-attachments/assets/0bb10208-2d2a-40b6-bc59-7df60e1b05eb)

3.	Enumeración.

Empezamos buscando la ip en el navegador ya que el puerto 80 está abierto.
 
![imagen](https://github.com/user-attachments/assets/1c567322-e8d0-4c83-add7-3c492ee6226a)

Encontramos una página al parecer de un gym. 
 
![imagen](https://github.com/user-attachments/assets/5dc28f62-1fff-4d0a-929b-7502d1e1e03e)

Tiene un apartado de registro, usaré credenciales falsas para registrarme y ver si tenemos algo relevante.

![imagen](https://github.com/user-attachments/assets/23a3df3c-fd73-4e72-a297-ff16d4d57584)

Russoski Coaching dice, hagamos fuerza bruta con ese nombre.

![imagen](https://github.com/user-attachments/assets/5bab5d6a-d349-4bb8-a4d4-8de8b2d425e3)
 
4.	Explotación.

Encontramos una contraseña con la que podremos ingresar por SSH.

![imagen](https://github.com/user-attachments/assets/6f9d3e27-b01c-4a03-9dbe-c636863a9252)

Ingresamos como russoski. 
 
![imagen](https://github.com/user-attachments/assets/3785e90e-1dfb-45e8-a925-853c457a2ab3)

Usemos el bin vim para escalar privilegios.
 
![imagen](https://github.com/user-attachments/assets/8b2982ba-faff-4d6a-94ab-c1c3d3edb653)



Nota:
Usando otros métodos, se podía llegar al usuario russoski
por ejemplo, usando `gobuster` encontrarán directorios con pistas que dirigen a ese usuario.

si usan a flag `-sCV`en el nmap, podrán ver que en ftp hay 2 archivos que con `get` podrán descargar a su dispositivo local y en ellos, está el usuario russoski.

¡¡¡¡Ánimo!!!!
