# Notas de clase — Taller 4: Mapa de Infraestructura y Diagnóstico Técnico

## Fecha



## Asistentes



## Componentes identificados (Paso 1)

- App Móvil - Usuario Final
- App Móvil - Mensajero
- Portal Web - Operador
- Balanceador de Carga
- API Gateway Bogotá
- API Gateway Medellín
- Módulo de Procesamiento de Rutas y Paquetes (Bogotá)
- Base de Datos Distribuida
- Servicio de Monitoreo y Alertas

## Repaso del caso base (RedExpress)

- **Agrupación por zona:** los componentes se organizan en 4 zonas: Clientes, Borde/Global (lo que atiende a todas las regiones por igual), Región Bogotá y Región Medellín. Esta agrupación es la que permite ver de un vistazo que Medellín depende de Bogotá para procesar rutas.
- **Tráfico trazado:** los 3 clientes entran por el Balanceador de Carga, que enruta a cada API Gateway regional; ambos gateways alimentan el único módulo de procesamiento de rutas (en Bogotá) y reportan al servicio de monitoreo.
- **Redundancia marcada:** el Balanceador de Carga y la Base de Datos Distribuida quedan como instancia única (sin redundancia); el API Gateway de Medellín queda marcado como dependiente de Bogotá para el procesamiento de rutas.
- **Diagnóstico priorizado (tabla de la guía):**

| Componente | Riesgo diagnosticado | Categoría | Prioridad |
|---|---|---|---|
| Balanceador de Carga (instancia única) | Punto único de falla | Disponibilidad | Alta |
| Base de Datos Distribuida (escritura única en Bogotá) | Cuello de botella de latencia | Rendimiento | Alta |
| Región Medellín sin módulo de rutas propio | Límite de escalabilidad geográfica | Escalabilidad | Media |

- **Por qué "cuello de botella" y "punto único de falla" no son lo mismo:** un punto único de falla tumba toda la plataforma si falla (disponibilidad); un cuello de botella no la tumba, pero la vuelve lenta bajo carga (rendimiento) — por eso el Balanceador y la Base de Datos están en categorías distintas aunque ambos sean "instancia única".

## Checklist de autoevaluación

- [x] Todos los componentes de infraestructura relevantes están representados.
- [x] Los componentes están agrupados por zona/región o capa.
- [x] Cada conexión relevante entre componentes está trazada.
- [x] Los componentes críticos indican si tienen redundancia o son instancia única.
- [x] Cada riesgo diagnosticado está clasificado (disponibilidad, rendimiento o escalabilidad) y priorizado.
- [x] El diagnóstico hace referencia explícita a los componentes del mapa, no a afirmaciones genéricas.

## Retroalimentación del docente



## Pendientes para la Parte 2 (cliente real)

- [ ] Levantar con el equipo los componentes reales de infraestructura del Contact Center (¿dónde vive el Excel? ¿hay algún servidor/servicio propio o todo corre sobre Microsoft 365 y Anexo Unisabana?)
- [ ] Clasificar los riesgos reales encontrados (disponibilidad, rendimiento, escalabilidad) igual que en la tabla de diagnóstico
- [ ] Investigar buenas prácticas de infraestructura cloud/on-premise/híbrida aplicables al caso, para `entrega/referencias.md`
