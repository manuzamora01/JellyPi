# Sistema de Peticiones y Reporte de Errores

Este documento explica cómo integrar un sistema externo de solicitudes (Google Forms) directamente en la interfaz de inicio de sesión de Jellyfin, permitiendo a los usuarios pedir contenido o reportar fallos sin salir del ecosistema.

## 1. Crear el Formulario (Backend)
Se utiliza Google Forms por su simplicidad y capacidad de exportar a Excel automáticamente.

1.  Crear un nuevo formulario en [Google Forms](https://forms.google.com/).
2.  Campos recomendados:
    * **Nombre de Usuario** (Respuesta corta).
    * **Tipo de Solicitud** (Desplegable: Película, Serie, Error de reproducción).
    * **Título / Descripción** (Párrafo).
3.  Obtener el enlace público del formulario (botón "Enviar" -> Icono enlace -> "Acortar URL").

## 2. Integración en Jellyfin (Frontend)
Dado que las versiones recientes de Jellyfin (10.9+) han eliminado la opción de enlaces en el menú lateral, se utiliza una inyección de HTML en el mensaje de bienvenida de la pantalla de Login.

**Ruta:**
Panel de Control -> General -> Sección "Personalización" -> **Mensaje de aviso de inicio de sesión**.

### Código HTML del Botón
Copiar y pegar el siguiente bloque en el cuadro de texto. Sustituir `TU_ENLACE_AQUI` por la URL del formulario.

```html
<div style="text-align: center; margin-top: 20px;">
  <p style="color: #ddd; margin-bottom: 15px; font-size: 0.9em;">¿No encuentras lo que buscas?</p>
  <a href="TU_ENLACE_AQUI" target="_blank" 
     style="background-color: #aa5cc3; color: white; padding: 10px 20px; text-decoration: none; border-radius: 8px; font-weight: bold; font-family: sans-serif; box-shadow: 0 4px 6px rgba(0,0,0,0.3); transition: background 0.3s;">
     📝 PEDIR PELÍCULA / REPORTAR
  </a>
</div>
```

### 3. Resultado
El botón aparecerá centrado justo debajo del formulario de usuario/contraseña. Es visible tanto en navegadores web como en la mayoría de aplicaciones de Smart TV y móviles antes de iniciar sesión.

### 4. Gestión
El administrador solo necesita consultar periódicamente la hoja de cálculo vinculada al Google Form para ver las nuevas peticiones ordenadas por fecha.
