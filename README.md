<div align="center">

# 🐱‍👤 GATLOCK
### El revisor de seguridad con IA para el código que hoy escribe la IA

**Encuentra la falla. La explica. Te entrega el arreglo. En segundos.**

`Agente v2.12` · `Página v28` · `Noviembre 2026`

[🌐 Ver GATLOCK en acción](https://gatlock-seguridad.ai.studio/) · [💬 Hablar con GATLOCK](https://agent.jotform.com/01a1ff72a1a870008745db19e4838d0fa34a) · [📚 Biblioteca de vulnerabilidades](https://revisor-seguridad.cmarevalor1.workers.dev/biblioteca.html)

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
| fallas reales encontradas en una app bancaria de prueba, en 64 segundos | casos de práctica aprobados | casos superados en la última ronda | tiempo típico por revisión |

</div>

> **En el mismo caso bancario, Semgrep 1.186.0, una herramienta profesional de referencia, encontró 4 de 10. GATLOCK encontró 9.**

### Casos de éxito

**1. App bancaria con 10 fallas escondidas** · 🔴 Crítica
Encontró 9 en 64 segundos: datos manipulables para ejecutar órdenes, transferencias y reportes que dejaban leer la base de datos, contraseñas guardadas con un método débil, documentos de otros usuarios a la vista, una clave por defecto, el modo de pruebas encendido, registros con datos sensibles y subida de archivos sin control. Además reconoció 2 protecciones que el código sí tenía bien hechas.

**2. Contraseña escrita dentro del código** · 🔴 Crítica
Detectada en 27 segundos. Arreglo propuesto: guardar la clave aparte, en una variable de entorno.

**3. Formulario que dejaba leer toda la base de datos (inyección SQL)** · 🔴 Crítica
Detectado en JavaScript en 15 segundos, y también en Python. Arreglo: consultas seguras con parámetros.

**4. Página que dejaba meter código malicioso (XSS)** · 🟠 Alta
Detectada en JavaScript y en React. Arreglo: mostrar el texto del usuario como texto, sin ejecutarlo.

**5. Lista de usuarios visible para cualquiera** · 🟠 Alta
Una función entregaba los datos de todos sin comprobar si quien pedía era administrador. Encontrada en 14 segundos.

**6. Comandos peligrosos y archivos fuera de lugar** · 🟠 Alta
Un atacante podía ejecutar órdenes en el servidor (detectado en 13 s) o abrir archivos privados cambiando la ruta (11 s).

**7. Librerías viejas con fallas conocidas** · 🟠 Alta / 🟡 Media
Consultó OSV.dev, la base de datos de Google, y encontró 12 fallas conocidas en 3 librerías (lodash, jsonwebtoken y express), con la versión exacta a la que hay que actualizar.

**8. Código sano, sin falsas alarmas** · ✅
En 3 de 4 revisiones de código sano no dio ninguna falsa alarma e incluso señaló lo que estaba bien hecho. La alarma de más del cuarto caso se corrigió en versiones posteriores.

---

## Novedades de la versión 2.12

- 🤝 **Dos especialistas de IA trabajando a la vez.** Uno caza inyecciones, claves y configuración; el otro, fallas de control de acceso, operaciones con dinero, archivos y rutas de administración.
- 🧹 **Filtro contra falsas alarmas.** Solo reporta lo que tiene evidencia real en el código.
- 🔑 **Detector de claves y contraseñas expuestas.**
- 📦 **OSV.dev en vivo con doble comprobación.** Dice qué librerías tienen fallas conocidas y a qué versión actualizar, y antes de recomendarla la vuelve a verificar (hasta 3 veces).
- ✅ **Botón "Verificar parche".** Comprueba si la corrección realmente cerró la falla.
- 🌐 **Escáner pasivo de páginas web.** Revisa HTTPS, cabeceras de seguridad, cookies, CORS y librerías, sin atacar el sitio.
- 📚 **Biblioteca de 36 vulnerabilidades**, cada una con su ficha CWE, OWASP Top 10:2025, corrección y forma de comprobarla.

### Inspirado en herramientas de referencia de la industria

| Herramienta de referencia | Cómo lo cubre GATLOCK |
|---|---|
| Semgrep | Equivalente propio con dos especialistas de IA |
| Gitleaks | Detector de claves propio |
| OSV-Scanner | Integrado (OSV.dev en vivo) |
| Trivy | OSV.dev + revisión de archivos de servidor |
| OWASP ZAP (modo pasivo) / Nuclei | Escáner pasivo de páginas web (cobertura parcial) |

---

## El ecosistema GATLOCK

| | |
|---|---|
| 🌐 **Web GATLOCK** | La presentación del proyecto: [gatlock-seguridad.ai.studio](https://gatlock-seguridad.ai.studio/) |
| 🧠 **Revisor de código** | El agente que analiza el código y las páginas web |
| 💬 **GATLOCK, el asistente** | Conversa contigo sobre seguridad en código hecho con IA: [abrir chat](https://agent.jotform.com/01a1ff72a1a870008745db19e4838d0fa34a) |
| 📚 **Biblioteca** | [36 fichas de vulnerabilidades](https://revisor-seguridad.cmarevalor1.workers.dev/biblioteca.html) |
| 🧰 **Reporte de herramientas** | [Qué herramientas, conectores y APIs usa](https://revisor-seguridad.cmarevalor1.workers.dev/herramientas.html) |

---

## Transparencia

- GATLOCK hace análisis estático: no ejecuta el código ni lanza ataques.
- Los resultados vienen de pruebas internas del prototipo (versiones 2.10 y 2.11, noviembre de 2026). La versión 2.12 se está midiendo.
- Ninguna herramienta garantiza seguridad total. Una persona revisa cada corrección antes de aplicarla.

---

## ¿Quieres GATLOCK para tu equipo o empresa?

El código fuente es privado. GATLOCK está disponible para **licencias, pilotos con empresas y alianzas**.

💼 **Contacto comercial:** [LinkedIn](https://www.linkedin.com/in/cmar2026)

---

## El equipo

GATLOCK es un proyecto creado y presentado por:

**Líderes del proyecto**
- Carlos Ibáñez
- Andrés Tarquino

**Implementadores**
- Claudia Margarita Arévalo Ruiz
- Camilo Miranda
- Carlos Fabián Vargas Medina
- Fabián Pulido
- Jaime Granados
- Javier
- Javier Alfonso Morán Rodríguez
- Jepher Díaz
- Jorge Andrés Sánchez
- Jorge Enrique Ladrón de Guevara López
- José William Medina Rodríguez
- Mauricio Rodríguez Barrero
- Miguel Caicedo
- Orlando Chaparro Herrera
- Yesenia Ballestas

<div align="center">

**GATLOCK · Sector ITO · Ciberseguridad para el código generado con IA**

© 2026 · Todos los derechos reservados

</div>
