# 🎱 Bingo de los Torres Peralvo

Bingo de números (1-90) para jugar en familia. La **TV** canta los números con voz y lleva el marcador. Cada jugador escanea un **QR** con su móvil, escribe su nombre y recibe su **cartón** (3 filas × 9 columnas, 15 números, como el bingo español).

Es una web estática (un solo `index.html`). No necesita servidor propio, base de datos ni cuentas.

## Cómo se juega

1. Abre la web en la TV (o en un portátil conectado a la TV) y pulsa **«Esta pantalla es la TV»**.
2. Cada jugador escanea el QR, escribe su nombre y recibe su cartón.
3. En **«Opciones de la partida»** puedes elegir cuántas **líneas** y cuántos **bingos** se premian (por defecto, uno de cada), el **ritmo** (tranquilo / normal / rápido) y si se canta con voz.
4. Pulsa **«¡Comenzar!»**. La TV dice *«¡Comenzamos!»* y va cantando los números sola, a ritmo normal. Puedes pausar, continuar o sacar el siguiente número a mano (barra espaciadora = pausa, flecha derecha = siguiente).
5. Los jugadores van marcando los números en su móvil. Cuando completan una fila cantan **«¡LÍNEA!»** y cuando completan el cartón, **«¡BINGO!»**, con los dos botones del móvil.
6. La TV comprueba sola el cartón contra los números que han salido y anuncia si es correcto (o si todavía faltan números).
7. Al cantarse el bingo, la partida termina. Con **«Nueva partida»** se reparten cartones nuevos en todos los móviles y se puede volver a empezar.

Cada jugador puede ganar como máximo una línea y un bingo por partida. Si se permiten varias líneas o bingos, se van anunciando «primera línea», «segunda línea»…

## Cómo funciona por dentro

- La TV hace de «servidor» de la partida. Los móviles se conectan directamente a ella mediante [PeerJS](https://peerjs.com/) (WebRTC), que usa el servidor público gratuito de PeerJS solo para encontrarse.
- El QR lleva a `?sala=CÓDIGO`, y cada sala tiene un código de 5 letras.
- **La voz**: la TV usa primero `voces.mp3`, un único fichero con los 101 clips pregrabados (los 90 números con sus frases, «¡Comenzamos!», «¡Línea!», «¡Bingo!»…, generados con [Piper](https://github.com/rhasspy/piper), voz `es-carlfm`). Funciona en cualquier navegador, también en los de las Android TV (TV Bro, etc.) que no tienen síntesis de voz. Si el MP3 no se puede cargar, usa la síntesis de voz del navegador (`speechSynthesis`) en español.
- **Modo altavoz**: si aun así la TV no suena, cualquier móvil puede unirse a la sala como altavoz (enlace «Usar este móvil como altavoz» bajo el cartón, o `?sala=CÓDIGO&altavoz`) y canta los números con su propia voz.
- La TV guarda la partida en el navegador (`localStorage`). Si se recarga, recupera la sala, los jugadores, sus cartones y los números cantados.
- Si un móvil se desconecta (por ejemplo, se bloquea la pantalla), al volver recupera su mismo cartón.

**Requisitos:** la TV y los móviles necesitan internet. Lo ideal es estar todos en la misma wifi de casa. Algunas redes muy restrictivas (wifis de empresa, algunos datos móviles) pueden bloquear WebRTC.

## Publicarlo con GitHub Pages

1. Sube `index.html` y `voces.mp3` a un repositorio público (los dos en la raíz).
2. Ve a **Settings → Pages**.
3. En **Source** elige *Deploy from a branch*, rama `main` y carpeta `/ (root)`.
4. En un minuto estará en `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`.
