# Personalización Visual y UI de Jellyfin (JellyPi)

Este documento detalla las modificaciones de estilo (CSS), personalización de logotipos y organización de la interfaz de usuario realizadas para transformar la apariencia por defecto de Jellyfin en una interfaz moderna estilo "Streaming Service" (Dark/Glass).

## 1. Código CSS Global (Tema Oscuro y Logo)

Para aplicar este estilo, ir a:
**Panel de Control** -> **Personalización de apariencia** -> **Estilo CSS personalizado**.

Copia y pega el siguiente bloque completo:

```css
/* --- 1. ESTILO GENERAL: TEMA DARK & GLASS --- */

/* Bordes redondeados en todas las tarjetas */
.cardOverlayContainer,
.cardImageContainer,
.cardContent,
.cardScalable,
.itemSelectionPanel,
.cardBox {
    border-radius: 12px !important;
    overflow: hidden !important;
}

/* Fondo Negro Profundo (OLED Style) */
.backgroundContainer, .dialog, .mainDrawer, .drawer-open {
    background-color: #080808 !important;
}

/* Barra Superior Transparente (Efecto Blur) */
.skinHeader {
    background: rgba(0, 0, 0, 0.6) !important;
    backdrop-filter: blur(20px) !important;
    -webkit-backdrop-filter: blur(20px) !important;
}

/* Colores de Acento (Botones y Barras) */
.button-flat:hover, .playstatebutton-icon-played, .rating-button-icon-active {
    color: #aa5cc3 !important; /* Morado Jellyfin */
}
.mdl-slider__background-lower {
    background-color: #aa5cc3 !important;
}

/* Logotipos de Items más grandes en la ficha */
.itemDetailImage {
   border-radius: 20px !important;
   box-shadow: 0 0 20px rgba(0,0,0,0.5);
}

/* --- 2. LOGOTIPO PERSONALIZADO EN LA BARRA --- */

/* Ocultar el logo original */
.pageTitleWithLogo img { display: none !important; }

/* Inyectar el nuevo logo (JellyPi) */
.pageTitleWithLogo {
    /* Reemplazar URL por el enlace directo a la imagen (.png) */
    background-image: url('TU_ENLACE_DIRECTO_AQUI.png'); 
    
    background-repeat: no-repeat;
    background-position: left center;
    background-size: contain;

    /* Ajustes de tamaño y zoom visual */
    width: 150px;
    height: 40px; 
    display: block;
    margin-top: 5px;
    
    /* Efecto Lupa para agrandar sin deformar la barra */
    transform: scale(1.5);    
    transform-origin: left center;
    margin-left: 15px; 
}
```
## 2. Organización de la Pantalla de Inicio
Jellyfin no permite forzar el orden globalmente, se debe configurar **por usuario**.

**Ruta:** Icono de Usuario -> Inicio -> Sección Pantalla de inicio.

### Orden Optimizado (Flujo de Usuario):
1.  **Mis contenidos:** (Muestra las carpetas de bibliotecas arriba del todo).
2.  **Seguir viendo:** (Aparece solo si hay películas a medias).
3.  **Últimos elementos añadidos:** (Novedades recientes).
4.  **Resto de secciones:** `Nada` (Desactivar para limpieza visual).

> **Nota:** La sección "Seguir viendo" se oculta automáticamente si no hay contenido en progreso.

## 3. Imágenes de Bibliotecas (Carátulas)
Para eliminar los iconos de carpetas por defecto y usar imágenes de alta calidad (estilo Netflix):

1.  Ir a **Panel de Control** -> **Bibliotecas**.
2.  Clic en los `...` de la biblioteca (ej: "Películas") -> **Editar imágenes**.
3.  Subir una imagen horizontal (Backdrop/Wallpaper) manualmente.
4.  Repetir para "Series" y "Colecciones".

## 4. Notas Adicionales
* **Logo:** Se requiere un enlace directo (que termine en `.png` o `.jpg`). Sitios como ImgBB o Postimages son recomendados.
* **Actualizaciones:** El código CSS es persistente, pero si una actualización mayor de Jellyfin cambia los nombres de las clases (ej: `.skinHeader`), puede requerir ajustes.
