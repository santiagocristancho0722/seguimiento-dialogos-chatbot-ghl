# Proyecto: Seguimiento Diálogos Chatbot GHL

## Qué es esto
Sistema de seguimiento mensual de la revisión de diálogos del chatbot (Quicktext/Velma) para las ~42 propiedades de GHL Hoteles. Cada mes se envía un PDF a cada hotel para que valide y actualice información operativa (precios, horarios, contactos, políticas, etc.) y este proyecto consolida las respuestas.

## Archivos del proyecto (ÚNICOS — regla crítica)
- `Seguimiento_Dialogos_Chatbot_Consolidado.html` — dashboard interactivo de consulta visual
- `Base_Maestra_Consolidada_Dialogos_Chatbot.xlsx` — base maestra para compartir con otras áreas

**REGLA INQUEBRANTABLE: estos son los ÚNICOS dos archivos del proyecto. NUNCA crear archivos nuevos con sufijos de versión (_v2, _final, _actualizado, fecha, etc.). Siempre editar estos dos archivos in-place.**

## Estructura de datos

### HTML (`Seguimiento_Dialogos_Chatbot_Consolidado.html`)
- Array JS `allData` con un objeto por hotel por ciclo mensual
- Campos: `ciclo` (ej. "Jun-26"), `hotel`, `contacto`, `correo`, `envio`, `respuesta`, `mod` ("Sí"/"No"/"—"), `desc`, `fechaChatbot`, `actualizadoPor`, `comentarios`, `estado`, `mods` (array de diálogos modificados)
- Cada item de `mods`: `{dialogo, nombre, campo, antes, despues}`
- Pestañas por ciclo mensual (barra azul superior) + vista "Todos"
- Barra de % de cumplimiento (hoteles que respondieron) — global y por ciclo
- KPIs: total, respondieron, sin respuesta, con modificaciones, actualizados en chatbot

### Excel (`Base_Maestra_Consolidada_Dialogos_Chatbot.xlsx`)
4 pestañas:
1. **Consolidado** — todas las filas de todos los ciclos. Columna A (Ciclo) con color por mes. Filtros y desplegables (Sí/No/Por confirmar en col G; estados en col L)
2. **Modificaciones detalle** — una fila por diálogo modificado (hotel puede tener múltiples filas por ciclo)
3. **Dashboard cumplimiento** — % de respuesta por ciclo con barra de progreso (caracteres █░) y color scale
4. **Instrucciones de uso** — guía para otras áreas

Colores por ciclo (rotan en este orden): Jun-26 azul (`DBEAFE`/`185FA5`), Jul-26 verde (`D1FAE5`/`065F46`), Ago-26 ámbar (`FEF3C7`/`92400E`), etc.

Estados posibles: `Actualizado` (verde), `Pendiente actualización` (amarillo), `Sin respuesta` (rojo), `Sin modificaciones` (azul claro).

## Flujo de trabajo típico
Cuando el usuario pega el contenido de un correo de un hotel con cambios de diálogos:

1. Identificar el hotel y buscar su fila en el ciclo actual (HTML y Excel)
2. Extraer cada diálogo modificado: número, nombre, campo específico que cambió, valor antes, valor después
3. Actualizar en el HTML: reemplazar el objeto del hotel en `allData` con fecha de respuesta, `mod:"Sí"`, descripción resumen (1 línea con los temas tocados), y el array `mods` completo con cada cambio
4. Actualizar en el Excel:
   - Hoja Consolidado: actualizar fecha respuesta, modificaciones, descripción resumen, estado → "Pendiente actualización"
   - Hoja Modificaciones detalle: eliminar filas placeholder previas de ese hotel/ciclo si existían, agregar una fila por cada diálogo modificado
   - Refrescar Dashboard cumplimiento y Resumen por ciclo con los nuevos totales
5. Nunca usar texto genérico tipo "Ver correo" o "Por confirmar" si el dato exacto está disponible en el correo — extraer el valor real

## Estilo de las descripciones resumen
Una sola línea, formato: "N diálogos modificados: tema1, tema2, tema3..." — listar los temas principales tocados (ej. "contacto recepción, desayuno, mascotas, restaurante").

## Nuevo ciclo mensual
Para abrir un nuevo mes (ej. Jul-26):
1. HTML: duplicar el bloque completo de 42 hoteles del ciclo anterior, cambiar `ciclo` y `envio`, dejar `respuesta`, `mod`, `desc`, `mods` vacíos/neutros
2. Excel hoja Consolidado: igual, duplicar 42 filas con el nuevo ciclo y su color correspondiente
3. Actualizar Dashboard cumplimiento y Resumen por ciclo con la nueva fila de ciclo

## Identidad de marca GHL (si se requiere para otros entregables)
- Navy `#023859`, mid blue `#2a5e95`, accent blue `#6399ba`
- Tipografía: DM Serif Display + DM Sans (Plantilla 2) o Nunito Sans (Plantilla 1/legacy)

## Notas de calidad
- Si un correo tiene un dato que parece error de digitación (ej. precio absurdo), señalarlo al usuario en vez de asumir
- Si un correo llega cortado/truncado, marcar el campo con "⚠ Verificar en correo" en vez de inventar el valor
- Mantener consistencia: mismo hotel debe tener exactamente el mismo nombre en ambos archivos (ver lista oficial de 42 hoteles abajo)

## Lista oficial de 42 hoteles (nombre exacto a usar siempre)
Arsenal, Biohotel, Bioxury, Geotel Antofagasta, Geotel Calama, GHL Collection 93, Armeria Real, GHL Collection Hamilton, GHL Grand Villavicencio, GHL Hotel Capital, GHL Lago Titicaca, GHL Montería, GHL Portón Medellín, GHL Relax Club Hotel el Puente, GHL Relax Hotel Corales de Indias, GHL Relax Hotel Costa Azul, GHL Relax Hotel Sunrise, GHL Style Barrancabermeja, GHL Style Bogotá Occidente, GHL Style Neiva, GHL Style Yopal, Hotel Tequendama Bogotá, GHL Hotel Abadía Plaza, GHL Hotel Grand Barranquilla, LATAM XELA, Makani Luxury Wanderlust, Park Lake Luxury, San Lazaro Art, Sonesta Bucaramanga, Sonesta Cali, Sonesta Cartagena, Sonesta Cusco, Sonesta El Olivar, Sonesta Arequipa, Sonesta Bogotá, Sonesta Ibagué, Sonesta Loja, Sonesta Osorno, Sonesta Pereira, Sonesta Puno, Sonesta Yucay, Sonesta Valledupar
