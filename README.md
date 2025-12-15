# 🎬 Servidor Multimedia con Raspberry Pi (Jellyfin + Tailscale)

> **Documentación Técnica del Proyecto**
>
> Una guía completa para desplegar un "Netflix casero" usando Raspberry Pi 4/5, con aceleración por hardware optimizada y acceso remoto seguro sin abrir puertos.

---

## 📋 Tabla de Contenidos

1. [Hardware y Requisitos](#-hardware-y-requisitos)
2. [Paso 1: Montaje de Disco NTFS (Crítico)](#-paso-1-montaje-de-disco-ntfs-fstab)
3. [Paso 2: Instalación y Permisos de Sistema](#-paso-2-instalación-y-permisos)
4. [Paso 3: Configuración Aceleración por Hardware (V4L2)](#-paso-3-configuración-aceleración-por-hardware)
5. [Paso 4: Acceso Remoto (Tailscale)](#-paso-4-acceso-remoto-seguro)
6. [Solución de Problemas (Troubleshooting)](#-solución-de-problemas-comunes)

---

## 🛠 Hardware y Requisitos

* **Servidor:** Raspberry Pi 4 Model B o Raspberry Pi 5.
* **OS:** Raspberry Pi OS (64-bit recomendado).
* **Almacenamiento:** Disco Duro Externo USB 3.0 (Formato NTFS).
* **Software:** Jellyfin Server, Tailscale VPN.

---

## 💾 Paso 1: Montaje de Disco NTFS (fstab)

*El desafío principal en Linux es que el usuario `jellyfin` tenga permisos totales sobre un disco formateado en Windows (NTFS).*

1.  **Crear punto de montaje:**
    ```bash
    sudo mkdir -p /media/peliculas
    ```

2.  **Obtener el UUID del disco:**
    ```bash
    sudo blkid
    ```
    *(Copia el valor `UUID="XXXX-XXXX"` de tu disco NTFS).*

3.  **Configurar montaje persistente:**
    Editar el archivo fstab:
    ```bash
    sudo nano /etc/fstab
    ```
    Añadir la siguiente línea al final. **IMPORTANTE:** Usar `umask=000` para conceder permisos totales de lectura/escritura a todos los usuarios.

    ```text
    UUID=PEGAR_TU_UUID_AQUI  /media/peliculas  ntfs-3g  defaults,auto,uid=1000,gid=1000,umask=000  0  0
    ```

4.  **Aplicar cambios:**
    ```bash
    sudo mount -a
    ```

---

## ⚙️ Paso 2: Instalación y Permisos

1.  **Instalar Jellyfin (Script automático):**
    ```bash
    curl [https://repo.jellyfin.org/install-debuntu.sh](https://repo.jellyfin.org/install-debuntu.sh) | sudo bash
    ```

2.  **Configurar Permisos de Usuario:**
    Es vital añadir al usuario `jellyfin` a los grupos correctos para que pueda leer archivos del usuario `pi` y usar la tarjeta gráfica.

    ```bash
    # Permiso para leer archivos del usuario principal
    sudo usermod -aG pi jellyfin

    # Permiso para usar la aceleración de hardware (GPU)
    sudo usermod -aG video,render jellyfin
    ```

3.  **Reiniciar servicio:**
    ```bash
    sudo systemctl restart jellyfin
    ```

---

## 🚀 Paso 3: Configuración Aceleración por Hardware

Para evitar que la CPU se sature, configuramos el driver **Video4Linux2 (V4L2)**.

* **Ruta:** Panel de Control > Reproducción (Playback).

| Configuración | Valor a Seleccionar |
| :--- | :--- |
| **Aceleración** | Video4Linux2 (V4L2) |
| **Dispositivo** | `/dev/dri/renderD128` |
| **H.264 / AVC** | ✅ Marcado |
| **HEVC / H.265** | ✅ Marcado |
| **MPEG2 / MPEG4 / VC1** | ✅ Marcado |
| **AV1** | ❌ **DESMARCADO** (No soportado por HW) |
| **VP9** | ✅ Marcado (Opcional) |

> **Nota:** Si se marca HEVC, asegurarse de que el archivo no sea de perfil 10-bit si da problemas, aunque la RPi 4/5 suele soportarlo bien.

---

## 🌍 Paso 4: Acceso Remoto Seguro

Usamos **Tailscale** para crear una red privada (VPN Mesh). Esto evita tener que abrir puertos en el router, protegiendo el servidor de ataques externos.

1.  **Instalar Tailscale:**
    ```bash
    curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh
    ```

2.  **Vincular la Raspberry Pi:**
    ```bash
    sudo tailscale up
    ```
    *(Copiar el enlace que aparece y autenticarse con Google/Microsoft).*

3.  **Invitar amigos:**
    Desde el panel de administración de Tailscale, usar la función "Share Machine" enviando invitación a su correo electrónico.

---

## ⚠️ Solución de Problemas Comunes

### ❌ Error: "Fallo de reproducción fatal" (Fatal Player Error)
Este error suele ocurrir por dos razones principales:

#### 1. Formato de vídeo incompatible (AV1)
* **Síntoma:** Películas modernas en 4K o 1080p rips recientes fallan al instante.
* **Diagnóstico:** Mirar "Info de medios" -> Códec: AV1.
* **Solución:** La Raspberry Pi no tiene chip para decodificar AV1.
    * *Opción A:* Descargar versión H.264 o HEVC.
    * *Opción B:* Usar Jellyfin Media Player en PC (decodifica el cliente).

#### 2. Permisos de Disco
* **Síntoma:** Ninguna película reproduce, incluso las antiguas.
* **Diagnóstico:** Jellyfin no puede leer la carpeta.
* **Solución:** Revisar el **Paso 1**. Asegurarse de que `umask=000` está en `/etc/fstab` y reiniciar.

### 📂 Cómo añadir más películas

* **Método Red (SFTP):** Usar FileZilla conectando a `sftp://IP_LOCAL` (Puerto 22).
* **Método USB (Físico):**
    1.  `sudo poweroff`
    2.  Desconectar disco y conectar a PC para copiar archivos.
    3.  Volver a conectar a Raspberry y encender.
    4.  En Jellyfin: "Escanear biblioteca".
