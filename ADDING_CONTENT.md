# Cómo Añadir Películas y Series a Jellyfin (JellyPi)

Este documento detalla el flujo de trabajo para subir nuevo contenido multimedia al servidor Raspberry Pi desde un ordenador externo (Windows/Mac) y actualizar la biblioteca.

## 📂 Opción A: Vía Red Local (Samba / SMB)
*El método más sencillo: "Copiar y Pegar" desde el Explorador de archivos.*

Para que esto funcione, la Raspberry Pi debe tener configurado Samba (`sudo apt install samba`).

1.  **En Windows:**
    * Abrir el Explorador de Archivos.
    * En la barra de direcciones escribir: `\\IP_DE_TU_RASPBERRY` (Ej: `\\192.168.1.50`).
    * Introducir usuario (`pi`) y contraseña si lo pide.
    * Verás las carpetas compartidas. Simplemente arrastra tus archivos de vídeo a la carpeta `Peliculas` o `Series`.

2.  **En Mac:**
    * Abrir Finder -> Ir -> Conectarse al servidor.
    * Escribir: `smb://IP_DE_TU_RASPBERRY`.
    * Conectar como usuario registrado.

---

## 🚀 Opción B: Vía SFTP (FileZilla)
*Método recomendado para archivos muy grandes o si la red falla.*

1.  Descargar e instalar **[FileZilla Client](https://filezilla-project.org/)**.
2.  **Conexión:**
    * **Servidor:** IP de la Raspberry.
    * **Nombre de usuario:** `pi` (o tu usuario).
    * **Contraseña:** Tu contraseña.
    * **Puerto:** `22`.
3.  Pulsar "Conexión rápida".
4.  En el lado **Izquierdo** están tus archivos del PC.
5.  En el lado **Derecho** está la Raspberry. Navega a la ruta de tus discos (normalmente `/media/tu_disco` o `/mnt/tu_disco`).
6.  Arrastra los archivos de izquierda a derecha.

---

---

## 🔌 Opción C: Método Físico (Conexión Directa al PC)
*El método más rápido para transferencias masivas (ej: muchas películas 4K).*

Este método consiste en apagar la Raspberry Pi, desconectar el disco duro y enchufarlo directamente a tu ordenador principal para copiar los archivos a máxima velocidad.

### Pasos a seguir:

1.  **Apagado Seguro:**
    * Apaga la Raspberry Pi usando el botón de la app, el widget o el comando `sudo poweroff`.
    * Espera a que la luz verde deje de parpadear y desconecta la corriente.
2.  **Conexión al PC:**
    * Desconecta el disco duro USB de la Raspberry Pi.
    * Conéctalo a tu ordenador (Windows/Mac).
3.  **Transferencia:**
    * Copia las películas y series a las carpetas correspondientes dentro del disco.
    * **IMPORTANTE:** Recuerda expulsar el disco de forma segura en tu ordenador antes de desconectarlo ("Quitar hardware con seguridad").
4.  **Reconexión:**
    * Vuelve a conectar el disco duro a la Raspberry Pi (intenta usar el mismo puerto USB que usabas antes).
    * Enciende la Raspberry Pi.
5.  **Verificación:**
    * Jellyfin debería detectar el disco automáticamente al arrancar. Si no aparecen las películas nuevas, dale a "Escanear biblioteca" manualmente.

> **⚠️ Nota sobre Formatos de Disco (Windows vs Linux):**
> * Si tu disco duro está formateado en **NTFS** o **exFAT** (lo estándar en Windows), tu PC lo reconocerá al instante.
> * Si formateaste el disco en **EXT4** (el formato nativo de Linux/Raspberry) para ganar rendimiento, **Windows no reconocerá el disco** al conectarlo. En ese caso, deberás usar una herramienta como *Linux File Systems for Windows* o usar la Opción A (Red).

---

## 🏷️ Nombramiento de Archivos (Importante)
Para que Jellyfin descargue las carátulas y sinopsis correctamente, sigue esta estructura:

### Películas:
```text
/Peliculas
   /Avatar (2009)
      Avatar (2009).mkv
   /The Matrix (1999)
      The Matrix (1999).mp4
```

### Series
```text
/Series
   /Breaking Bad
      /Season 01
         Breaking Bad S01E01.mkv
         Breaking Bad S01E02.mkv
```
Usar siempre el formato S01E01 (Temporada 01, Episodio 01).

### 🔄 Paso Final: Actualizar Jellyfin
Una vez copiados los archivos, Jellyfin necesita "leerlos".
1. Jellyfin suele detectar cambios automáticamente en unos minutos.
2. Para forzarlo:
 * Entra en Jellyfin.
 * Ve a las bibliotecas (o a la biblioteca específica "Películas").
 * Pulsa en los 3 puntos verticales (⋮) de la biblioteca.
 * Selecciona "Escanear biblioteca".
3. En unos segundos aparecerá la carátula y la información.
