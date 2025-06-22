Vacaciones.

1.	Inicialización

Para iniciar desplegamos la máquina usando el comando de Kali Linux `sudo bash` para inicializar los archivos .sh y .tar

 ![imagen](https://github.com/user-attachments/assets/d15109ff-b09b-48dd-a588-f53e4867a4e3)

2.	Reconocimiento.

Una vez inicializada, tomamos la ip y utilizamos la herramienta `nmap` para revisar los puertos abiertos. (`sudo nmap -p- --open -sS -min-rate 5000 -vvv -n -Pn “IP a escanear” `)
En este caso estamos usando un reconocimiento activo (escaneo de puertos).
Donde:
- `-p-` sirve para escanear los 65536 puertos TCP 
- `--open` se usa para mostrar solo los puertos abiertos
- `-sS` hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
- `-min-rate 5000` fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
- `-vvv` Modo verbose x3, o lo máximo detallado posible.
- `-n` elimina la resolución DNS
- `-Pn` Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![imagen](https://github.com/user-attachments/assets/383c601b-c244-46b5-a6de-1da9210379a6)
 
Como podemos ver, encontramos los puertos 80 y 22 que corresponden a los protocolos HTTP y SSH respectivamente.

3.	Enumeración
Ya que está el protocolo http abierto, ingresamos al dominio asociado a la ip dada  

![imagen](https://github.com/user-attachments/assets/e2f1262f-9590-44f3-902f-f2f4dee41e74)

En el que no encontramos absolutamente nada, por lo que el conducto regular que suelo seguir es, primero ver el código fuente de la página, y si no hay nada importante, realizar un ataque de fuzzing para ver directorios ocultos.

Comencemos con el código fuente.
 
![imagen](https://github.com/user-attachments/assets/81f21e3c-66cc-4c58-99f3-660e0989da8b)

Nos encontramos con este mensaje, y como el puerto 22 que corresponde al protocolo SSH está abierto, procedemos a realizar un ataque de fuerza bruta con esos nombres que encontramos, para lo que usaré la herramienta `hydra`.  

![imagen](https://github.com/user-attachments/assets/ab6dc836-7d20-4908-9001-0f41603c2ccd)

Encontramos una contraseña asociada a camilo, intentemos ingresar por SSH con ella, en paralelo dejaremos buscando a hydra contraseñas asociadas a juan.

![imagen](https://github.com/user-attachments/assets/9410da94-3293-4141-976b-bf2bfffdbef3)
 
Logramos ingresar, usemos `sudo -l` a ver si tenemos acceso a comandos de root.

![imagen](https://github.com/user-attachments/assets/dfd97896-5295-4bd4-935f-6fd384fcdf40)
  
No tenemos acceso al root por acá; busquemos en los directorios del sistema.  

![imagen](https://github.com/user-attachments/assets/b15e7ba0-a181-4504-aa57-cf0a1f4b555c)

Escojamos directorios aleatorios y revisemos uno por uno.
Tomaré el primero y luego el último y así sucesivamente hasta encontrar algo interesante.

![imagen](https://github.com/user-attachments/assets/3190f019-9232-4740-b6f5-21645dc50176)

Todos directorios del sistema predeterminados, Buscaré en var.
 
![imagen](https://github.com/user-attachments/assets/3e21c35c-9314-49a4-8755-97db1b1ac8c2)

Si recordamos bien, el mensaje de la página decía que juan le dejaría un correo a camilo.
veamos si en mail encontramos algo interesante. 

![imagen](https://github.com/user-attachments/assets/c24b05ab-c638-45ad-93e2-92233af5f875)

La contraseña de juan, por lo que podemos parar el hydra.
Hacemos `su juan` y luego en juan `sudo -l` para ver si podemos usar root. 

![imagen](https://github.com/user-attachments/assets/cdd3ed8f-4662-42e7-ade4-b2bc47017dce)
 
Encontramos el binario ruby, pero como no sé qué vulnerabilidad podemos explotar, acudiré a GTFObins 

![imagen](https://github.com/user-attachments/assets/ae91f886-e4c7-4fc7-90cb-82e1f46d2363)
 
Donde encontramos este comando que introduciremos en el promt de juan. 
 
![imagen](https://github.com/user-attachments/assets/a93c6358-f55f-47cf-bc36-e5d1d77c9bd4)

Y listo, somos root.
