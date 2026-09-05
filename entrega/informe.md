# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 4 - Mapa de Infraestructura y Diagnóstico Técnico_

## 👥 Integrantes del equipo
- Nombre 1 (correo o usuario GitHub)
- Nombre 2

## 🧠 Descripción general del trabajo
Este entregable corresponde a la **Parte 1 (Trabajo en Clase)** del taller: la construcción del mapa de infraestructura y el diagnóstico técnico priorizado sobre el caso base **RedExpress**, siguiendo la metodología de 5 pasos (mapa + diagnóstico) descrita en la guía paso a paso. El objetivo fue practicar la identificación, agrupación y evaluación de riesgos de infraestructura antes de aplicar la misma metodología al sistema real del cliente en la Parte 2.

## 🔧 Proceso de desarrollo
Se siguieron los 5 pasos de la guía:

1. **Identificar componentes:** se listaron los tres puntos de entrada de cliente (App Móvil Usuario Final, App Móvil Mensajero, Portal Web Operador), el Balanceador de Carga, los API Gateway regionales (Bogotá y Medellín), el Módulo de Procesamiento de Rutas y Paquetes, la Base de Datos Distribuida y el Servicio de Monitoreo y Alertas.
2. **Agrupar por zona/capa:** los componentes se organizaron en 4 zonas — Clientes, Borde/Global, Región Bogotá y Región Medellín — lo que permite ver de un vistazo que Medellín depende de Bogotá para el procesamiento de rutas.
3. **Conectar los componentes:** se trazó el tráfico real, desde los clientes hasta el Balanceador de Carga, de ahí a cada API Gateway regional, y de estos al único módulo de procesamiento de rutas (en Bogotá) y a la Base de Datos Distribuida; ambas regiones reportan al Servicio de Monitoreo.
4. **Marcar redundancia y capacidad:** se identificó el Balanceador de Carga y la Base de Datos Distribuida como instancia única (sin redundancia), y el API Gateway de Medellín como dependiente de Bogotá para el procesamiento de rutas.
5. **Diagnosticar y priorizar:** con las marcas del paso anterior se construyó la tabla de diagnóstico priorizado (ver abajo), clasificando cada riesgo en disponibilidad, rendimiento o escalabilidad.

La herramienta utilizada fue draw.io. El trabajo se validó contra la checklist de autoevaluación de la guía antes de considerarse terminado.

## 🧩 Análisis del modelo propuesto
- **Estructura del modelo:** la agrupación por zonas geográficas es la que permite detectar visualmente el riesgo de escalabilidad de Medellín, algo que no sería evidente en un listado plano de componentes.
- **Representación de las necesidades del caso:** el mapa cubre las tres áreas críticas del caso base — latencia en rastreo en tiempo real (Base de Datos Distribuida, cuello de botella), riesgo de punto único de falla (Balanceador de Carga) y escalabilidad por zona geográfica (Medellín sin módulo de rutas propio).
- **Supuestos tomados:** se asumió que ambas regiones comparten la misma Base de Datos Distribuida con escritura centralizada en Bogotá, y que el Servicio de Monitoreo tiene redundancia (no fue señalado como riesgo en la guía).

## 📈 Diagrama final entregado
- Mapa de infraestructura: `entrega/mapa-final.drawio`

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| App Móvil - Usuario Final | Cliente | Punto de entrada de usuarios finales | RedExpress |
| App Móvil - Mensajero | Cliente | Punto de entrada de mensajeros | RedExpress |
| Portal Web - Operador | Cliente | Punto de entrada de operadores | RedExpress |
| Balanceador de Carga | Infraestructura (riesgo) | Instancia única — punto único de falla | RedExpress |
| API Gateway Bogotá | Servicio | Enrutamiento de tráfico de la región Bogotá | RedExpress |
| API Gateway Medellín | Servicio (riesgo) | Sin módulo de rutas propio — depende de Bogotá | RedExpress |
| Módulo de Procesamiento de Rutas y Paquetes | Servicio | Procesa rutas y estados de paquetes (solo en Bogotá) | RedExpress |
| Base de Datos Distribuida | Almacenamiento (riesgo) | Escritura única en Bogotá — cuello de botella de latencia | RedExpress |
| Servicio de Monitoreo y Alertas | Servicio | Observabilidad de ambas regiones | RedExpress |

**Tabla de diagnóstico priorizado:**

| Componente | Riesgo diagnosticado | Categoría | Impacto si ocurre | Prioridad |
|---|---|---|---|---|
| Balanceador de Carga (instancia única) | Punto único de falla | Disponibilidad | Toda la plataforma queda inaccesible | Alta |
| Base de Datos Distribuida (escritura única en Bogotá) | Cuello de botella de latencia | Rendimiento | Lentitud en rastreo en tiempo real fuera de Bogotá | Alta |
| Región Medellín sin módulo de rutas propio | Límite de escalabilidad geográfica | Escalabilidad | No se puede escalar demanda en Medellín sin saturar Bogotá | Media |

## 🔍 Investigación complementaria
### Tema investigado:
_No aplica en esta entrega — la investigación sobre buenas prácticas de arquitectura de infraestructura (cloud, on-premise, híbrida) corresponde a la Parte 2 del taller, pendiente de desarrollo con el sistema real del cliente._

### Resumen:
_Pendiente para la Parte 2._

## 📚 Referencias
Ver `entrega/referencias.md`.

---

_Este documento hace parte de la entrega de la Parte 1 (Trabajo en Clase) del Taller 4 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
