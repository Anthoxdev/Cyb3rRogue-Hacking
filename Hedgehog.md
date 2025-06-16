Hedhehog.	

1.	Inicialización

Para iniciar desplegamos la máquina usando el comando de Kali Linux `sudo bash` para inicializar los archivos .sh y .tar
 
![imagen](https://github.com/user-attachments/assets/fab2d574-919c-4261-90c1-a1bd05189a0c)

2.	 Reconocimiento.

Una vez inicializada, tomamos la ip y utilizamos la herramienta `nmap` para revisar los puertos abiertos. (`sudo nmap -p- --open -sS -min-rate 5000 -vvv -n -Pn “IP a escanear” `)
En este caso estamos usando un reconocimiento activo (escaneo de puertos).
Donde:
`-p-` sirve para escanear los 65536 puertos TCP 
`--open` se usa para mostrar solo los puertos abiertos
`-sS` hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
`-min-rate 5000` fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
`-vvv` Modo verbose x3, o lo máximo detallado posible.
`-n` elimina la resolución DNS
`-Pn` Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![imagen](https://github.com/user-attachments/assets/9807dc85-5bd5-45ca-924d-d822de2e0e24)
 
Como podemos ver, encontramos los puertos 80 y 22 que corresponden a los protocolos HTTP y SSH respectivamente.

3.	Enumeración.

![imagen](https://github.com/user-attachments/assets/3a9362e5-9c43-44f0-8f20-9bf9e1fbbd25)
 
Revisamos el dominio asociado a la ip dada y encontramos la palabra “tails”, si inspeccionamos elementos encontramos exactamente lo mismo.
no hay nada oculto.

4.	Explotación.
Intentemos utilizar `hydra` a ver si encontramos alguna contraseña asociada a esa palabra.

![imagen](https://github.com/user-attachments/assets/b04b000f-7e54-4cc9-b3b9-60695fc2a1df)

Tardó demasiado tiempo, por lo que intentaré volteando el diccionario con `tac` que es `cat` al revés a ver si logramos algo.
Para esto buscaré el directorio donde se encuentra guardado y haré una copia directa en la carpeta donde guardo las máquinas.  

![imagen](https://github.com/user-attachments/assets/aee9db43-6013-4863-af8c-acbe24bf7420)

Procedemos.

![imagen](https://github.com/user-attachments/assets/00f2840c-77bd-43b7-93c6-9143cc5be0f4)

Veamos el archivo usando `cat`.

![imagen](https://github.com/user-attachments/assets/1f77417e-b6c3-4bad-ac72-cf81d382b17a)

Tiene espacios, hydra  nunca podrá leerlo con esas condiciones, así que eliminemoslos.

Hacemos 
 
![imagen](https://github.com/user-attachments/assets/1a20232c-f7f2-4516-92e2-1aa1374620e8)

Donde
•	`s/^[ \t]*//` Elimina espacios y tabulaciones al inicio de cada línea.
•	`s/[ \t]*$//` Elimina espacios y tabulaciones al final de cada línea.
•	`>` redirige el resultado a un nuevo archivo limpio: yourock_sin_espacios.txt.

Revisamos con `cat`.

![imagen](https://github.com/user-attachments/assets/196edc09-0174-423d-9a97-c130f025d627)
 
Revisemos si es posible ahora usar `hydra`.
 
![imagen](https://github.com/user-attachments/assets/f01ccf4a-9776-496a-9ebe-1a3f7fdb79db)

Listo, funcionó.

Intentemos ahora ingresar por SSH desde la consola.

![imagen](https://github.com/user-attachments/assets/a3f31253-c7ef-4c4e-b03a-a852953736ef)
 
5.	post-explotación. 

Vemos qué comandos podemos usar como root.

![imagen](https://github.com/user-attachments/assets/12d7d851-e703-4d9e-ade6-f11a22a43dab)

Nos dice que podemos usar cualquier comando con el usuario “Sonic” y además sin usar contraseña.

Así que usamos

![imagen](https://github.com/user-attachments/assets/497d4e54-ea35-4b7f-805f-e11459c16b9c)

y listo, somos root.
