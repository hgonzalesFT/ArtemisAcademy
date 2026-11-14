# ROADMAP.md — HOJA DE RUTA OFICIAL DE ARTEMIS ACADEMY
**Plan Estratégico de Evolución hacia el Ganador Global de NASA Space Apps Challenge 2026**  
*Documento operativo para guiar la implementación de Artemis Academy v9 con Claude Code y versiones posteriores.*

---

## 1. ESTADO ACTUAL DEL PROYECTO (LÍNEA BASE: v8)

### 1.1. Qué funciona hoy a nivel de excelencia
* **Motor Gráfico 2.5D y Rendimiento (60 FPS):** Terreno lunar en $3 \times 3$ chunks cacheados (`CHK`), arquitectura interior y techos pre-renderizados en offscreen canvas (`BC`, `RFC`), *frustum culling* (`vis()`), sprites de iluminación cacheados (`SPR`), post-procesado adaptativo (`applyPostFX` con bloom, tinte ambiental y viñeta) y control automático de calidad según FPS (`fpsTick`).
* **Identidad Visual y Sonora Aeroespacial:** Introducción cinematográfica con efecto *Ken Burns* y respaldo procedural en Canvas (`cineFallback`), HUD con *glassmorphism* NASA e iconos vectoriales SVG (`ICONS`), radar táctico con barrido rotatorio (`mmap`), astronautas en proporción 1:3 con visor dorado en EVA (`avatarSVG`), sintetizador musical Web Audio API con *audio ducking* y narración TTS limpia (`speakEssential`).
* **Exploración EVA y Mecánica del Rover en Oscuridad Profunda:** Máscara de oscuridad (`rgba(2,5,12,0.94)`) recortada en tiempo real por la linterna del casco y los faros dobles del rover; cráteres generados sin solapamiento (`45 px` de margen libre y zonas de exclusión); sistema completo de integridad del rover (`hp`), daño con sacudida de cámara y chispas, avería al llegar a `0 %`, generación anti-bloqueo de 3 kits de repuestos (`S.kits`) y reparación en campo manteniendo `[E]` por $2.5\text{ s}$.
* **Integración Científica y de Datos Abiertos NASA:** Conexión en vivo a `api.nasa.gov/DONKI` (FLR y CME) y `osdr.nasa.gov` con respaldo histórico curado honestamente etiquetado, sellos de autenticidad (`🟢 DATO NASA`, `🟡 ESCENARIO HISTÓRICO`, `🔵 SIMULACIÓN`), verificador de fórmulas en vivo (`formula()`), laboratorio interactivo de 3 pasos (`labRun`), 15 minijuegos STEM (`MGS`), árbol tecnológico de 6 ramas (`TECH`), tripulación por radio (`NP`) y copiloto socrático (**SocBot**).

### 1.2. Qué problemas o cuellos de botella existen hoy en v8
A pesar de tener un motor sistémico digno de un ganador mundial, la auditoría frente a los criterios del jurado y al video oficial del autor del reto (Mike Given) revela tres cuellos de botella críticos en la experiencia inicial:
1. **Las tareas de misión (`MS`) abren con un cuestionario (`ask(t)`) en lugar de una decisión operativa de simulación:** Aunque el juego posee un motor vivo de recursos y 15 minijuegos interactivos, casi todas las tareas de las Misiones 1 a 7 tienen `a[2] = 1` (`kind: 'q'`), lo que abre una pregunta de opción múltiple A/B/C/D al interactuar con la estación objetivo. Esto choca frontalmente con la frase más importante del autor del reto: *«Not a quiz, not a slideshow, a living simulation where every choice has a cost»*.
2. **Fricción excesiva en el *Onboarding* (8 pantallas antes de jugar la Misión 1):** Un juez o niño que abre el juego por primera vez debe pasar por: (1) Pantalla `#boot`, (2) 3 diapositivas `#cine`, (3) Menú principal, (4) Redirección forzosa a la pestaña `Credencial` porque `PROF.name` está vacío (`if (!PROF.name) { menu('cred'); return; }`), escribir un nombre y guardar, (5) Volver a pulsar Jugar, (6) Modal de introducción de la Misión 1, (7) Modal de controles y (8) 6 globos del tutorial `#tut`. Esto retrasa el *Time-to-Wow* a más de 3 minutos.
3. **Falta un panel directo de asignación/reparto de energía en tiempo real (*Power & Life Support Allocation*):** Actualmente el consumo de cada módulo en `rates()` ocurre automáticamente y el jugador solo puede activar un botón general de `"Modo ahorro"` en `pow` o apagar `isru`. Falta permitir que el comandante distribuya activamente la energía entre **Soporte Vital (ECLSS)**, **Control Térmico/Escudos**, **Invernadero** y **Ciencia/Comunicaciones**, viendo al instante cómo subir uno obliga a sacrificar otro.

---

## 2. PROBLEMAS DETECTADOS (CLASIFICACIÓN POR DOMINIO)

| Dominio | Problema Detectado en v8 | Impacto en Evaluación Space Apps |
| :--- | :--- | :--- |
| **1. Educación** | Las tareas principales de las misiones (`runTask` $\rightarrow$ `ask`) evalúan al jugador con preguntas de opción múltiple A/B/C/D antes de dejarle activar la máquina, haciendo que los primeros minutos se sientan como un examen escolar en lugar de un simulador de ingeniería. En modo `cadete` (7–9 años), algunas preguntas de `QT` exigen divisiones y tasas combinadas propias de 10–12 años. | **CRÍTICO:** Riesgo de que un juez aplique el filtro *"Not a quiz"* en los primeros 2 minutos de prueba. |
| **2. Jugabilidad** | Falta una consola interactiva de **Reparto de Cargas (*Load Shedding / Power Allocation*)** donde el jugador ajuste en vivo qué porcentaje de energía recibe el Soporte Vital, la Calefacción, el Invernadero y la Ciencia. Además, en la Misión 2 el evento `crisis('drill')` lanza un quiz en vez de un dilema operativo de 10 segundos como el que describe el autor del reto en su video. | **ALTO:** Desaprovecha el corazón mecánico del reto (*"cada vatio de potencia cuesta oxígeno o ciencia"*). |
| **3. Interfaz (UI/UX)** | Bloqueo obligatorio de formulario de texto (`!PROF.name`) antes de permitir iniciar la Misión 1 o los simuladores; exceso de modales consecutivos al arrancar M1; falta un botón de acceso directo para jueces en el menú principal. | **ALTO:** Eleva el *Time-to-Wow* y añade fricción innecesaria durante la evaluación del jurado. |
| **4. Ciencia y Datos** | La conexión en vivo con **NASA DONKI** y los escenarios históricos reales (como la tormenta G5 de mayo de 2024 o agosto de 1972) son excelentes, pero el selector para cambiar de escenario climático está oculto dentro de submenús (`Ciencia y Fuentes` o `Centro de Meteorología`), en lugar de poder conmutarse en 1 clic desde la barra superior `#top`. | **MEDIO:** Un juez rápido podría no notar que puede probar la base bajo distintas tormentas reales de DONKI. |
| **5. Narrativa** | Al saltar o terminar la introducción cinematográfica (`#cine`), la función `intro()` ya fue unificada, pero al entrar a la Misión 1 se acumulan el modal de misión, el modal de controles y el globo 1 del tutorial simultáneamente. | **MEDIO:** Satura de texto el primer minuto dentro de la base. |
| **6. Visuales** | Aunque el mapa y el HUD ya lucen profesionales en v8, las relaciones causales entre sistemas ($\text{Energía} \rightarrow \text{Electrólisis O}_2 / \text{Reciclaje H}_2\text{O} \rightarrow \text{Salud}$) se muestran solo como números en tablas, sin un diagrama visual de flujo que un niño de 7 a 9 años comprenda de un vistazo. | **MEDIO:** Un esquema visual de flujos haría evidente el pensamiento sistémico. |
| **7. Rendimiento** | El motor ya corre a 60 FPS gracias al cacheo en v8. El único cuidado es garantizar que cualquier interfaz nueva en v9 no invalide `BC` (caché de la base) ni regenere el DOM de `#top`/`#res` más de 4 veces por segundo. | **BAJO (Controlado):** Mantener la disciplina actual de caché. |

---

## 3. PLAN DE LANZAMIENTO POR VERSIONES

### 3.1. ARTEMIS ACADEMY v9 (Objetivo: Ganador Global de Space Apps 2026)
**Filosofía de v9:** *Cero cirugía destructiva; máximo impacto en los primeros 90 segundos de juego y transformación del bucle de misiones de "Quiz" a "Simulación Operativa de Trade-Offs".*

1. **[V9.1] Reemplazar el bloqueo de Quiz (`ask(t)`) en las tareas de misión por Decisiones Operativas de Ingeniería + Minijuegos de Sistema:**
   * Al pulsar `[E]` en una estación con objetivo de misión activo (`runTask(t)`), en lugar de abrir siempre una pregunta A/B/C/D, abrir un **Panel de Decisión Operativa del Comandante** donde el jugador elija entre **3 modos reales de ejecutar la operación** (cada uno con un *trade-off* explícito de recursos y tiempo, sin opción gratuita: ej. *Modo Intensivo: $-10\text{⚡}$, termina rápido* vs. *Modo Equilibrado: $-5\text{⚡}, -3\text{🫁}$* vs. *Modo Manual con cautela: $-2\text{⚡}, +8\text{😴 fatiga}$, tarda más*) **o** resolver directamente el minijuego de simulación de esa estación (`mgP` en paneles, `mgC` en red eléctrica, `mgV` en soporte vital, `mgS` en blindaje), dejando las preguntas `GQ()` como retos opcionales de bonificación o de la Biblioteca/Cartas.
2. **[V9.2] Consola Interactiva de Reparto de Potencia (*Power & Life Support Allocation*) en Red Eléctrica y Centro de Control:**
   * Añadir en `stM('pow')` y `stM('ctrl')` (y accesible desde el diagnóstico de SocBot) un distribuidor en tiempo real con 3 modos por subsistema (**Soporte Vital ECLSS**: *Ahorro 60 % / Nominal 100 % / Impulso 130 %*; **Calefacción y Escudos**: *Apagado / Nominal / Reforzado*; **Invernadero e ISRU**: *Pausado / Activo*), actualizando al instante las tasas de `rates()` y mostrando visualmente cómo ahorrar energía hoy reduce el oxígeno o el agua de mañana.
3. **[V9.3] "El Dilema de los 10 Segundos de los Paneles Solares" (Homenaje directo al video de Mike Given):**
   * Reemplazar el quiz de `crisis('drill')` por una emergencia interactiva en tiempo real de $12\text{ segundos}$ inspirada palabra por palabra en el ejemplo del autor del reto: una nube densa de eyección/polvo amenaza los paneles solares; el jugador debe decidir en segundos si **plegar y proteger los paneles** (pierde generación solar hoy, $-12\text{⚡}$, pero conserva el $100\text{ \%}$ de integridad) o **mantenerlos desplegados para no perder carga esta noche** (arriesgando $-18\text{🔧}$ de daño estructural y obstrucción severa).
4. **[V9.4] *Onboarding* sin fricción y Botón `"⚡ MODO JURADO / DEMO NASA (90 s)"` en el Menú Principal:**
   * Eliminar el bloqueo `if (!PROF.name)` en `newGame()` y `simStart()`: si el jugador no ha visitado *Credencial*, asignar automáticamente el perfil por defecto (`Cadete Nova`) y entrar directo a la acción en 1 clic.
   * Integrar el recordatorio de controles dentro del primer globo del tutorial sin abrir dos modales seguidos al inicio de la Misión 1.
   * Añadir en la pestaña `🚀 Jugar` del menú un botón destacado `"⚡ Modo Demo para Jurado (Simulación Viva DONKI + EVA + Trade-Offs)"` que coloque al evaluador en 1 segundo dentro de una base en plena operación con todos los sistemas clave desbloqueados, el rover listo y una alerta solar de DONKI en curso.
5. **[V9.5] Diagrama Visual de Interdependencia Sistémica y Selector Rápido de Clima DONKI en el HUD:**
   * Añadir en el diagnóstico de SocBot (`diag()`) y en `stM('ctrl')` un diagrama SVG vivo que muestre con flechas animadas el flujo $\text{☀️ Sol} \rightarrow \text{⚡ Batería} \rightarrow \text{🫁 Electrólisis O}_2 \text{ y 💧 Reciclaje 98 \%} \rightarrow \text{🌱 Cultivos y ❤️ Salud}$, coloreando en rojo cualquier eslabón que esté fallando.
   * Hacer que la insignia `.badge.dsrc` de la barra superior (`📡 DONKI` / `📚 Archivo NASA`) sea clicable para alternar en vivo entre el clima solar real sincronizado hoy y los grandes eventos históricos de la NASA (Mayo 2024 G5, Agosto 1972 Apolo, Halloween 2003).

---

### 3.2. ARTEMIS ACADEMY v10 (Post-Hackathon / Piloto Escolar)
1. **Proxy Backend Ligero para APIs NASA:** Microservicio sin estado (Cloudflare Worker) con API Key dedicada para evitar el límite de peticiones por hora (`RATE_LIMIT`) de `DEMO_KEY` cuando un colegio entero de 40 alumnos se conecta desde la misma IP.
2. **Exportación de Informe de Misión y Credencial en PDF/PNG:** Generación en un clic de un reporte descargable con el avatar del alumno, gráficas de cómo gestionó sus recursos, muestras analizadas y sello de rango alcanzado para entregar al profesor.
3. **Simplificación Visual de Problemas Matemáticos para 7–9 años:** Representar los retos numéricos del modo *Cadete* mediante bloques gráficos de baterías y tanques además de cifras escritas.

---

### 3.3. ARTEMIS ACADEMY v11+ (Visión a Largo Plazo)
1. **Modo Multijugador Cooperativo de Aula (*Crew Bridge Mode*):** 3 o 4 estudiantes conectados por WebRTC donde cada uno asume un rol en la misma base (Comandante, Ingeniería, Biología/Médico, Piloto EVA).
2. **Campaña Completa en la Superficie de Marte (Cráter Jezero):** Segundo mapa completo de $4\,800 \times 4\,800\text{ px}$ con gravedad $0.38\text{ g}$, tormentas globales de polvo de semanas de duración, planta **MOXIE** atmosférica y retraso continuo de señal de $3\text{ a }22\text{ min}$.
3. **Panel de Analítica Docente (*Artemis Classroom*):** Tablero anónimo por código de clase para que el profesor identifique qué conceptos STEM domina su grupo y cuáles requieren refuerzo.

---

## 4. SISTEMA DE PRIORIDADES PARA LA IMPLEMENTACIÓN DE v9

| ID | Mejora Propuesta para v9 | Prioridad | Complejidad | Justificación Estratégica |
| :--- | :--- | :---: | :---: | :--- |
| **V9.1** | Transformar la activación de tareas de misión (`runTask`) en **Decisiones Operativas de Trade-Offs + Minijuegos** en lugar de abrir un quiz A/B/C/D (`ask`) | **ALTA** | Baja | Elimina el único riesgo real de descalificación conceptual frente a la directriz *"Not a quiz, a living simulation"* del autor del reto. |
| **V9.2** | Consola interactiva de **Reparto de Potencia (*Power Allocation*)** acoplada a `rates()` | **ALTA** | Media-Baja | Materializa la mecánica central pedida en el reto: cada vatio asignado o retirado tiene consecuencias inmediatas sobre el oxígeno, el agua y la temperatura. |
| **V9.3** | Evento interactivo del **"Dilema de los 10 Segundos de los Paneles Solares"** en M1/M2 | **ALTA** | Baja | Conecta directamente con la escena de apertura del video oficial de Mike Given, demostrando que el juego fue diseñado a medida para este desafío. |
| **V9.4** | Eliminar bloqueo de `PROF.name` vacío + unificar modal inicial + botón **"⚡ Modo Demo Jurado"** | **ALTA** | Baja | Reduce el *Time-to-Wow* de más de 3 minutos a menos de 15 segundos, asegurando que ningún juez abandone antes de ver lo mejor del simulador. |
| **V9.5** | Diagrama SVG de flujo causal ($\text{⚡}\rightarrow\text{🫁}/\text{💧}\rightarrow\text{❤️}$) + selector rápido de tormentas DONKI en `#top` | **MEDIA** | Baja | Hace visible el pensamiento sistémico para los niños más pequeños (7–9 años) y pone los datos reales de la NASA a un solo clic en todo momento. |
| **V9.6** | Barras gráficas de ayuda visual en retos matemáticos del modo `cadete` | **BAJA** | Media | Útil para alumnos de 7–8 años, pero secundario frente a las mecánicas de simulación viva si el tiempo de implementación es limitado. |

---

## 5. EVALUACIÓN ESTRATÉGICA SPACE APPS (MATRIZ FODA OFICIAL)

### 5.1. Fortalezas Actuales (Lo que ya hace destacar a Artemis Academy)
* **Rigor científico y honestidad de datos insuperables:** Conexión real a APIs de NASA DONKI y OSDR con fallback curado transparente, sellos de autenticidad y verificador de fórmulas matemáticas dentro del juego.
* **Arquitectura técnica de élite en archivo único:** Motor 2.5D a 60 FPS, iluminación nocturna dinámica, sintetizador Web Audio API, TTS inteligente con *ducking* y 15 minijuegos sin una sola dependencia externa.
* **SocBot como innovación pedagógica:** En lugar de dar respuestas pasivas, audita las causas reales en la bitácora de la partida del alumno y aplica el método socrático.

### 5.2. Debilidades Actuales (Lo que v9 resolverá)
* Exceso de protagonismo del formato *quiz* (`ask(t)`) al iniciar tareas de misión en los primeros minutos.
* Fricción de entrada por obligar a rellenar la *Credencial* antes de permitir jugar la Misión 1.
* Falta de interruptores granulares de reparto de energía entre subsistemas en tiempo real.

### 5.3. Riesgos Críticos a Evitar durante la Programación de v9
* **Riesgo de regresión técnica:** Intentar reescribir funciones núcleo (`L`, `adv`, `ext`, `okp`, `draw`) en lugar de extenderlas limpiamente. **Mitigación:** En v9 todas las mejoras se integrarán de forma aditiva respetando las *Restricciones Críticas* de `PROJECT_CONTEXT.md`.
* **Riesgo de sobrecarga en la interfaz:** Añadir demasiados controles nuevos en el HUD principal. **Mitigación:** Mantener el HUD limpio en sus 3 zonas actuales y alojar el distribuidor de potencia y el diagrama causal dentro de los modales de Red Eléctrica (`pow`), Centro de Control (`ctrl`) y Diagnóstico de SocBot (`diag`).

### 5.4. Oportunidad Ganadora
Con la base técnica y visual ya consolidada en v8, implementar los **5 puntos de prioridad ALTA/MEDIA de v9** requiere tocar menos de 180 líneas de código sin riesgo para la estabilidad, pero transforma radicalmente la primera impresión del jurado: convierte a **Artemis Academy** en la encarnación exacta de la simulación viva que la NASA solicitó en el reto *Build a Junior Astronaut Mission Trainer*.