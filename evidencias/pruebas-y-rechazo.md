# 🧪 GATLOCK · Evidencia de pruebas y rechazo

Soporte público de cómo se probó GATLOCK, qué se rechazó y qué se corrigió en cada versión. Cada cifra indica su versión y su fecha. El código del agente es privado; aquí solo están los resultados.

[⬅️ Volver a la vitrina](../README.md) · [🌐 Probar el revisor](https://revisor-seguridad.cmarevalor1.workers.dev)

---

## 1. Cómo se prueba GATLOCK

| Prueba | Qué mide |
|---|---|
| **Catálogo de 13 casos de práctica** | Códigos en JavaScript/Node.js, React y Python con fallas sembradas (se sabe cuántas hay) y casos sanos (no tienen fallas). Incluye un caso "empresa" con 10 fallas. Se mide si encuentra lo esperado y si inventa falsas alarmas. |
| **Banco ciego** | Casos nuevos que no conoce quien corrige el agente, para medir la detección real. |
| **48 pruebas automáticas sin IA** | Comprueban las reglas fijas, los filtros contra falsas alarmas y el formato del informe antes de cada publicación. Resultado vigente: **48 de 48 bien** (27/11/2026). |
| **Pruebas y rechazo** (Jepher Díaz y Fabián Pulido) | Registro de corridas con metas de detección, falsas alarmas y tiempo, y revisión del proyecto frente a los estándares OWASP. Si no se cumple la meta, la versión se rechaza y se corrige. |

En cada revisión la página da un veredicto: **PASÓ** (encontró lo esperado y nada de más), **NO PASÓ** o **INCOMPLETA** (un especialista de IA no respondió y no se puede juzgar).

---

## 2. Historial de pruebas: qué se rechazó y qué se corrigió

| Versión y fecha | Resultado | Decisión y corrección |
|---|---|---|
| v2-maestra · 21/11 | Caso Flask de 4 fallas: 3 de 4 (descartaba el XSS) | ❌ Rechazada → v2.1: el filtro acepta citas de hasta 5 líneas |
| v2.1 · 22/11 | Caso Flask: **PASÓ** (4 de 4) | ✅ |
| v2.2 a v2.4 · 22/11 | Caso empresa: 4, 6 y 7 de 10 fallas | ❌ Rechazadas → reglas nuevas, filtro que une textos de varias líneas, reintentos y respuesta sin razonamiento largo; en la v2.5, dos especialistas de IA en paralelo |
| v2.5 · 22/11 | Caso empresa: **PASÓ 10 de 10**, pero el caso 7 (sano) dio una falsa alarma (IDOR inventado) | ❌ Rechazada → v2.6: especialista B más estricto |
| v2.6 · 23/11 | Caso empresa 10 de 10; caso 7 (sano): inyección SQL inventada | ❌ Rechazada → v2.7: filtro de consultas con parámetros |
| v2.7 · 24/11 | Caso 7 (sano): IDOR en una ruta sin sesión | ❌ Rechazada → v2.8 |
| v2.8 · 24/11 | Catálogo: 9 de 12 PASÓ; falsas alarmas en los sanos 8 y 10 | ❌ Rechazada → v2.9 y v2.10: filtros nuevos |
| v2.10 · 24/11 | Catálogo: 12 de 13 PASÓ (falló el 8)\*; empresa 9 de 10 reales; mediana 31,1 s | 🟡 Mejor; falta el caso 8 |
| v2.11 · 24/11 | Casos 4, 6 y 8: **3 de 3 PASÓ**; 3 de 4 parches verificados | ✅ |
| v2.12 · 24/11 | Banco ciego (24 corridas): ver punto 3 | ❌ Metas no cumplidas → v2.13 |
| v2.13 · 25/11 | 48 de 48 pruebas automáticas | ✅ Publicada; falta medir con la IA real |
| v2.13.1 · 27/11 | Primera medición con la IA real: ver punto 5 | ✅ Vigente |

\* Cifra en revisión: esta misma ronda aparece una vez como 12 de 13 y otra como 11 de 12.

**Comparación (24/11/2026, caso empresa):** Semgrep 1.186.0 con reglas comunitarias de Python y Flask encontró 4 de 10; Gitleaks 8.24.3 encontró 0 de 1; GATLOCK v2.10 encontró 9 de 10.

---

## 3. Banco ciego de la v2.12: el rechazo que dio origen a la v2.13

Registro de pruebas y rechazo de Jepher Díaz y Fabián Pulido (25/11/2026), **24 corridas**:

| Indicador | Resultado v2.12 | Meta | Estado |
|---|---|---|---|
| Detección (recall) | **58,3 %** (7 de 12) | 85 % | ❌ No cumple |
| Falsas alarmas en código sano | **2** en 12 corridas sanas (83,3 % limpios) | 0 | ❌ No cumple |
| Tiempo medio por revisión | **20,5 s** (mediana 13,2 s; máximo 84,9 s) | 15 s | ❌ No cumple |

**Decisión:** versión rechazada. **Corrección (v2.13, 25/11/2026):**
- Motor de **reglas fijas sin IA** (como Semgrep) para las fallas clásicas: inyección SQL y de comandos, XSS, eval, rutas de archivo, rutas de administración sin rol, MD5, modo de pruebas encendido.
- **Filtros nuevos contra falsas alarmas:** no reporta datos que no vienen del usuario ni "claves expuestas" sin valor escrito.
- **Límite de tiempo** por intento y por especialista, para acercarse a la meta de 15 s.
- Ficha nueva RV-COD-18 (asignación masiva) y aviso de revisión **incompleta** si falta un especialista.

> Los casos de ese banco ciego se usaron para corregir la v2.13, así que ya no son ciegos. La detección real se debe medir con casos nuevos.

---

## 4. Revisión frente a estándares OWASP

En el mismo trabajo de pruebas y rechazo, Jepher Díaz y Fabián Pulido revisaron el proyecto frente a **OWASP Top 10 2025, OWASP GenAI 2026, OWASP API 2023 y ASVS**, con una hoja de validación y riesgos. Recomendaciones principales y qué se hizo:

| Recomendación | Estado |
|---|---|
| Límite de uso (rate limiting) para que nadie agote el servicio | ✅ Hecho en v2.13.1: 5 revisiones por minuto por persona, con contador exacto |
| Protección contra uso automatizado (bots) | ✅ Hecho en v2.13.1: verificación "no soy un robot" (Cloudflare Turnstile) |
| Bajar las falsas alarmas y exigir evidencia | ✅ Filtro de evidencia literal y reglas fijas (v2.7 a v2.13) |
| Medir con un banco de pruebas y no afirmar sin evidencia | 🟡 En curso: primera medición de la v2.13.1 (punto 5); falta un banco ciego nuevo |
| Modelo de amenazas del propio servicio | ⏳ Pendiente |
| Pruebas de inyección de instrucciones a la IA (red team) | ⏳ Pendiente |
| Prueba con usuarios (utilidad de las correcciones) | ⏳ Pendiente |

---

## 5. Primera medición de la v2.13.1 con la IA real (27/11/2026)

11 revisiones de casos de práctica en la página publicada (agente v2.13.1):

| Caso | Qué tiene | Esperado | Encontrado | Veredicto | Tiempo |
|---|---|:---:|:---:|:---:|---:|
| 2 | Inyección SQL (JavaScript) | 1 | 1 | ✅ PASÓ | 15,1 s |
| 3 | XSS con innerHTML (JavaScript) | 1 | 1 | ⏳ INCOMPLETA\*\* | 85,2 s |
| 5 | Inyección de comandos (JavaScript) | 1 | 1 | ✅ PASÓ | 14,7 s |
| 6 | Librerías con fallas (package.json) | 3 | 3 | ⏳ INCOMPLETA\*\* | 51,1 s |
| 7 | Código sano (Node.js) | 0 | 0 | ✅ PASÓ | 41,9 s |
| 7 | Código sano (Node.js), repetido | 0 | 0 | ✅ PASÓ | 10,2 s |
| 8 | Código sano con variable de entorno | 0 | 0 | ✅ PASÓ | 24,4 s |
| 9 | Recorrido de rutas (JavaScript) | 1 | 1 | ✅ PASÓ | 49,9 s |
| 10 | Código sano con basename | 0 | 0 | ✅ PASÓ | 50,5 s |
| 11 | Inyección SQL (Python/Flask) | 1 | 2 | ❌ NO PASÓ (1 hallazgo de más) | 27,8 s |
| 12 | XSS en React | 1 | 1 | ✅ PASÓ | 47,3 s |

**Resumen:**
- Encontró la falla esperada en **7 de 7** casos con falla.
- **0 falsas alarmas** en 4 revisiones de código sano.
- **1 hallazgo de más** en el caso 11: "credenciales en el código" citando una línea sin ninguna clave. Queda identificado para corregir.
- \*\* **2 revisiones incompletas** (casos 3 y 6): un especialista de IA no respondió, aunque las dos encontraron lo esperado. Hubo además 1 intento que no terminó por un error del servicio de IA (tardó más de 30 s).
- **Tiempos de 10 a 85 s** por revisión. Incluyen la verificación "no soy un robot"; falta medir el tiempo del agente por separado.

> Es una muestra pequeña y no es un banco ciego: sirve como evidencia de funcionamiento, no como medida final de detección.

---

## 6. Lo que falta

- Un **banco ciego nuevo** con la v2.13.1 (casos que no haya visto quien corrige el agente).
- Los **13 casos del catálogo**, 10 corridas sanas y 6 parches verificados con la versión vigente.
- Corregir el hallazgo de más del caso 11 y que una revisión de solo librerías no salga "incompleta" cuando falla un especialista.
- **Prueba con usuarios**, modelo de amenazas y pruebas de red team.

Ninguna herramienta garantiza seguridad total: que GATLOCK no encuentre fallas no prueba que el código sea seguro, y una persona revisa cada corrección antes de aplicarla.

---

<div align="center">

**GATLOCK · Sector ITO · Ciberseguridad para el código generado con IA**

</div>
