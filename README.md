<div align="center">

# 🐱‍👤 GATLOCK
### El revisor de seguridad con IA para el código que hoy escribe la IA

**Encuentra la falla. La explica. Te entrega el arreglo. En segundos.**

`Agente v2.13.1` · `Página v30.2` · `Demo Day · 3 de diciembre de 2026`

[🌐 Ver GATLOCK en acción](https://gatlock-seguridad.ai.studio/) · [💬 Hablar con GATLOCK](https://agent.jotform.com/01a1ff72a1a870008745db19e4838d0fa34a) · [📚 Biblioteca de vulnerabilidades](https://revisor-seguridad.cmarevalor1.workers.dev/biblioteca.html)

<a href="https://github.com/cmarevalor1-oss/Gatlock/raw/main/evidencias/GATLOCK_pruebas_y_rechazo.xlsx"><img src="https://img.shields.io/badge/%F0%9F%93%A5%20Descargar-Evidencia%20de%20pruebas%20(Excel)-1D6F42?style=for-the-badge" alt="Descargar evidencia de pruebas en Excel"></a>

</div>

---

## El problema

La IA escribe código cada vez más rápido. Pero rápido no es lo mismo que seguro.
Una contraseña olvidada dentro del código, un formulario que deja leer toda la base de datos o una librería vieja con fallas conocidas bastan para abrir la puerta a un robo de datos. Y casi nadie revisa ese código antes de publicarlo.

## La solución: GATLOCK

GATLOCK es un agente de IA que revisa el código **sin ejecutarlo** y, por cada problema que encuentra, te dice:

| | |
|---|---|
| 🔎 **Dónde está** | La línea y el fragmento exacto que causa la falla |
| ⚠️ **Qué tan grave es** | Crítica, Alta o Media, y si está Confirmada o es Condicional |
| 🧠 **Por qué importa** | El riesgo explicado en palabras claras, con su ficha CWE y OWASP Top 10:2025 |
| 🛠️ **Cómo arreglarlo** | El código corregido, listo para revisar y aplicar |
| ✅ **Si el arreglo funcionó** | Con el botón **Verificar parche** |

Revisa **JavaScript / Node.js, HTML / React y Python**, además de archivos de servidor (nginx, Apache, Dockerfile, .env). Y también revisa **páginas web publicadas**.

---

## Resultados medidos, sin maquillar

<div align="center">

| 🏆 **9 de 10** | ✅ **12 de 13** | 🎯 **3 de 3** | ⚡ **~31 s** |
|:---:|:---:|:---:|:---:|
| fallas sembradas encontradas en una app bancaria de prueba · v2.10, 24/11/2026 | casos de práctica aprobados · v2.10, 24/11/2026 * | casos superados en la última ronda · v2.11, 24/11/2026 | mediana por revisión · v2.10, 24/11/2026 |

</div>

> **En el mismo caso bancario, Semgrep 1.186.0, una herramienta profesional de referencia, encontró 4 de 10 (24/11/2026, reglas comunitarias de Python y Flask). GATLOCK v2.10 encontró 9.**
>
> \* Cifra en revisión: en el registro de cambios, esta misma ronda aparece una vez como 12 de 13 y otra como 11 de 12 (falló el caso 8).

**Primera medición de la versión 2.13.1 con la IA real (27/11/2026, 11 revisiones de casos de práctica):**

| 🎯 **7 de 7** | ✅ **0** | ⚠️ **1** | ⏱️ **10 a 85 s** |
|:---:|:---:|:---:|:---:|
| casos con falla en los que la encontró | falsas alarmas en 4 revisiones de código sano | hallazgo de más (caso 11, Flask), ya identificado | por revisión; incluyen la verificación "no soy un robot" (pendiente medir el tiempo del agente por separado) |

> Es una muestra pequeña y no es un banco ciego. 2 de las 11 revisiones salieron incompletas: un especialista de IA no respondió (casos 3 y 6), aunque las dos encontraron lo esperado. Hubo además 1 intento que no terminó por un error del servicio de IA (tardó más de 30 s).
>
> 🧪 **[Ver la evidencia completa de pruebas y rechazo](evidencias/pruebas-y-rechazo.md)** · 📥 **[Descargar en Excel](https://github.com/cmarevalor1-oss/Gatlock/raw/main/evidencias/GATLOCK_pruebas_y_rechazo.xlsx)**: cómo se prueba, qué se rechazó en cada versión, el banco ciego de la v2.12, la revisión frente a OWASP y el detalle de estas 11 revisiones.

### Casos de éxito

**1. App bancaria con 10 fallas escondidas** · 🔴 Crítica
Encontró 9 (v2.10, 24/11/2026): datos manipulables para ejecutar órdenes, transferencias y reportes que dejaban leer la base de datos, contraseñas guardadas con un método débil, documentos de otros usuarios a la vista, una clave por defecto, el modo de pruebas encendido, registros con datos sensibles y subida de archivos sin control. Además reconoció 2 protecciones que el código sí tenía bien hechas.

**2. Contraseña escrita dentro del código** · 🔴 Crítica
Detectada (v2.10). Arreglo propuesto: guardar la clave aparte, en una variable de entorno.

**3. Formulario que dejaba leer toda la base de datos (inyección SQL)** · 🔴 Crítica
Detectado en JavaScript y también en Python (v2.10). Arreglo: consultas seguras con parámetros.

**4. Página que dejaba meter código malicioso (XSS)** · 🟠 Alta
Detectada en JavaScript y en React. Arreglo: mostrar el texto del usuario como texto, sin ejecutarlo.

**5. Lista de usuarios visible para cualquiera** · 🟠 Alta
Una función entregaba los datos de todos sin comprobar si quien pedía era administrador (v2.10).

**6. Comandos peligrosos y archivos fuera de lugar** · 🟠 Alta
Un atacante podía ejecutar órdenes en el servidor o abrir archivos privados cambiando la ruta (ambos detectados con la v2.10).

**7. Librerías viejas con fallas conocidas** · 🟠 Alta / 🟡 Media
Consultó OSV.dev, la base de datos de Google, y encontró 12 fallas conocidas en 3 librerías (lodash, jsonwebtoken y express), con la versión exacta a la que hay que actualizar.

**8. Código sano, sin falsas alarmas** · ✅
En 3 de 4 revisiones de código sano no dio ninguna falsa alarma e incluso señaló lo que estaba bien hecho. La alarma de más del cuarto caso se corrigió en versiones posteriores.

---

## Novedades de la versión 2.13.1 (27/11/2026)

- 🛡️ **Protección contra abuso.** Cada persona puede hacer hasta 5 revisiones por minuto, con un contador exacto. Así nadie puede agotar la IA del servicio.
- 🤖 **Verificación "no soy un robot"** (Cloudflare Turnstile). Casi siempre es invisible; si hace falta, aparece una casilla abajo a la derecha.
- 👥 **"¿Quién hace la prueba?"** Se escoge el nombre de la lista del equipo y queda en cada informe.

## Novedades de la versión 2.13

- ⚙️ **Reglas fijas, sin IA, como Semgrep.** Buscan al instante patrones conocidos (inyección SQL y de comandos, XSS, eval, rutas de archivo, rutas de administración sin rol, contraseñas con MD5, modo de pruebas encendido y más). Si la IA encuentra lo mismo, la falla queda confirmada por las dos.
- 🧹 **Menos falsas alarmas.** Filtros nuevos: no reporta datos que no vienen del usuario, ni "claves expuestas" que en realidad no tienen ningún valor escrito, ni temas fuera de alcance.
- ⏱️ **Más rápido.** Límite de tiempo por revisión y el segundo especialista solo entra cuando el código tiene sesiones, archivos, dinero o administración.
- 📚 **Ficha nueva:** asignación masiva (RV-COD-18). La biblioteca llega a 37 vulnerabilidades.

### Lo que ya traía

- 🤝 **Dos especialistas de IA trabajando a la vez.** Uno caza inyecciones, claves y configuración; el otro, fallas de control de acceso, operaciones con dinero, archivos y rutas de administración.
- 🧹 **Filtro contra falsas alarmas.** Solo reporta lo que tiene evidencia real en el código.
- 🔑 **Detector de claves y contraseñas expuestas.**
- 📦 **OSV.dev en vivo con doble comprobación.** Dice qué librerías tienen fallas conocidas y a qué versión actualizar, y antes de recomendarla la vuelve a verificar (hasta 3 veces).
- ✅ **Botón "Verificar parche".** Comprueba si la corrección realmente cerró la falla.
- 🌐 **Escáner pasivo de páginas web.** Revisa HTTPS, cabeceras de seguridad, cookies, CORS y librerías, sin atacar el sitio.
- 📚 **Biblioteca de 37 vulnerabilidades**, cada una con su ficha CWE, OWASP Top 10:2025, corrección y forma de comprobarla.

### Inspirado en herramientas de referencia de la industria

| Herramienta de referencia | Cómo lo cubre GATLOCK |
|---|---|
| Semgrep | Reglas fijas propias + dos especialistas de IA |
| Gitleaks | Detector de claves propio |
| OSV-Scanner | Integrado (OSV.dev en vivo) |
| Trivy | OSV.dev + revisión de archivos de servidor |
| OWASP ZAP (modo pasivo) | Escáner pasivo de páginas web: cabeceras, cookies, CORS, formularios y errores |
| Nuclei | Cobertura parcial: reconoce librerías JavaScript con fallas conocidas (CVE) sin enviar ataques; no ejecuta las plantillas de Nuclei |
| Greenbone | Fuera de alcance: escanea redes y dispositivos y debe instalarse dentro de la red |

---

## El ecosistema GATLOCK

| | |
|---|---|
| 🌐 **Web GATLOCK** | La presentación del proyecto: [gatlock-seguridad.ai.studio](https://gatlock-seguridad.ai.studio/) |
| 🧠 **Revisor de código** | El agente que analiza el código y las páginas web |
| 💬 **GATLOCK, el asistente** | Conversa contigo sobre seguridad en código hecho con IA: [abrir chat](https://agent.jotform.com/01a1ff72a1a870008745db19e4838d0fa34a) |
| 📚 **Biblioteca** | [37 fichas de vulnerabilidades](https://revisor-seguridad.cmarevalor1.workers.dev/biblioteca.html) |
| 🧰 **Reporte de herramientas** | [Qué herramientas, conectores y APIs usa](https://revisor-seguridad.cmarevalor1.workers.dev/herramientas.html) |

---

## Cómo se hizo el video

El video de presentación de GATLOCK se armó con estas herramientas:

| Herramienta | Para qué se usó |
|---|---|
| 🎙️ **ElevenLabs** | La voz en off del gato. La versión final usa la voz **Jaider** (en una primera versión se usó ttsmaker) |
| 🎬 **Kling** | La animación del gato entrando al túnel de código |
| 🤖 **Claude Code** | La edición en el computador: unir las escenas con la voz, arreglar el inicio en negro y sacar una versión compatible con cualquier reproductor |
| 🌐 **Google AI Studio** | Las tomas de la [web GATLOCK](https://gatlock-seguridad.ai.studio/) que aparecen en el video |
| ▶️ **VLC** | La reproducción y revisión del video |

---

## Transparencia

- GATLOCK hace análisis estático: no ejecuta el código ni lanza ataques.
- Los resultados vienen de pruebas internas del prototipo (versiones 2.10 a 2.13.1, noviembre de 2026); cada cifra indica su versión y su fecha. La versión 2.13.1 tiene una primera medición pequeña (11 revisiones, 27/11/2026); falta un banco ciego nuevo.
- Ninguna herramienta garantiza seguridad total. Una persona revisa cada corrección antes de aplicarla.

---

## ¿Quieres GATLOCK para tu equipo o empresa?

El código fuente es privado. GATLOCK está disponible para **licencias, pilotos con empresas y alianzas**.

💼 **Contacto comercial:** [LinkedIn](https://www.linkedin.com/in/cmar2026)

---

## El equipo

GATLOCK (Revisor IA de Seguridad de Código) es un proyecto del grupo del Reto 6, Sector ITO.

**Líderes del proyecto**
- **Carlos Ibáñez** (profesor): nos dio las pautas para culminar con éxito el proyecto.
- **Andrés Tarquino**: entregó la *Instrucción Maestra de Operación* y elaboró el objetivo general y los objetivos específicos.

| Rol | Integrantes |
|---|---|
| Repositorio en GitHub (subir el código y verificar que corra) | Camilo Andrés Miranda Fuentes y Claudia Margarita Arévalo Ruiz |
| Auditoría de repositorios GitHub | Camilo Andrés Miranda Fuentes |
| Documentación | Jorge Enrique Ladrón de Guevara López y Miguel Caicedo |
| Integración y desarrollo | Edwin Javier Largo y Claudia Margarita Arévalo Ruiz |
| Pruebas y rechazo | Jepher Díaz y Fabián Pulido |
| Revisión final | Todo el equipo: Carlos Fabián Vargas Medina, Jaime Granados, Javier Alfonso Morán Rodríguez, Jorge Andrés Sánchez, José William Medina Rodríguez, Mauricio Rodríguez Barrero, Orlando Chaparro Herrera y Yesenia Ballestas, junto con los demás integrantes |
| Observador | Johan Alfonso |

**Aportes destacados**
- **Camilo Andrés Miranda Fuentes**: desarrolló la auditoría de repositorios GitHub. El agente revisa un repositorio completo a partir de su dirección y rama, conectado a un segundo servicio de análisis en Python (versiones v2.14 a v2.16, en desarrollo).
- **Edwin Javier Largo**: el prompt que dio origen a la biblioteca de vulnerabilidades basada en las herramientas del agente, y a la revisión de servidores y páginas web, no solo de código.
- **Jepher Díaz y Fabián Pulido**: la hoja de pruebas y rechazo (banco ciego) que midió los resultados reales del agente.

<div align="center">

**GATLOCK · Sector ITO · Ciberseguridad para el código generado con IA**

© 2026 · Todos los derechos reservados

</div>
