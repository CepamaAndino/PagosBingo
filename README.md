# Pagos Bingo Andino 2026

Dashboard público (GitHub Pages) con el avance de pagos del Bingo Andino por curso, contando **1 alumno por familia**.

- Las compras de cartones se leen **en vivo** del Google Sheet de la app del bingo (solo lectura, no se modifica nada).
- Las delegadas marcan **Pagó a delegada** (con monto) o **Pagó hermano/a** y los totales se recalculan para todos.
- No se necesita cuenta: basta el link.

```
index.html            ← la página (va en GitHub Pages)
apps-script/Code.gs   ← el backend (va en Google Apps Script, NO se publica en GitHub)
```

> La nómina de alumnos vive dentro de `Code.gs` (en tu Google), no en el repo. Si el repo es público, **no subas la carpeta `apps-script/`**.

## 1. Backend (Apps Script) — 5 minutos

1. Abre el Sheet **"Pagos Bingo Andino 2026"** (ya está creado en tu Drive).
2. En ese Sheet: **Extensiones → Apps Script**. Borra lo que haya y pega todo `apps-script/Code.gs`.
3. Arriba del código, en `BINGO_SHEET_ID`, pega el ID del Sheet de la app del bingo (la parte entre `/d/` y `/edit` de su URL). Si la pestaña de compras no se llama `Cartones`, cambia `BINGO_TAB` (si no, la busca sola).
4. Guarda. Elige la función **`probar`** y dale **Ejecutar** → acepta los permisos. En el registro debe decir cuántas compras leyó y si alguna quedó "sin asignar".
5. **Implementar → Nueva implementación → Tipo: Aplicación web**
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier persona**
   - Copia la URL que termina en `/exec`.

La pestaña **Pagos** se crea sola con el primer check. Ahí queda el registro de cada pago (familia, tipo, monto, quién marcó y cuándo).

## 2. Página (GitHub Pages)

1. En `index.html`, reemplaza `PEGA_AQUI_LA_URL_DEL_APPS_SCRIPT` por la URL `/exec` del paso anterior.
2. Sube `index.html` a un repo nuevo (por ejemplo `CepamaAndino/PagosBingo`).
3. En el repo: **Settings → Pages → Branch: main / root → Save**.
4. El link queda: `https://cepamaandino.github.io/PagosBingo/`

## Cómo funciona el conteo

- Cada familia cuenta **una vez**. Si compró, cuenta en el curso donde compró (si compró en varios, el del hijo menor); si no, en el curso del hijo menor.
- Los hermanos que estudian en otro curso aparecen en su curso en **morado** ("Pagó por hermano/a") cuando la familia ya pagó.
- Una compra con "curso completo" en el nombre del alumno deja ese curso al 100%.
- Compras **pendientes** se muestran aparte y no suman al %. Compras rechazadas no cuentan.

## Si una compra queda "sin asignar"

Aparece al final de la página. Crea una pestaña **Asignaciones** en el Sheet de pagos:

| ID compra | Familia |
|---|---|
| C-0050 | F073 |
| C-0060 | CURSO:IV Medio |

El ID de familia (`F073`) lo ves en la pestaña **Pagos** o pidiéndoselo a quien mantiene el sistema.

## Cambios en el código del Apps Script

Si editas `Code.gs`, vuelve a publicar: **Implementar → Administrar implementaciones → ✏️ → Versión: Nueva → Implementar**. La URL no cambia.
