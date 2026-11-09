# 🛡️ Enterprise Deception & Active Defense Architecture

Implementación integral de un entorno de **Defensa Activa y Deception Technology** simulando una red corporativa protegida. El proyecto combina el filtrado y redirección de tráfico a nivel de red con un honeypot interactivo de alta fidelidad y un sistema centralizado de visualización y auditoría en tiempo real.

---

## 🎥 Demostración del Laboratorio
*[Inserta aquí tu video o GIF, por ejemplo: `![Demo](Secuencia 01_1.gif)`]*

---

## 🏗️ Topología y Arquitectura del Laboratorio
El entorno fue desplegado en un hipervisor local utilizando **VMware Workstation**, aislado mediante una red privada *Host-Only*:

![Arquitectura del Laboratorio](Gemini_Generated_Image_xttj81xttj81xttj.jfif)

* **Ubuntu Server (Nodo Defensivo / Honeypot):** `192.168.209.130`
  * **Capa 3 y 4 (Red / Transporte):** Implementación de reglas perimetrales con `iptables` y persistencia con `netfilter-persistent` para bloqueo de IPs y redirección transparente de puertos (*Port Forwarding*).
  * **Capa 7 (Aplicación):** Despliegue del honeypot interactivo **Cowrie** (SSH/Telnet) ejecutándose bajo un entorno virtual aislado de Python con un usuario de servicio restringido.
  * **Stack de Monitoreo:** Contenedores Docker corriendo **Loki** y **Promtail** para la recolección de logs, integrados con **Grafana** para la visualización analítica.
* **Kali Linux (Nodo Atacante / Simulación):** `192.168.209.134`
  * Utilizado para ejecutar escaneos de reconocimiento con `nmap`, pruebas de fuerza bruta e intrusión controlada contra los servicios expuestos.

---

## ⚙️ Configuración e Implementación Técnica

### 1. Perímetro y Redirección (Capa 3 y 4)
* **Cualquier intento de conexión dirigido al puerto SSH estándar (`22`) es interceptado a nivel de transporte y redirigido de manera invisible hacia el honeypot en el puerto `2222`:**
```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-ports 2222
```
### 2. Señuelo Interactivo (Capa 7 - Cowrie)
El honeypot emula un sistema Linux vulnerable, aceptando credenciales de forma simulada y permitiendo al atacante interactuar con una shell falsa sin comprometer los sistemas reales.

### 3. Stack de Observabilidad (Docker + Loki + Grafana)
* **Los registros generados por Cowrie son ingeridos en tiempo real por Promtail, almacenados en Loki y procesados en Grafana mediante un Dashboard personalizado que incluye:**

  * Métricas de ataques por minuto (Series temporales).

  * Auditoría de TTPs (Tácticas, Técnicas y Procedimientos) con extracción de comandos específicos ejecutados por los intrusos (whoami, ls, etc.).

  * Contadores de volumen de ataques acumulados.

## 🚀 Conclusiones y Aprendizajes
Este laboratorio permitió comprobar la eficacia de la tecnología de engaño (Deception) frente a botnets automatizadas y atacantes humanos, reduciendo el riesgo operativo y obteniendo inteligencia de amenazas (Threat Intelligence) directa sobre las técnicas de intrusión modernas sin comprometer infraestructura crítica.

**Desarrollado en el marco de la formación técnica en Redes y Ciberseguridad.**
