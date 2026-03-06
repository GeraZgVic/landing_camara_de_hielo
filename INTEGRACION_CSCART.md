# Integracion CS-Cart - Landing Camara de Hielo

## Objetivo
Este proyecto se esta usando para definir visualmente la nueva landing de camaras de refrigeracion antes de integrarla al entorno real de CS-Cart.

Las propuestas actuales son solo de validacion visual. Cuando se apruebe una, el siguiente paso sera convertirla a una version productiva compatible con el backend de formularios de CS-Cart.

## Archivos importantes

### Referencia productiva actual
- `referencia-landing-production.html`

Este archivo muestra como se integra una landing real dentro de CS-Cart:
- usa un bloque `<style>...</style>` inline
- no usa `<!doctype>`, `<html>`, `<head>` ni `<body>`
- contiene solo CSS + markup embebible
- usa clases propias encapsuladas
- incluye `!important` en propiedades clave para resistir estilos del theme de CS-Cart
- conecta el formulario al backend real de CS-Cart

### Formulario real a reutilizar
- `codigo-a-tomar-para-index.html`

Este archivo contiene el formulario real del backend que se debe usar cuando se pase a produccion.

Datos importantes detectados:
- `page_id="71"`
- `form_values[123]`: nombre
- `form_values[128]`: correo
- `customer_email="128"`
- `form_values[129]`: telefono
- `form_values[130]`: espacio aproximado
- `form_values[135]`: estado
- `dispatch[pages.send_form]`

Nota:
- `security_hash` probablemente no sea necesario colocarlo manualmente, porque CS-Cart suele inyectarlo solo.
- Los `value` de los `select` si deben respetarse tal cual en produccion.

### Propuestas visuales
- `index.html` -> Propuesta 1
- `propuesta-2.html` -> Propuesta 2
- `propuesta-3.html` -> Propuesta 3

## Estado actual del proyecto
- ya existen 3 propuestas visuales publicadas para revision
- se optimizaron pensando en trafico principalmente mobile
- se agrego navbar temporal para comparar propuestas
- ese navbar es temporal y despues se eliminara
- las propuestas 1 y 2 ya fueron ajustadas para presentar el logo dentro del bloque principal, igual que la 3
- las propuestas tienen como fondo local `img/bg-body.jpg`
- se usan imagenes reales de instalacion:
  - `img/1.jpeg`
  - `img/2.jpeg`
  - `img/3.png`

## Decision importante sobre integracion
Cuando se apruebe el diseño final:

No se debe subir el HTML completo tal como esta en estas propuestas.

Se debe crear un archivo nuevo orientado a CS-Cart que:
- no use `<!doctype>`
- no use `<html>`
- no use `<head>`
- no use `<body>`
- solo tenga:
  - un bloque `<style>...</style>`
  - el markup HTML de la landing

Esto debe seguir el mismo patron de `referencia-landing-production.html`.

## Consideraciones CSS para CS-Cart
CS-Cart y su theme pueden inyectar estilos globales sobre:
- `body`
- `h1`, `h2`, `p`
- `input`, `select`, `button`, `label`
- formularios
- tipografias
- margenes y paddings globales

Por eso, al integrar a produccion:
- encapsular todo dentro de un contenedor raiz unico
- usar clases con namespace propio, por ejemplo prefijos tipo `lp-...`
- evitar selectores genericos como `h1 {}` o `input {}`
- preferir selectores como `.lp-camara h1 {}` o `.lp-camara .lp-input {}`
- usar `!important` de forma quirurgica en propiedades sensibles

### Donde probablemente si usar `!important`
- layout
- display
- width
- padding
- margin
- background
- border
- color
- font-size
- position
- box-shadow
- elementos de formulario

Especialmente en:
- `input`
- `select`
- `button`
- bloques del hero
- textos principales

## Flujo correcto cuando aprueben una propuesta
1. Elegir la propuesta final.
2. Crear un nuevo archivo para integracion productiva en CS-Cart.
3. Convertir la propuesta elegida a fragmento embebible.
4. Integrar el formulario real de `codigo-a-tomar-para-index.html`.
5. Respetar:
   - `page_id`
   - `form_values[...]`
   - `customer_email`
   - `dispatch[pages.send_form]`
   - `value` reales de los `select`
6. Blindar los estilos frente al theme de CS-Cart.
7. Probar dentro del entorno real.

## Nota sobre mobile UX
Se definio como criterio importante que, en mobile:
- el usuario vea primero el contexto del producto
- despues el formulario
- y luego el bloque visual de apoyo

Esto ya se tomo en cuenta en las propuestas actuales.

## Repositorio
El repositorio local ya fue inicializado.

Estado realizado:
- `README.md` creado
- commit inicial hecho
- rama renombrada a `main`
- remoto configurado a:
  - `https://github.com/GeraZgVic/landing_camara_de_hielo.git`

El `push` fallo por autenticacion de GitHub en la maquina local.

## Recordatorio final
La referencia tecnica real para la integracion no son las propuestas HTML completas.

La referencia tecnica real es:
- `referencia-landing-production.html` para estructura CS-Cart
- `codigo-a-tomar-para-index.html` para el formulario real
