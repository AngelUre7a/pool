# Bola Ocho

Billar 8 ball para dos jugadores en el navegador. Un solo archivo HTML, sin build,
sin dependencias que instalar y sin servidor propio.

## Cómo se juega

- **En línea**: uno abre una mesa y le manda el link a su amigo. El link lleva el
  código de la mesa en el `#`, así que el otro solo lo abre y entra. Las dos
  computadoras se conectan **directo entre ellas** por WebRTC.
- **Misma pantalla**: se turnan el mouse. No necesita internet más allá de cargar la página.

Apuntás moviendo el mouse, arrastrás hacia atrás desde la blanca para cargar
fuerza y soltás. En celular: tocás para apuntar y arrastrás para cargar.
Con teclado: flechas para apuntar y dosificar, espacio para tirar.

## Reglas implementadas

Mesa abierta hasta la primera bola embocada legal, reparto de lisas y rayadas,
primer contacto legal, obligación de bola a banda después del contacto, saque
corto, la 8 que cae en el saque y vuelve a la mesa, falta con bola en mano donde
sea, y la 8 al final para ganar (o perder, si cae antes de tiempo o con falta).

No están: el efecto (inglés) sobre la blanca y cantar tronera para la 8.

## Publicarlo en GitHub Pages

```bash
git init
git add .
git commit -m "Bola Ocho"
git branch -M main
git remote add origin https://github.com/AngelUre7a/pool.git
git push -u origin main
```

Después, en el repo: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
En un par de minutos queda en `https://AngelUre7a.github.io/pool/`.

## Cómo viaja la partida

Cuando tirás, tu navegador manda solo el tiro: ángulo, fuerza y posición de la
blanca. El del rival vuelve a simular con la misma física de paso fijo, así que
ve exactamente la misma jugada. Al terminar, quien tiró manda el estado real y
hay una firma de control que lo corrige si algo se hubiera desviado.

Eso quiere decir que por la red viajan unos pocos números por tirada, no las
posiciones de las dieciséis bolas cuadro a cuadro.

## Limitaciones que conviene saber

- **Para encontrarse, los dos navegadores usan el servidor público gratuito de
  PeerJS.** Solo sirve para el saludo inicial: una vez conectados, la partida no
  pasa por ahí. Si ese servidor está caído, no se puede abrir mesa nueva.
- **Hay redes donde el P2P no logra conectar.** Con NAT simétrico (bastante común
  en redes universitarias y en algunos operadores móviles) hacen falta servidores
  TURN, que este proyecto no incluye porque no hay ninguno gratuito confiable.
  Si les pasa, el modo de misma pantalla sigue funcionando.
- **La mesa vive en la pestaña del anfitrión.** Si la cierra, se cae la partida.
- Son dos jugadores por mesa. El tercero que entre al mismo código es rechazado.

## Estructura

Todo está en `index.html`, en cuatro bloques:

1. física y reglas del 8 ball
2. dibujo en canvas
3. estado, marcador y entrada del jugador
4. red (WebRTC) y pantallas de lobby

El bloque 1 no toca el DOM ni la red, así que se puede probar aparte con Node.
