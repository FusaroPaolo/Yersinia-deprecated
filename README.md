# Yersinia-deprecated
Investigación sobre alternativas a Yersinia
## Yersinia: Estado deprecado y problemas de la GUI

Yersinia es una herramienta histórica para ejecutar ataques de nivel de enlace (STP, CDP, DTP, DHCP, etc.) en redes LAN. Sin embargo, en su última versión oficial (0.8.2, de 2017) presenta varios problemas:

* **Mantenimiento detenido**: Desde su última liberación, el proyecto no ha recibido actualizaciones importantes, lo que lo deja desfasado frente a librerías modernas (GTK3, nuevas versiones de glibc, etc.).
* **Interfaz gráfica obsoleta**: Su GUI está basada en GTK2, cuya API ha quedado en desuso. Las distribuciones actuales (incluyendo Kali Linux 2025.1) ya no integran de forma nativa muchas de las cabeceras y funciones que Yersinia espera, provocando errores de compilación y ejecución.
* **Dependencias rotas**: Los scripts de configuración (`autogen.sh` y `configure`) y el código fuente no manejan adecuadamente las diferencias de tipos en sistemas de 64 bits, resultando en multitud de errores de compilación.

Por estas razones, compilar Yersinia con su GUI en sistemas modernos resulta impráctico y propenso a fallos.

---

## Alternativas modernas con interfaz gráfica

A continuación se presentan dos herramientas activamente mantenidas que cubren la mayoría de casos de uso de Yersinia y ofrecen interfaces gráficas nativas o basadas en web:

### 1. Ettercap (GUI GTK)

**Descripción:**
Ettercap es una utilidad clásica de ataque tipo MiTM (man-in-the-middle) y sniffing de red. Permite ARP poisoning, DNS spoofing, filtrado de tráfico y plugins adicionales.

**Instalación en Kali Linux:**

```bash
sudo apt update
sudo apt install ettercap-graphical
```

**Ejecución de la interfaz gráfica:**

```bash
sudo ettercap -G
```

Desde la ventana gráfica podrás seleccionar la interfaz de red, especificar hosts objetivo y activar ataques desde los menús “Sniff → Unified sniffing” y “Mitm → ARP poisoning”.

---

### 2. Bettercap (Web UI moderna)

**Descripción:**
Bettercap es la evolución de Ettercap, con arquitectura modular, soporte para HTTPS hijacking, Wi‑Fi, BLE, y más. Ofrece una REST API y una completa interfaz web.

**Instalación en Kali Linux:**

```bash
sudo apt update
sudo apt install bettercap bettercap-ui
```

**Arrancar la Web UI:**

```bash
sudo bettercap -eval "ui on"
```

Luego abre en tu navegador: `http://localhost:8081` para acceder al dashboard gráfico donde podrás activar módulos, ver estadísticas y gestionar ataques.

---

## Conclusión

Aunque Yersinia fue pionera en su campo, su falta de mantenimiento y la obsolescencia de GTK2 hacen inviable su uso en entornos modernos. Ettercap y Bettercap son alternativas robustas, activas y con GUIs cómodas para pruebas de penetración en redes LAN.
