BREAKMYSSH.

1. Inicialización.

Para iniciar desplegamos la máquina usando el comando de Kali Linux sudo bash para inicializar los archivos .sh y .tar

![image](https://github.com/user-attachments/assets/049091bc-0858-4522-bc9c-08e5a9ed411b)
  
2. Reconocimiento.

Procedemos a realizar un escaneo de puertos con la herramienta nmap en la ip suministrada. Donde:
-p- sirve para escanear los 65536 puertos TCP 
--open se usa para mostrar solo los puertos abiertos
-sS hace un escaneo TCP SYN (semiabierto) más rápido e indetectable que un escaneo completo (-sT)
-min-rate 5000 fuerza nmap para enviar 5000 paquetes por segundo (puede arrojar falsos negativos)
-vvv Modo verbose x3, o lo máximo detallado posible.
-n elimina la resolución DNS
-Pn Evita que nmap haga ping previo, asumiendo que el host está up (se usa para evitar filtros ICMP o Firewall).

![image](https://github.com/user-attachments/assets/712950a6-4b12-48e7-92d5-bcdfa52d076d)
 
Notamos abierto solo el puerto 22 que corresponde al protocolo SSH.

3. Enumeración.

Usamos la herramienta hydra para buscar combinaciones de usuario y contraseña que nos permitan ingresar como usuario.

![image](https://github.com/user-attachments/assets/8453565e-ed48-4fae-9566-786094a72304)
 
En este caso la estructura de hydra tiene 
-L que significa buscar varios usuarios (luego la biblioteca de usuarios).
-P que significa buscar varias contraseñas (luego la biblioteca de contraseñas).
-t es la abreviatura de thread y el 4 son los intentos concurrentes al mismo tiempo.

4. Explotación.
   
Intentamos entrar por la consola con SSH , pero vemos que no hay usuario.
configuramos hydra esta vez declarando la contrasella encontrada.

![image](https://github.com/user-attachments/assets/356caf31-45e0-43b0-8655-911fe0f64282)
 
Encontramos el usuario root.

![image](https://github.com/user-attachments/assets/b5902fdb-5e61-4267-b09b-2f46a0ba5959)
 
No es necesario escalar privilegios, ya somos usuarios root.
