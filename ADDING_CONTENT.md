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
