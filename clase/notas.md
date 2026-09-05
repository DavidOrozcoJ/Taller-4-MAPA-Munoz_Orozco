# Notas de Clase — Taller 4: Mapa de Infraestructura y Diagnóstico Técnico

**Equipo:** _(completar con nombres del equipo)_
**Fecha:** _(completar)_
**Caso trabajado:** RedExpress (caso base)

---

## Paso 1 — Identificar componentes

Componentes de infraestructura identificados a partir del caso base:

- Cliente App Móvil (mensajeros)
- Cliente Plataforma Web (operadores/usuarios)
- Balanceador de Carga
- API Gateway Bogotá
- API Gateway Medellín
- Módulo de Procesamiento de Rutas y Estados de Paquetes
- Base de Datos Distribuida (escritura centralizada en Bogotá)
- Servicio de Monitoreo y Alertas

---

## Paso 2 — Agrupar por zona/capa

| Zona | Componentes |
|---|---|
| **Clientes** | App Móvil, Plataforma Web |
| **Borde / Global** | Balanceador de Carga, Servicio de Monitoreo y Alertas |
| **Región Bogotá** | API Gateway Bogotá, Módulo de Procesamiento de Rutas, Base de Datos Distribuida |
| **Región Medellín** | API Gateway Medellín (sin módulo de procesamiento propio) |

---

## Paso 3 — Conectar los componentes

- Clientes → Balanceador de Carga
- Balanceador de Carga → API Gateway Bogotá
- Balanceador de Carga → API Gateway Medellín
- API Gateway Bogotá → Módulo de Procesamiento de Rutas (local)
- API Gateway Medellín → Módulo de Procesamiento de Rutas **de Bogotá** (dependencia entre regiones)
- Módulo de Procesamiento de Rutas → Base de Datos Distribuida
- Región Bogotá → Servicio de Monitoreo y Alertas
- Región Medellín → Servicio de Monitoreo y Alertas

*(Ver mapa gráfico completo en `mapa-borrador.drawio`)*

---

## Paso 4 — Marcar redundancia y capacidad

| Componente | ¿Redundancia? | Observación |
|---|---|---|
| Balanceador de Carga | ❌ Instancia única | Punto único de falla |
| Base de Datos Distribuida | ❌ Escritura única (Bogotá) | Cuello de botella de latencia |
| API Gateway Medellín | ⚠️ Sin módulo propio | Depende de Bogotá para procesar rutas |
| API Gateway Bogotá | ✅ Redundante | — |
| Servicio de Monitoreo y Alertas | ✅ Redundante | — |

---

## Paso 5 — Diagnóstico y priorización

| Componente | Riesgo diagnosticado | Categoría | Impacto si ocurre | Prioridad |
|---|---|---|---|---|
| Balanceador de Carga (instancia única) | Punto único de falla | Disponibilidad | Toda la plataforma queda inaccesible | **Alta** |
| Base de Datos Distribuida (escritura única en Bogotá) | Cuello de botella de latencia | Rendimiento | Lentitud en rastreo en tiempo real fuera de Bogotá | **Alta** |
| Región Medellín sin módulo de rutas propio | Límite de escalabilidad geográfica | Escalabilidad | No se puede escalar demanda en Medellín sin saturar Bogotá | **Media** |

---

## Checklist de autoevaluación

- [x] Todos los componentes de infraestructura relevantes están representados.
- [x] Los componentes están agrupados por zona/región o capa.
- [x] Cada conexión relevante entre componentes está trazada.
- [x] Los componentes críticos indican si tienen redundancia o son instancia única.
- [x] Cada riesgo diagnosticado está clasificado (disponibilidad, rendimiento o escalabilidad) y priorizado.
- [x] El diagnóstico hace referencia explícita a los componentes del mapa, no a afirmaciones genéricas.

---

## Retroalimentación del docente

_(Registrar aquí los comentarios y ajustes sugeridos por el docente durante la clase)_

- 
- 
- 

## Ajustes pendientes tras la retroalimentación

- 
- 
