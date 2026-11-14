# PROJECT_CONTEXT.md — ARTEMIS ACADEMY · BASE LUNAR
**Fuente Oficial de Referencia Arquitectónica, Científica y de Diseño (Estado Base: v8 Consolidada)**  
*Documento de contexto permanente para agentes de desarrollo (Claude Code) y colaboradores técnicos.*

---

## 1. VISIÓN DEL PROYECTO

### 1.1. ¿Qué es Artemis Academy y por qué existe?
**Artemis Academy · Base Lunar** es un simulador educativo interactivo en tiempo real (vista cenital 2.5D) contenido íntegramente en un único archivo HTML5 portátil (`ArtemisAcademy_v8.html`), sin dependencias ni librerías externas. En él, estudiantes de **7 a 12 años** asumen el rol de **Comandante de un puesto avanzado en el Polo Sur Lunar**, gestionando soporte vital, energía solar/baterías, blindaje radiológico, producción hidropónica de alimentos, extracción de recursos *in situ* (ISRU), salud física/psicológica de la tripulación y retrasos de comunicación.

El proyecto existe para resolver una brecha crítica en la educación STEM espacial: los materiales actuales para niños suelen caer en uno de dos extremos defectuosos:
1. **Divulgación pasiva o cuestionarios abstractos (*quizzes/slideshows*):** Enseñan datos aislados para memorizar (ej. la distancia Tierra-Luna), pero nunca permiten experimentar la lógica de ingeniería detrás de una misión real.
2. **Videojuegos puramente arcade o caricaturescos:** Eliminan las restricciones físicas reales (energía infinita, cultivos instantáneos, ausencia de radiación acumulada), destruyendo el valor formativo.

**Artemis Academy** construye el puente exacto entre la diversión y el rigor científico: un entorno donde las propias leyes físicas y los datos abiertos de la NASA son las mecánicas del juego, y donde **ninguna decisión es gratuita (*no free choices*)**.

### 1.2. Relación Directa con NASA y el Reto de Space Apps Challenge 2026
* **Reto Oficial:** *Build a Junior Astronaut Mission Trainer* (NASA International Space Apps Challenge 2026). El autor del reto (Mike Given) define el espíritu del desafío como una **simulación viva donde cada vatio de potencia, cada litro de oxígeno y cada gramo de blindaje es una compensación (*trade-off*) frente a otra cosa**, dejando claro que *«no se busca un cuestionario ni una presentación, sino una simulación viva donde cada elección tiene un costo»*.
* **Programa NASA Artemis y Estrategia *Moon to Mars*:** El juego modela la arquitectura real del *Artemis Base Camp* en el Polo Sur Lunar como plataforma de validación tecnológica y humana (de misiones cortas de $3\text{ días}$ hasta la misión final de $180\text{ días}$) antes de dar el salto a Marte (representado en el simulador de retraso de señal de Marte y en el gancho narrativo de la introducción).
* **NASA DONKI (*Space Weather Database Of Notifications, Knowledge, Information*):** El juego se conecta vía `fetch` asíncrono a `api.nasa.gov/DONKI/FLR` y `api.nasa.gov/DONKI/CME` (con caché de $6\text{ h}$ en `localStorage` y respaldo histórico curado cuando no hay internet) para transformar llamaradas solares reales (clases C, M, X), eventos de partículas energéticas solares (SEP) y eyecciones de masa coronal (CME) en el clima espacial que gobierna la radiación y las comunicaciones de la partida.
* **NASA OSDR (*Open Science Data Repository*) y *RadLab*:** Inspiran la biología espacial del invernadero (experimento *Veggie*, luces LED rojas/azules, brotes de moho en *Zinnia* por exceso de humedad en microgravedad, cultivo en regolito Apolo con estrés oxidativo), la fisiología humana (pérdida ósea del $1\text{ \%/mes}$ sin ejercicio, tasa de errores por fatiga, *NASA Twins Study*) y la comparación dosimétrica Tierra–EEI–Superficie Lunar (*Chang'e 4* / *Artemis I MARE*).
* **Datos Ambientales Lunares (*LRO/Diviner, LCROSS, M3, NASA ECLSS*):** Sustentan el mapa de 12 sectores reales del Polo Sur Lunar (temperaturas desde $-40\text{ }^\circ\text{C}$ en picos de luz eterna hasta $-240\text{ }^\circ\text{C}$ en cráteres en sombra perpetua PSR), la proporción de agua en el regolito de Cabeus ($\approx 5.6\text{ \%}$ medida por LCROSS) y el límite físico del soporte vital ECLSS ($\le 97\text{ \%}$–$98\text{ \%}$ de recuperación de agua y generación de $\text{O}_2$ por electrólisis dependiente de la electricidad).

---

## 2. OBJETIVO EDUCATIVO Y HABILIDADES STEM

### 2.1. Qué debe aprender el jugador
1. **Acoplamiento sistémico de soporte vital:** Comprender que en el espacio la electricidad ($\text{E}$), el agua ($\text{W}$) y el oxígeno ($\text{O}$) no son independientes; el oxígeno se obtiene separando agua mediante electricidad (electrólisis), por lo que un apagón nocturno detiene la producción de aire respirable.
2. **Física del clima espacial en tres tiempos:** Distinguir entre una **Llamarada Solar (FLR)** (viaja a la velocidad de la luz, corta la radio al instante), **Partículas Energéticas Solares (SEP)** (protones rápidos que llegan en $15\text{–}45\text{ min}$ y obligan a abortar caminatas EVA) y una **Eyección de Masa Coronal (CME)** (nube de plasma que tarda $1\text{ a }3\text{ días}$ y permite preparar escudos y refugios con agua y regolito).
3. **Diferencia entre fatiga y radiación acumulada:** Entender que dormir en los dormitorios recupera el cansancio ($\text{F}$), pero **jamás borra la dosis de radiación ($\text{R}$)**, la cual se acumula y solo se mitiga con menos tiempo de exposición, blindaje rico en hidrógeno y tratamiento médico.
4. **Geografía extrema del Polo Sur Lunar e ISRU:** Descubrir por qué las cimas iluminadas (*Malapert, Leibnitz*) son óptimas para paneles solares verticales (*VSAT*) pero peligrosas en radiación, mientras que los cráteres en sombra perpetua (*Cabeus, Shackleton, Haworth, Shoemaker*) resguardan hielo milenario a $-240\text{ }^\circ\text{C}$ pero descargan las baterías más del doble de rápido por el frío extremo.
5. **Autonomía operativa y retraso de señal:** Experimentar por qué a $1.3\text{ s}$ de distancia (Luna) aún hay apoyo de Houston, pero con un retraso de $3\text{ a }22\text{ min}$ por sentido (Marte) la tripulación debe resolver emergencias mediante procedimientos locales sin esperar órdenes de la Tierra.

### 2.2. Habilidades cognitivas que desarrolla
* **Pensamiento sistémico:** Anticipar cadenas causales y efectos dominó (ej. no limpiar el polvo lunar de los paneles $\rightarrow$ caída de energía $\rightarrow$ falla térmica $\rightarrow$ pérdida de cultivos).
* **Presupuesto cuantitativo con margen de seguridad:** Calcular reservas dividiendo existencias entre tasas de consumo diario y dejando siempre un margen para imprevistos.
* **Toma de decisiones bajo incertidumbre y presión temporal:** Priorizar sistemas críticos para la vida frente a sistemas secundarios (reparto de cargas o *load shedding*).

---

## 3. PÚBLICO OBJETIVO Y ACCESIBILIDAD COGNITIVA

* **Rango de edad:** Niños y adolescentes de **7 a 12 años** (2.º de primaria a 1.º de secundaria), con extensión a docentes STEM y familias.
* **Segmentación dual implementada (`PROF.level`):**
  * **Modo Cadete (7–9 años):** Consumo de la base suavizado ($\times 0.85$), retos con 3 opciones de respuesta, pistas desplegables habilitadas y narración por voz activada por defecto.
  * **Modo Explorador (10–12+ años):** Consumo al $100\text{ \%}$, 4 opciones de respuesta, problemas de tasas horarias y porcentajes.
  * **Escalado dinámico a Comandante (`S.boost`):** Si el alumno encadena 5 respuestas correctas seguidas, sube automáticamente a nivel Comandante (4 opciones sin pistas gratuitas); si falla 3 veces en un mismo tema (`S.fails[cat]`), el juego abre automáticamente la pista en la siguiente oportunidad.
* **Dificultades naturales de este público que el diseño atiende:**
  * Memoria de trabajo limitada (se mitiga con la **visibilidad progresiva `VIS`**, que muestra solo los indicadores esenciales en la Misión 1 y desbloquea el resto misión por misión).
  * Rechazo a la jerga técnica árida (se mitiga con el **traductor automático de jerga `jargon(t)`** y las analogías cotidianas en cada retroalimentación).

---

## 4. FILOSOFÍA DE DISEÑO Y RESOLUCIÓN DE CONTRADICCIONES HISTÓRICAS

A lo largo de las iteraciones del proyecto (v1 a v8) surgieron ideas que evolucionaron o chocaron entre sí. **Las siguientes resoluciones son definitivas y constituyen la doctrina oficial del proyecto:**

1. **Arquitectura de Archivo Único (*Single-File Mandate*) + 100 % Arte y Audio Procedural:**
   * *Resolución:* Todo el juego vive en un único archivo `.html` autocontenido. No depende de hojas de sprites externas, librerías JS externas (ni Three.js ni PixiJS) ni archivos MP3. En la introducción cinematográfica (`INTRO_SLIDES`), el código intenta cargar opcionalmente `intro1.jpg`–`intro3.jpg` si el usuario las coloca en la misma carpeta, pero si no existen (o tardan más de $900\text{ ms}$), dibuja automáticamente en un `<canvas>` de respaldo tres escenas espaciales de alta calidad (`cineFallback(i)`) para que jamás aparezca un icono de imagen rota.
2. **Perspectiva Cenital 2D con Iluminación 2.5D Falsa (Prohibido cambiar a 3D o Isométrico):**
   * *Resolución:* El mapa mantiene estrictamente sus coordenadas 2D cenitales (`ROOMS`, `ST`, `PARKS`, `SECT`) para no romper jamás las colisiones (`okp`, `inR`) ni el minimapa. El volumen visual se logra mediante **2.5D falso**: sombras proyectadas desplazadas, degradados diagonales, biseles de luz/sombra, oclusión ambiental en los bordes de los muros y sprites de brillo radial pre-renderizados (`glowSpr`).
3. **Diseño del Astronauta (Proporción 1:3 y Doble Estado del Visor):**
   * *Resolución de contradicción histórica:* Algunos prompts antiguos pedían eliminar por completo el rostro del personaje, mientras que el sistema de *Credencial* permite personalizar cara, piel, peinado y lentes. La solución consolidada en v8 (`avatarSVG`) es: **proporción estilizada adulta 1:3**; en el **interior de la base** el casco es transparente y muestra el rostro personalizado del cadete; en el **exterior lunar (EVA)** se activa automáticamente el **visor curvo de oro reflectante multicapa** (sin rostro visible) más el haz de luz frontal del casco.
4. **Cero intercambios mágicos («NO intercambiar herramientas por energía»):**
   * *Resolución:* Las herramientas (`tools` 🛠) y los repuestos (`parts` 🔩) son objetos físicos discretos que se ganan en entrenamientos/retos, llegan en naves de suministros o se imprimen en 3D con regolito/roca mediante tecnología ISRU nivel 5. Jamás se convierten mágicamente en electricidad ni se regeneran solos.
5. **Transparencia Epistemológica Radical:**
   * *Resolución:* El juego jamás engaña al estudiante ni al jurado haciendo pasar datos estáticos por datos en vivo. La barra superior muestra siempre si DONKI está sincronizado en tiempo real (`📡 DONKI HH:MM`) o usando el respaldo histórico (`📚 Archivo NASA`), y cada dato lleva su sello (`🟢 DATO NASA`, `🟡 ESCENARIO HISTÓRICO`, `🔵 SIMULACIÓN`).

---

## 5. ESTADO ACTUAL DEL JUEGO (ARQUITECTURA IMPLEMENTADA EN v8)

El código fuente actual (`ArtemisAcademy_v8.html`, $\approx 1\,815\text{ líneas}$ altamente compactadas y consolidadas sin funciones duplicadas) está estructurado en los siguientes subsistemas funcionales:

### 5.1. Arranque, Introducción Cinematográfica y Menú Principal (`#boot`, `#cine`, `menu()`)
* **Pantalla de Arranque (`bootScreen`):** Desbloquea el `AudioContext` y `speechSynthesis` con el botón `INICIAR TRANSMISIÓN` o permite saltar con `Saltar intro [ESC]`.
* **Cinemática Ken Burns (`playIntro`, `cineSlide`):** 3 diapositivas (`INTRO_SLIDES`) con zoom suave CSS, fundido cruzado, respaldo procedural en Canvas (`cineFallback`), texto tipo máquina de escribir con clic de telemetría, filtro musical en crescendo ($380\text{ Hz} \rightarrow 1\,300\text{ Hz}$) y narración de voz sincronizada.
* **Menú en 4 Pestañas (`menu`):** `🚀 Jugar` (continuar partida, nueva partida, selector de 7 misiones, 4 simuladores, historia, fuentes NASA, audio y controles), `🧑‍🚀 Credencial` (personalización de nombre, callsign, dificultad, especialidad y avatar SVG), `🗺️ Trayectoria` (mapa de progreso con estrellas) y `🏆 Récords` (tabla de clasificación local, logros y álbum de cartas).

### 5.2. Mapa 4800×4800, Base Radial y 12 Sectores (`ROOMS`, `ST`, `SECT`, `PARKS`)
* **Interior de la Base (`ROOMS`, centro en `2400, 2400`):** Atrio Central (`atr`) conectado por 4 pasillos (`cN, cS, cE, cW`) a 4 alas (`N, S, E, W`) y 4 esclusas cardinales (`aN, aS, aE, aW`) revestidas con foil térmico dorado (`foil()`).
* **16 Estaciones Interiores y Exteriores (`ST`):**
  * *Atrio:* Centro de control (`ctrl`), Comunicaciones (`ant`), Manual (`man`), Trajes EVA (`suit`).
  * *Ala Norte:* Soporte vital (`life`), Planta ISRU (`isru`).
  * *Ala Sur:* Laboratorio (`lab`), Biblioteca (`lib`), Bahía médica (`med`), Investigación (`res`).
  * *Ala Este:* Red eléctrica (`pow`), Taller 3D (`shop`), Repuestos (`kit`).
  * *Ala Oeste:* Invernadero (`grow`), Dormitorios (`bed`), Cocina y comedor (`galley`).
  * *Exterior:* 4 esclusas/entradas (`hatch*`), 3 parques solares (`PARKS`: Norte, Este, Oeste), Sitio de muestras (`sample`), Rover LTV (`rover`) y Pista de aterrizaje (`PAD`).
* **12 Sectores del Polo Sur Lunar (`SECT`):** 3 anillos (`Zona 1 Perímetro`, `Zona 2 Intermedia`, `Zona 3 Frontera polar`) $\times$ 4 cuadrantes cardinales, incluyendo cráteres reales en sombra perpetua (`Cabeus`, `Shackleton`, `Haworth`, `Shoemaker`) y picos de luz eterna (`Malapert`, `Leibnitz`).

### 5.3. Simulación de Recursos, Tiempo y Clima Espacial (`L()`, `rates()`, `adv()`, `ext()`)
* **Estado Global (`S`):** Almacena recursos `S.res` (`E, O, I, M, F, R, W, A, S`), capacidad `S.cap`, inventario (`tools, parts, sci, smp`), niveles de instalaciones `S.lv` (1 a 5), obra en curso `S.ug`, árbol tecnológico `S.tech` (6 ramas), investigación en curso `S.rs`, salas reveladas `S.rev`, tripulación `S.crew`, clima solar `S.solar`, emergencia activa `S.sec`, catástrofe `S.cat`, rover `S.rover` y bitácora `S.log`.
* **Clima Espacial en 3 Fases (`solarFlare`, `solarTick`, `cmeHit`, `cmePlan`):** Secuencia alimentada por DONKI que dispara Llamaradas (FLR, cortan la radio), Partículas (SEP, cuenta regresiva ámbar para volver a la base antes de que la radiación exterior se dispare) y CME (aviso con $1\text{ a }3\text{ días}$ para preparar escudos eléctricos y refugio en el Centro de Control).
* **Director de Emergencias y Catástrofes (`director`, `launchEv`, `catStart`):** Eventos procedurales de grado 1 a 3 (`EVT`) con fase ámbar de prevención (-30 % daño), efecto dominó (`CASC`) si se elige la opción negligente, protección anti-espiral de derrota (se pausa si hay $\ge 2$ recursos críticos) y catástrofes multi-sala (`CATS_MR`) que requieren coordinar a la tripulación por radio.

### 5.4. Tripulación (`NP`), SocBot (`sb`) y Comunicación
* **Compañeros NPC (`Maya` y `Leo`):** Se unen en la Misión 3 (`recruit()`). Tienen barra de fatiga propia, caminan por la base (`pathIn`), ejecutan órdenes por radio (`renderRadio`, `radioMenu`: cubrir catástrofes, cubrir emergencias, limpiar parques solares, tareas rutinarias, descansar, reparar a SocBot) y pueden acompañar al jugador en **EVA en binomio** (`c.fl = 1`).
* **SocBot (`socbot`, `chatOpen`, `ask2`, `diag`):** Copiloto robótico que sigue al jugador, realiza diagnósticos predictivos cuando un recurso va a agotarse, responde preguntas en lenguaje natural auditando el libro mayor de la partida (`S.tot`), traduce jerga con analogías y ejecuta acciones físicas con sus brazos (congelar emergencias $+20\text{ s}$, patrullar o llevar muestras) con un $12\text{ \%}$ de riesgo de averiarse (`sbBreak`).

### 5.5. Exploración EVA, Iluminación Nocturna/PSR y Rover LTV
* **Movimiento en $1/6\text{ g}$ (`movePlayer`):** Inercia lunar, saltos con propulsor (`Espacio`), huellas en el regolito (`FP`), balizas (`B`) y brújula perimetral (`drawCompass`).
* **Oscuridad Profunda y Conos de Luz (`drawLighting`):** En la noche lunar y en los cráteres PSR (`darkness()` hasta $0.94$), una máscara negra a media resolución cubre la pantalla y solo los conos `destination-out` de la linterna del astronauta o los faros dobles del rover revelan el terreno, los huecos y el brillo de los cristales de hielo.
* **Cráteres No Superpuestos e Integridad del Rover (`genExterior`, `roverHoles`, `roverBreak`, `roverTick`):** Los huecos se generan respetando zonas de exclusión y un margen mínimo de $45\text{ px}$ entre bordes (`d2`). Si el rover cae en un hueco sufre entre $-25\text{ y }-35\text{ \%}$ de integridad (`hp`), sacudida de cámara (`SHK`), chispas e invulnerabilidad de $1.5\text{ s}$. Con $\text{hp} \le 35\text{ \%}$ emite humo y reduce su velocidad; en $0\text{ \%}$ queda varado con luz de emergencia ámbar, genera automáticamente **3 cajas de repuestos (`S.kits`)** en un radio alcanzable de $250\text{ a }650\text{ px}$ y se repara manteniendo pulsada `[E]` durante $2.5\text{ segundos}$.

### 5.6. Entrenamiento (15 Minijuegos), Laboratorio (3 Pasos) y Contenido NASA
* **15 Minijuegos (`MGS`):** `wires, panel, valve, circuit, shield, torque, seq, rover, eva, ice, csi, signal, dust, garden, fix`.
* **Laboratorio de Muestras (`labRun`):** Proceso científico en 3 etapas: (1) Lupa interactiva sobre textura, (2) Clasificación de partículas y (3) Comparación gráfica con referencias reales de la NASA (Apolo, LCROSS, LRO, Veggie) para obtener ciencia (`sci`) y desbloquear las 27 tecnologías (`TECH`).
* **Banco de Preguntas (`QT`), Cartas (`GL`) y Biblioteca (`LIB`):** 66 plantillas paramétricas con sellos de autenticidad, 33 cartas desbloqueables por experiencia y 15 artículos científicos en doble nivel de lectura (*Modo Cadete* / *Modo Científico*).

---

## 6. DECISIONES DEFINITIVAS (CERRADAS — NO REABRIR)

1. **Un único archivo HTML autocontenido:** No separar nunca en archivos `.css`, `.js` o imágenes obligatorias externas.
2. **Estética aeroespacial cinematográfica 2.5D estilizada:** Mantener el estilo visual logrado en v8 (glassmorphism oscuro con bordes cian, foil térmico dorado NASA, radar holográfico, iluminación con bloom/viñeta y personajes 1:3). No volver jamás al estilo plano ni caricaturesco.
3. **Economía física rigurosa:** Las herramientas (`tools`) y repuestos (`parts`) no se compran con energía ni aparecen de la nada; el reciclaje de agua nunca llega al $100\text{ \%}$; la radiación no baja durmiendo.
4. **Tono seguro y constructivo para niños de 7 a 12 años:** Ante un fallo crítico de recursos, la tripulación es evacuada a salvo en cápsula o rescatada por el compañero de binomio, se conservan las tecnologías/áreas desbloqueadas y se muestra el análisis de causas para aprender del error.
5. **Audio 100 % Web Audio API con *Audio Ducking* y TTS limpio:** Nunca usar ondas cuadradas/diente de sierra crudas sin filtrar; mantener el sintetizador pentatónico suave y la función `speakEssential()` que filtra emojis y corchetes antes de hablar.

---

## 7. RESTRICCIONES CRÍTICAS DE INTEGRIDAD (PROHIBIDO ROMPER EN v9)

Cualquier modificación realizada por Claude Code en futuras versiones **TIENE TERMINANTEMENTE PROHIBIDO** alterar o romper los siguientes contratos del código:

1. **Geometría, Coordenadas y Colisiones Intocables:**
   * NO cambiar `WORLD = 4800`, `WC_ = 2400`, ni las coordenadas/dimensiones de `ROOMS`, `ST`, `PARKS`, `PAD` y `SECT`.
   * NO romper las funciones `okp(x, y)`, `inR(x, y, r, m)`, `walkRects()`, `wingOpen(w)`, `roomAt(x, y)` ni `sectorOf(x, y)`.
2. **Núcleo Transaccional y de Simulación:**
   * Toda modificación de recursos o inventario debe seguir pasando por `L(categoria, cambios)` para no corromper el libro mayor (`S.ld` y `S.tot`) que alimenta el Balance Diario y las respuestas de SocBot.
   * Mantener la compatibilidad de serialización JSON en `S` para que `saveGame()` y `loadGame()` (`localStorage` clave `aa_save`) funcionen sin errores de referencias circulares.
3. **Estructura DOM e Identificadores:**
   * NO renombrar ni eliminar los IDs principales del DOM: `#c, #top, #res, #obj, #mm, #radio, #pr, #bigt, #banner, #toasts, #tip, #al, #sl, #pad, #acts, #chat, #tut, #md, #mb, #boot, #cine`.
4. **Presupuesto de Rendimiento (60 FPS):**
   * Mantener siempre el *Frustum Culling* (`vis(x, y, r)` con margen de $150\text{ px}$), las cachés offscreen (`baseCache()`, `roofCache()`, `CHK`, `SPR`, `AVS`), las comparaciones con distancia al cuadrado (`d2`), el tope de `dpr <= 1.5`, el límite de $300\text{ partículas}$ en `PA` y el monitor adaptativo `fpsTick()`.

---

## 8. ORDEN OFICIAL DE PRIORIDADES DEL PROYECTO

Ante cualquier dilema técnico o de diseño en el desarrollo de v9, se aplicará estrictamente esta jerarquía (nunca sacrificar un nivel superior por uno inferior):

1. **Estabilidad absoluta y cero bugs** (carga limpia en consola, guardado/carga intactos, cero bloqueos o *softlocks*).
2. **Fluidez y rendimiento constante (objetivo 60 FPS en escritorio y $\ge 30\text{ FPS}$ en móvil incluso en la Misión 7).**
3. **Alineación pedagógica y mecánica con el reto de NASA Space Apps** (*simulación viva de trade-offs y consecuencias visibles por encima de cuestionarios pasivos*).
4. **Experiencia de usuario sin fricción en los primeros 60 segundos (*Onboarding* inmediato para niños y jueces).**
5. **Calidad visual cinematográfica 2.5D y atmósfera sonora.**
6. **Profundidad científica, trazabilidad de datos abiertos (DONKI/OSDR/Artemis) y rejugabilidad.**
7. **Nuevas funcionalidades o expansiones.**