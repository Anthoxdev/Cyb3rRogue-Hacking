FirstHacking.

1.	Inicialización 

Para iniciar desplegamos la máquina usando el comando de Kali Linux sudo bash para inicializar los archivos .sh y .tar

![imagen](https://github.com/user-attachments/assets/af4e5dff-c267-4020-8438-f9ac1873f6ca)

2.	Reconocimiento 

Procedemos a realizar un escaneo de puertos con la herramienta nmap en la ip suministrada. Donde:
`-p-` sirve para escanear los 65536 puertos TCP 
`--open` se usa para mostrar solo los puertos abiertos
`-sS` hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
`-min-rate 5000` fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
`-vvv` Modo verbose x3, o lo máximo detallado posible.
`-n` elimina la resolución DNS
`-Pn` Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![imagen](https://github.com/user-attachments/assets/6f80c4b1-6227-45cd-9b7b-1f0819d23ccc)

Vemos que tenemos el puerto 21 abierto, que corresponde al protocolo FTP.
Lo que se me ocurre es tratar buscar vulnerabilidades asociadas a la versión del protocolo.

3.Enumeración. 

añadimos la flag `-sCV`  a nmap para ver la versión.

![imagen](https://github.com/user-attachments/assets/095aef81-56e9-4bbf-99d0-1c33060e14ac)

Notamos que la versión es “vsftdp 2.3.4”, procederé a buscarla en searchsploit.

![imagen](https://github.com/user-attachments/assets/d7fc2481-981e-45a6-a37e-882ee8d721f5)

En este caso usaremos backdoor .py  para lo que utilizaremos la herramienta metasploit.

![imagen](https://github.com/user-attachments/assets/53ecb9b8-2d84-4a1a-86ee-231a837f6a7a)
 
4.Explotación.

![imagen](https://github.com/user-attachments/assets/5fd587ba-ed94-4368-ad9c-ffccce9dfb49)

Configuramos en remote host la ip del host en este caso la máquina a la que tratamos de vulnerar y revisamos que esté correcto.

![imagen](https://github.com/user-attachments/assets/450341cf-1886-464f-b6e0-c20e5b1966d6)
 
Luego simplemente arrancamos el ataqué. 

![imagen](https://github.com/user-attachments/assets/b7dfc312-d638-47e8-b050-564b4fbc53de)

Usamos `whoami` y listo, ya somos GODS. 

![imagen](https://github.com/user-attachments/assets/29a95a70-b684-43e4-80cb-d31314df3ae3)
  
