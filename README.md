# Monitor NAS — versión Docker

Panel web para vigilar varios NAS y routers de tu red: temperaturas, discos (con horas
de trabajo y desgaste), volúmenes, CPU, interfaces y estado del SAI conectado a un Mikrotik por USB, con gráficas e
histórico.

Es la **misma interfaz** que la app Android de Monitor NAS, servida desde un contenedor,
así que funciona en **cualquier navegador**, incluido **iOS**: Safari → *Compartir →
Añadir a pantalla de inicio* y queda como una app (sin App Store y sin caducidad).

## ¿Por qué un contenedor y no una web normal?

Un navegador no puede hacer **SNMP**, **SSH** ni **ping**, y los NAS no mandan cabeceras
CORS. Este contenedor hace ese trabajo por la web y expone un panel en el puerto **8099**.

## Instalación con Portainer

1. Sube la imagen a Portainer en el apartado imagenes ( el tar.gz ese que va en el comprimido)
2. **Stacks → Add stack**, nombre `nasmon`. con el editor oegas el contenido del yml que va en el comprimido.
3. *(Opcional, recomendado)* En **Environment variables** añade:
   - `NASMON_USER` = el usuario que quieras
   - `NASMON_PASS` = una contraseña
   Con eso el panel pide login al abrirlo.
4. **Deploy the stack** y abre `http://IPLOCALDELNAS:8099`.

```

## Primer uso

1. Abre el panel y pulsa **+** para añadir un equipo.
2. Elige el tipo y rellena:

| Tipo | Qué pide |
|---|---|
| **Synology (DSM)** | IP, puerto 5001, HTTPS, usuario con permisos de administración (acepta 2FA) |
| **QNAP (SNMP)** | IP, puerto 161, comunidad SNMP. Opcional: SSH para horas y desgaste de discos |
| **TerraMaster (TOS)** | IP, SSH (usuario, contraseña, puerto) |
| **Ugreen (UGOS)** | IP, SSH (usuario, contraseña, puerto) |
| **MikroTik (RouterOS)** | IP, puerto 161 y **SNMPv1 o SNMP v2c** (comunidad) o **v3** (usuario + contraseña, SHA-1/MD5, opcional AES/DES) |

3. Cada equipo se refresca solo cada 5 minutos mientras el panel está abierto. Las
   gráficas necesitan **2 muestras** para dibujarse.

## Datos y seguridad

- Usa siempre el **SNMP de solo lectura** y, si el equipo lo permite, un usuario SNMPv3.
- Si vas a abrir el panel desde fuera de casa, ponle `NASMON_USER`/`NASMON_PASS` y
  publícalo por VPN, nunca directamente a Internet.

.
