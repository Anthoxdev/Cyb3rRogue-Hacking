Tproot.

1.	Inicialización 

Inicializamos la máquina de Dockerlabs haciendo `sudo bash` con los archivos .tar .sh

![imagen](https://github.com/user-attachments/assets/1f273c81-6f26-4579-bad5-9097179fe9cb)

2.	Reconocimiento

Procedemos a realizar un `nmap` de la ip para ver los puertos abiertos.

![imagen](https://github.com/user-attachments/assets/59e45ead-dadf-47f0-a6fd-cb9a08026afe)
 
Vemos los puertos  21 y 80  que corresponden a los puertos FTP y HTTP respectivamente.
3.	Enumeración

![imagen](https://github.com/user-attachments/assets/d67ac517-7b97-465e-a193-96baf4c5ac10)

Buscamos la ip en el navegador para revisar el dominio asociado a ella.  
Página predeterminada.
Buscaré con `gobuster`subdirectorios asociados.

![imagen](https://github.com/user-attachments/assets/6df37500-9fbe-42a9-9437-7293c2ad727c)
 
Solo la página de inicio (index) es exequible, el resto tiene código 403 lo cual significa que no hay forma de acceder a ella convencionalmente. 


Por el HTTP no es, busquemos por el protocolo FTP, para ello usaré nmap con la flag 
`-sCV` para que me muestre la versión del puerto. 

![imagen](https://github.com/user-attachments/assets/7983f21d-8e27-4266-82bc-fa49ac59ca27)
 
Con la versión en mano busquemos posibles vulnerabilidades usando `msconsole`para abrir metasploit y luego `search`para buscar si la version tiene alguna vulnerabilidad asociada.

![imagen](https://github.com/user-attachments/assets/fd8aec59-9b1c-4950-b01e-50677f29bb6e)
 
Encontramos Backdoor Command Execution.

4.	Explotación y Post-Explotación.
   
Usemos 0 para iniciar el exploit, seguidamente configuramos el RHOST con el ip de la victima e iniciamos el exploit.

![imagen](https://github.com/user-attachments/assets/3a02b2d0-b4b0-4da8-bad5-8a5f09a8a224)
![imagen](https://github.com/user-attachments/assets/c13d2d41-c25c-42bd-89a3-25fede541872)
  
`Clear` y `whoami`

![imagen](https://github.com/user-attachments/assets/b6eac152-be21-41c7-94a7-aa0813eb0264)
 
Y listo, somos root
