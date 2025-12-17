# Gestión Remota y Apagado Seguro (Android)

Este documento explica cómo administrar la Raspberry Pi ("JellyPi") desde un dispositivo Android para realizar un apagado seguro y evitar la corrupción de la tarjeta SD al desconectar la corriente.

## 📱 Opción A: App "RaspController" (Recomendada)

Esta opción permite monitorizar la temperatura, el uso de RAM y apagar el dispositivo mediante una interfaz visual.

### 1. Configuración Inicial
1.  Descargar **[RaspController](https://play.google.com/store/apps/details?id=it.Ettore.RaspController)** desde Google Play Store.
2.  Abrir la app y pulsar el botón `+` para añadir un dispositivo.
3.  Rellenar los datos de conexión SSH:
    * **Nombre del dispositivo:** (Elige un nombre, ej: `JellyPi`).
    * **Dirección IP:** La IP local de la Raspberry (ej: `192.168.1.XX`).
    * **Puerto:** `22` (Por defecto).
    * **Usuario:** Tu usuario de Linux (ej: `pi` o `admin`).
    * **Contraseña:** Tu contraseña de Linux.

### 2. Crear Widget de Escritorio (Botón Rápido)
Para apagar el servidor con un solo toque desde la pantalla de inicio de Android:

1.  Mantener pulsado un hueco vacío en el escritorio del móvil.
2.  Seleccionar **Widgets**.
3.  Buscar **RaspController** -> Arrastrar el widget **"Comando SSH"** o **"Apagar"**.
4.  En la configuración del widget:
    * Seleccionar el dispositivo `JellyPi`.
    * Seleccionar la acción: `Shutdown` (Apagar).
5.  **Uso:** Al pulsar el icono, la Raspberry se apagará en unos 60 segundos. Esperar a que la luz verde deje de parpadear antes de desconectar.

---

## ⚙️ Opción B: Botón de Apagado dentro de Jellyfin

Esta configuración permite apagar la Raspberry Pi directamente desde el menú lateral de la aplicación de Jellyfin, sin usar apps externas.

### 1. Dar permisos al usuario Jellyfin (Terminal)
Por seguridad, Linux no deja que Jellyfin apague el sistema. Hay que autorizarlo mediante terminal:

```bash
# Crear regla de sudo para permitir apagado sin contraseña
echo "jellyfin ALL=(ALL) NOPASSWD: /usr/sbin/shutdown" | sudo tee /etc/sudoers.d/jellyfin-shutdown
