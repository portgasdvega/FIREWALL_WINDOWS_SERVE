# Configuración Avanzada de Firewall en Windows Server 2019

## Descripción del Proyecto

Este proyecto demuestra la implementación de un conjunto de reglas personalizadas en **Windows Defender Firewall con seguridad avanzada** para proteger un servidor **Windows Server 2019** que ofrece servicios críticos. Se simula un entorno corporativo donde se permite sólo el tráfico esencial y se bloquea cualquier conexión no autorizada.

Las reglas fueron validadas desde una máquina cliente **Parrot OS** en la misma red, usando herramientas como `ftp`, `telnet`, `curl`, `nslookup`, `smbclient` y `xfreerdp`.

---

## Escenario Simulado

La empresa ficticia **SecureHost Inc.** desea proteger su servidor contra accesos no autorizados, asegurando el funcionamiento únicamente de los servicios necesarios:

### Servicios permitidos:

* HTTP (puerto 80)
* HTTPS (puerto 443)
* DNS saliente (puerto 53 UDP)
* Compartición de archivos SMB (puertos 139 y 445 TCP)
* Escritorio remoto (RDP, puerto 3389 TCP) únicamente desde una IP autorizada

### Servicios bloqueados:

* FTP saliente (puerto 21 TCP)
* Telnet saliente (puerto 23 TCP)
* Todo tráfico entrante no solicitado

---

## Reglas Aplicadas

| Nombre de Regla                        | Tipo                 | Dirección | Puerto  | Protocolo | Acción   | IP Remota       |
| -------------------------------------- | -------------------- | --------- | ------- | --------- | -------- | --------------- |
| Bloqueo FTP Saliente                   | Personalizada        | Salida    | 21      | TCP       | Bloquear | Todas           |
| Bloqueo Telnet Saliente                | Personalizada        | Salida    | 23      | TCP       | Bloquear | Todas           |
| Permitir HTTP Entrante                 | Puerto               | Entrada   | 80      | TCP       | Permitir | Todas           |
| Permitir HTTPS Entrante                | Puerto               | Entrada   | 443     | TCP       | Permitir | Todas           |
| Permitir DNS Saliente                  | Puerto               | Salida    | 53      | UDP       | Permitir | Todas           |
| Permitir SMB Entrante                  | Personalizada        | Entrada   | 139/445 | TCP       | Permitir | 192.168.13.0/24 |
| Permitir RDP desde Parrot              | Personalizada        | Entrada   | 3389    | TCP       | Permitir | 192.168.13.1    |
| Bloqueo tráfico entrante no solicitado | Configuración global | Entrada   | Todos   | Todos     | Bloquear | Todos           |

---

## Validaciones desde Parrot OS

### Pruebas de bloqueo:

* `ftp 192.168.13.130` → bloqueado
* `telnet 192.168.13.130 23` → bloqueado

### Pruebas de acceso permitido:

* `curl http://192.168.13.130` → responde HTML de IIS
* `nslookup google.com 192.168.13.130` → responde con IP
* `smbclient -L //192.168.13.130 -U Administrador` → lista recursos compartidos
* `xfreerdp /v:192.168.13.130 /u:Administrador /p:'R@mera66+' /cert:ignore` → acceso exitoso a RDP

---

## Evidencias (capturas)

Las capturas se encuentran en `Capturas de Configuracion de Firewall` y `Captura de Pruebas` e incluyen:

* Reglas configuradas (entrada/salida)
* Pruebas fallidas (FTP, Telnet)
* Pruebas exitosas (HTTP, DNS, SMB, RDP)

---

## Conclusión

Este proyecto demuestra la aplicación efectiva de una **política de lista blanca** en un servidor Windows Server 2019, restringiendo el tráfico a solo lo esencial y validando desde un sistema Linux externo.

El control de acceso por IP, la segmentación de servicios, y la exportación de configuraciones respaldan una buena práctica de seguridad en entornos reales.

---

🔗 Proyecto documentado por: **Emiliano Martínez Vega**
