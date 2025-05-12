# Yersinia-deprecated
Investigación sobre alternativas a Yersinia
## Yersinia: Estado deprecado y problemas de la GUI

Yersinia es una herramienta histórica para ejecutar ataques de nivel de enlace (STP, CDP, DTP, DHCP, etc.) en redes LAN. Sin embargo, en su última versión oficial (0.8.2, de 2017) presenta varios problemas:

* **Mantenimiento detenido**: El proyecto no ha recibido actualizaciones importantes desde 2017, quedando desfasado frente a librerías modernas (GTK3, nuevas versiones de glibc, etc.).
* **Interfaz gráfica obsoleta**: Su GUI está basada en GTK2, cuya API ha quedado en desuso. Los sistemas actuales (incluyendo Kali Linux 2025.1) ya no incluyen muchas de las cabeceras y funciones que Yersinia requiere, causando errores de compilación.
* **Dependencias rotas**: El código y los scripts de configuración no manejan adecuadamente las diferencias de tipos en sistemas de 64 bits, generando errores críticos en `admin.c` y otros módulos.

En conjunto, esto hace que compilar Yersinia con GUI en entornos modernos sea impráctico.

---

## Alternativas modernas con interfaz gráfica

A continuación se describen dos herramientas en activo mantenimiento que cubren gran parte de los ataques de capa 2 y ofrecen GUI nativas o basadas en navegador.

### 1. Ettercap (GUI GTK)

**Descripción:**
Ettercap es una herramienta veteran­a de MITM (man-in-the-middle) y sniffing de red. Permite:

* ARP poisoning (envenenamiento ARP)
* DNS spoofing (suplantación DNS)
* Sniffing de protocolos TCP, UDP, HTTP, FTP, etc.
* Uso de plugins para SSL stripping, filtrado avanzado, etc.

**Instalación en Kali Linux:**

```bash
sudo apt update
sudo apt install ettercap-graphical
```

**Ejecución de la GUI:**

```bash
sudo ettercap -G
```

En la interfaz seleccionas la interfaz de red, agregas objetivos y activas los ataques desde los menús **Sniff → Unified sniffing** y **Mitm → ARP poisoning**.

---

### 2. Bettercap (Web UI moderna)

**Descripción:**
Bettercap es la evolución de Ettercap, con arquitectura modular. Soporta:

* ARP poisoning, DHCP starvation, DNS spoofing, ICMP redirect…
* Módulos para HTTPS hijacking, proxy TCP/UDP, Wi‑Fi, BLE, etc.
* REST API completa y Web UI basada en navegador.

**Instalación en Kali Linux:**

```bash
sudo apt update
sudo apt install bettercap bettercap-ui
```

**Arrancar la Web UI (Bettercap v2.33+):**
Se usa el caplet `http-ui` que habilita la UI y la API REST:

```bash
sudo bettercap -caplet http-ui
```

Abre tu navegador en `http://127.0.0.1:8081`.

#### Inicio de sesión (credenciales por defecto)

* **Usuario:** `user`
* **Contraseña:** `pass`

Si necesitas cambiarlas (aunque sea para pruebas locales), puedes hacerlo con variables al arrancar:

```bash
sudo bettercap -caplet http-ui \
    -eval "set api.rest.username admin" \
    -eval "set api.rest.password secret"
```

O editando `/etc/bettercap/config.yml`:

```yaml
http-ui:
  interface: "127.0.0.1"
  port: 8081
api.rest:
  username: "admin"
  password: "secret"
```

Luego reinicia:

```bash
sudo bettercap -caplet http-ui
```

> **Nota:** Si ves errores 404 o no carga la UI, asegúrate de tener copiados los archivos de UI en `/usr/share/bettercap/ui`. Puedes hacerlo con:
>
> ```bash
> sudo cp -r /usr/local/share/bettercap/ui /usr/share/bettercap/
> ```

---

## Uso básico de la Web UI

1. **Navegación**: Barra lateral con módulos (net.probe, net.sniff, arp.spoof, etc.).
2. **Consola integrada**: Ejecuta comandos en tiempo real.
3. **Dashboards**: Estadísticas de tráfico, dispositivos activos, logs.
4. **Activación de módulos**: Haz clic en el botón `On/Off` junto a cada módulo.

Para ayuda interna en la sesión, usa la consola web y escribe `help` o `help ui`.

---

## Conclusión

Yersinia, si bien pionera, ya no es práctica hoy en día. **Ettercap** ofrece GUI GTK tradicional, mientras que **Bettercap** proporciona una interfaz web moderna y extensible. Ambos paquetes están disponibles en los repositorios de Kali y cubren la mayor parte de las necesidades de pruebas de penetración en capa 2.
