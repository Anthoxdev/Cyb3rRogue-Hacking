BorazuwarahCTF.

1.	Inicialización 
Para iniciar desplegamos la máquina usando el comando de Kali Linux sudo bash para inicializar los archivos .sh y .tar
--- 
![imagen](https://github.com/user-attachments/assets/23e39ea9-e9d2-4da8-ae36-31814e47b0b3)
---
2.	Reconocimiento
---
Procedemos a realizar un escaneo de puertos con la herramienta nmap en la ip suministrada. Donde:
- `-p-` → Escanea los 65536 puertos TCP.
- `--open` → Muestra solo los puertos abiertos.
- `-sS` → Escaneo TCP SYN (semiabierto), más rápido e indetectable que `-sT`.
- `--min-rate 5000` → Fuerza a Nmap a enviar 5000 paquetes por segundo (puede arrojar falsos negativos).
- `-vvv` → Modo verbose x3, lo más detallado posible.
- `-n` → Elimina la resolución DNS.
- `-Pn` → Evita el ping previo; asume que el host está activo (útil contra firewalls o filtros ICMP).
 

![imagen](https://github.com/user-attachments/assets/65bea9f9-a530-466f-b6aa-931386278f8d)
 
Encontramos abiertos el puerto 22 (SSH) y el 80 (HTTP), por lo que por protocolo revisamos dominios asociados a la ip dada.
---
3.	Enumeración.
---
![imagen](https://github.com/user-attachments/assets/f0e343b2-7c30-4ce8-8995-51b20576c980)
 
Solo aparece una imagen de un kínder sorpresa, y al inspeccionar elementos no aparece nada especial, solo el código fuente de la imagen.

Lo único que se me ocurre es usar estenografía en la imagen para ver si hay algún mensaje encriptado dentro de ella.
Con el comando `steghide` donde `-sf` le dice a steghide:
"Usa este archivo como portador (stegofile) del contenido oculto."

![imagen](https://github.com/user-attachments/assets/dbf9fe2c-6a6f-4880-9fbd-0a22a442e6b2)

Vemos que nos dio un archivo .txt llamado secret, por lo que lo abriremos con `cat` a ver qué se encuentra en él.  

![imagen](https://github.com/user-attachments/assets/119ed5c1-ce10-46cf-8aae-35b07e67dc20)

Efectivamente, nos dieron una pista, debemos seguir buscando en la imagen.
Usemos otro comando de estenografía.
Dentro de mis apuntes, tengo un apartado completo con comando de estenografía en orden de uso bajo mi criterio.

## 🛠️ Herramientas Básicas

| Comando                         | Descripción                                 |
|--------------------------------|---------------------------------------------|
| `exiftool archivo.jpg`         | Extrae metadatos del archivo.               |
| `strings archivo.jpg`          | Muestra texto legible oculto en binarios.   |
| `binwalk archivo.jpg`          | Busca datos embebidos o comprimidos.        |
| `xxd archivo.jpg \| less`      | Visualiza el contenido hexadecimal.         |
| `file archivo.jpg`             | Detecta el tipo real del archivo.           |


Usemos uno a uno en la imagen a ver si logramos encontrar algo interesante.
 
![imagen](https://github.com/user-attachments/assets/35d226fe-8086-4d0b-b3bf-724a7a4b9299)

pudimos encontrar un usuario sin contraseña, para mí esto significa `hydra`.
Así que manos a la obra.

![imagen](https://github.com/user-attachments/assets/7d762f51-21bd-432a-b8f0-b8eed61d1010)

Efectivamente, encontramos contraseña para ese usuario.
---
4.	Explotación
---
Con toda esta información encontrada tratemos de ingresar por el protocolo SSH  que corresponde al puerto 22.

![imagen](https://github.com/user-attachments/assets/06425751-553e-4a07-a676-364d79d6df3f)
---
5.	Post-explotación.
---
Usamos `sudo -l` a ver qué comandos de root podemos utilizar y con qué usuario. 

![imagen](https://github.com/user-attachments/assets/cc7f8f93-1380-4799-a919-fe6804c4901d)
---

Podemos usar `/bin/bash`, si ingresamos el comando `sudo -u root /bin/bash` deberíamos escalar privilegios.

![imagen](https://github.com/user-attachments/assets/c43d8f2c-e128-4577-85c7-54639d1dae58)
 
Y listo, i am ROOT.
