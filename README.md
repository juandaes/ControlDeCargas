# Control de Cargas ⚡

App web para registrar las cargas del carro híbrido en el cargador de un tercero, liquidar los kWh a la tarifa acordada y compartir el estado de cuenta con el pago.

Es un solo archivo (`index.html`) publicado con GitHub Pages, como CuentasCompartidas. Los datos se guardan en Firebase Realtime Database (el mismo proyecto de CuentasCompartidas, en la ruta `cargas/main`).

## Qué hace

- **Cargas**: registra fecha, hora de inicio, % de batería al iniciar, hora de fin y total de kWh.
  - **📷 Foto**: toma la foto del cargador, arrastra un recuadro sobre la pantalla (o solo sobre el número) y la app lee los kWh en el teléfono con Tesseract.js. Si encuentra la duración (p. ej. `02:23:39`) y ya pusiste la hora de inicio, calcula la hora de fin. **La foto no se sube ni se guarda**: se procesa en memoria y se descarta al cerrar.
- **Liquidar**: eliges las cargas pendientes y la tarifa (COP/kWh), y la app calcula el valor a pagar. Cada liquidación guarda su propia tarifa y una copia del detalle.
- **Pagos**: registra el pago (fecha y referencia de la transferencia), comparte la **imagen del estado de cuenta** por WhatsApp o copia el **enlace de solo lectura** de esa liquidación.
- **Ajustes**: título, a quién le pagas, tarifa por defecto, enlace de solo lectura del estado de cuenta general (`?estado`), importar líneas de la nota de Google Keep y descargar un respaldo en JSON.

## Publicarla (GitHub Pages)

1. Une la rama con `main` (o publica directamente desde la rama).
2. En GitHub: **Settings → Pages → Build and deployment → Deploy from a branch**, elige la rama y la carpeta `/ (root)`.
3. Queda en `https://juandaes.github.io/ControlDeCargas/`.

## Reglas de Firebase

La app entra con login anónimo (igual que CuentasCompartidas). Si tus reglas de Realtime Database solo permiten la ruta `cuentas`, agrega `cargas`:

```json
{
  "rules": {
    "cuentas": { ".read": "auth != null", ".write": "auth != null" },
    "cargas":  { ".read": "auth != null", ".write": "auth != null" }
  }
}
```

Si las reglas no lo permiten, la app muestra un aviso con el error. Igual que en CuentasCompartidas, cualquiera que tenga el enlace de la app puede abrirla. El modo de solo lectura oculta los botones de edición, pero no es un control de acceso.
