# Solución al "Error Fatal de Reproducción" en Jellyfin (Raspberry Pi)

Este documento detalla el diagnóstico y la solución para el error de reproducción fatal al intentar reproducir contenido de alta calidad (HEVC/H.265) en un servidor Jellyfin alojado en una Raspberry Pi (3/4/5).

## 🚩 El Problema
Al intentar reproducir una película, el reproductor falla inmediatamente mostrando el mensaje:
> *"La reproducción falló por un error fatal del reproductor."*

Esto ocurre porque Jellyfin intenta utilizar la aceleración por hardware (GPU) pero falla, recurriendo a la CPU, la cual no tiene potencia suficiente para decodificar formatos pesados en tiempo real.

## 🔍 Diagnóstico (Causa Raíz)

El error se debe a una combinación de dos factores:

1.  **Falta de Permisos (La "Llave"):** El usuario del sistema `jellyfin` no tiene permisos para acceder a los dispositivos de renderizado de video (`/dev/dri/renderD128`). Linux bloquea el acceso a la tarjeta gráfica.
2.  **Configuración de Codecs Incorrecta (El "Mapa"):**
    * Se intenta decodificar **AV1** por hardware (la Raspberry Pi no tiene soporte físico para esto).
    * Se deshabilita la decodificación **HEVC** (obligando a la CPU a hacer el trabajo pesado).

## 🛠️ Solución Paso a Paso

### 1. Corregir Permisos del Sistema (Terminal)
Es necesario añadir al usuario `jellyfin` a los grupos de video y renderizado para que pueda "hablar" con la tarjeta gráfica.

Ejecuta el siguiente comando en la terminal de la Raspberry Pi:

```bash
sudo usermod -aG video,render jellyfin

Importante: Reinicia el sistema para aplicar los cambios:
```bash
sudo reboot

### 2. Configuración de Transcodificación (Panel de Control Jellyfin)
Una vez reiniciado, ajusta la configuración en **Panel de Control** -> **Reproducción**:

* **Aceleración por hardware:** Seleccionar `Video Acceleration API (VAAPI)` (Más estable que V4L2).
* **Dispositivo VA-API:** `/dev/dri/renderD128`
* **Decodificación por hardware:**
    * ✅ Marcar: **H264**, **HEVC**, **VC1**.
    * ❌ Desmarcar: **AV1** (La RPi no lo soporta y causará crash), **VP9** (Opcional, suele dar problemas).
* **Opciones de codificación:**
    * ❌ Desmarcar: "Permitir la codificación en formato HEVC" (Dejar que la Pi lea, pero no escriba en este formato pesado).
    * ❌ Desmarcar: "Permitir la codificación en formato AV1".

### 💡 Consejos de Mantenimiento
* **Subtítulos:** Evitar subtítulos de imagen (PGS/VOBSUB). Usar siempre formato de texto (`.srt`) para evitar la transcodificación forzada (burn-in) que consume mucha CPU.
* **Actualizaciones:** Si tras una actualización del sistema (Kernel/OS) vuelve el error, verificar que los permisos del usuario `jellyfin` no se hayan reseteado.
