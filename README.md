# ¡A la frente! — juego tipo "Heads Up" para Android

Te pones el móvil en la frente, sale una palabra y los demás te la explican.
- **Inclina hacia arriba** → ¡Acierto!   **Inclina hacia abajo** → Paso
- Respaldo táctil: mitad derecha = acierto, mitad izquierda = paso.

## Qué incluye
- 10 categorías (1000 palabras) + **Mix** (todas mezcladas) + **Mis palabras** (lista propia, se guarda en el móvil; mantén pulsado para editar).
- Tiempo 30 s / 1 min / 1:30 / 2 min, cuenta atrás 3-2-1, aviso en los últimos 10 s.
- Sonidos generados (sin archivos), vibración nativa, pantalla siempre encendida, menú en vertical u horizontal, partida siempre en horizontal.
- Récord por categoría, resumen de aciertos/pasadas al final.
- Modo "solo mímica" e "Invertir inclinación" (por si algún móvil reporta el sensor al revés).

## Estructura
- `app/src/main/assets/index.html` → **todo el juego** (HTML/JS). Para añadir palabras o categorías edita el array `CATS`.
- `MainActivity.kt` → WebView a pantalla completa, horizontal solo en partida (`Android.setLandscape`), pantalla encendida, botón atrás y puente `Android.vibrate()`.

## Cómo sacar el APK
**Opción A – Android Studio:** File → Open → esta carpeta → Run ▶ (o Build → Build APK).

**Opción B – Sin instalar nada (GitHub Actions):** sube la carpeta a un repo de GitHub.
El workflow `.github/workflows/build-apk.yml` compila solo; descarga el APK desde
la pestaña *Actions* → última ejecución → *Artifacts*. Instálalo en el móvil
(permitiendo "orígenes desconocidos").

**Opción C – Línea de comandos** (con Android SDK instalado): `./gradlew assembleRelease`

> El release se firma con la clave de Oscar (`keystore/alafrente.jks.enc`, cifrada). GitHub Actions la descifra con el secreto `KEYSTORE_PASSWORD`; sin ese secreto se firma con la clave debug.

## Detección del gesto
Se usa el eje Z de la gravedad (perpendicular a la pantalla) con filtro paso bajo:
recto en la frente ≈ 0, pantalla al techo ≈ +9,8 (acierto), pantalla al suelo ≈ −9,8 (paso).
Umbral 6,2 m/s² y hay que volver a la posición recta (<3,8) para la siguiente palabra.
Ajustables en `TH` y `REARM` dentro de `index.html`.
