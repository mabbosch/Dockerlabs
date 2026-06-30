## Pequeñas Mentirosas

Técnicas usadas:

Enumeración de puertos con nmap.

Fuerza bruta con hydra.

Escalada de privilegios Sudo con Python3

## Enumeración

Empezamos la máquina pequeñas mentirosas con un escaneo de puertos abiertos , versiones y posibles vulnerabilidades con nmap.

```bash
# Nmap 7.95 scan initiated Sat Mar 22 04:35:55 2025 as: /usr/lib/nmap/nmap -p- --open -sVC --min-rate 3000 -n -Pn -oN escaneo 172.17.0.2
Nmap scan report for 172.17.0.2
Host is up (0.0000020s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 9e:10:58:a5:1a:42:9d:be:e5:19:d1:2e:79:9c:ce:21 (ECDSA)
|_  256 6b:a3:a8:84:e0:33:57:fc:44:49:69:41:7d:d3:c9:92 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Mar 22 04:36:02 2025 -- 1 IP address (1 host up) scanned in 7.05 seconds
```

El escaneo nos muestra dos unicos puertos abiertos el 80 que pertenece a HTTP con un servidor Apache y el 22 que corresponde a SSH.

Vamos al navegador y nos nuestra una pista.

Reconozco que esta fue la parte que mas me costo de la maquina porque tarde mucho en darme cuenta que A era un usuario, hice infinidad de pruebas antes con fuzzing, burpsuite, inspeccione a fondo la web, etc .. hasta que al final caí en la cuenta que A era un usuario y que la clave seria el password.

Dicho esto, me dispuse a realizar un ataque de fuerza bruta con hydra, y pam! saque el password de SSH. `a:secret`

Nos conectamos por SSH, y vemos que existe otro usuario llamado spencer.

Nuestro usuario actual “a” no tiene ningún tipo de privilegio en nada así que intentamos de nuevo fuerza bruta a spencer con hydra. Sacamos el password de spencer. `spencer:password1`

Nos conectamos por SSH y vemos los privilegios de spencer. El puede usar Python3 con privilegios de superusuario si que probamos de abrir un shell y nos convertimos en root.

La maquina en sí es sencilla pero hay que caer en la cuenta que “a” es un usuario y a mi me costo. Así y todo maquina powneada!!
