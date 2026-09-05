# Notas de Clase — Taller 3: Arquitectura Actual con el Modelo C4

**Caso trabajado:** RedExpress (caso base del taller)
**Archivos entregados en esta sesión:** `c1-contexto-borrador.drawio`, `c2-contenedores-borrador.drawio`

---

## Vista de Contexto (C1)

**Paso 1 — Actores identificados**
- Usuario Final: rastrea envíos y agenda recogidas.
- Mensajero: actualiza el estado de las entregas.
- Operador Logístico: gestiona rutas y despachos.

**Paso 2 — Sistema en alcance y sistemas externos**
- Sistema en alcance: **Plataforma RedExpress** (una sola caja, sin desglosar módulos internos).
- Sistemas externos: **API de Notificaciones** y **Proveedor de Geolocalización** (ambos de terceros, dibujados con doble borde para distinguirlos del sistema propio).

**Paso 3 — Relaciones trazadas**
- Los tres actores se conectan con la Plataforma RedExpress.
- La Plataforma RedExpress se conecta con los dos sistemas externos.

**Paso 4 — Etiquetas y validación**
| Origen | Destino | Etiqueta |
|---|---|---|
| Usuario Final | Plataforma RedExpress | Rastrea envíos y agenda recogidas |
| Mensajero | Plataforma RedExpress | Actualiza estado de entregas |
| Operador Logístico | Plataforma RedExpress | Gestiona rutas y despachos |
| Plataforma RedExpress | API de Notificaciones | Envía alertas de estado (API REST) |
| Plataforma RedExpress | Proveedor de Geolocalización | Consulta coordenadas y calcula rutas (API REST) |

✅ Checklist C1: todos los actores tienen relación directa, sistemas externos diferenciados visualmente, todas las flechas etiquetadas, ningún elemento queda aislado.

---

## Vista de Contenedores (C2)

**Paso 1 — Descomposición en contenedores**
La Plataforma RedExpress se abre en: App Móvil, Portal Web Operadores, Módulo de Gestión de Paquetes, Motor de Rutas, Seguimiento GPS, Sistema de Alertas.

**Paso 2 — Infraestructura de soporte**
- Balanceador de Carga (distribuye tráfico entrante).
- Base de Datos Distribuida (persiste paquetes, rutas y usuarios).

**Paso 3 — Relaciones trazadas**
Se conectan los contenedores entre sí y se retoman los actores/sistemas externos del C1, mostrando con qué contenedor específico interactúa cada uno (Usuario Final y Mensajero → App Móvil; Operador Logístico → Portal Web Operadores; Motor de Rutas → Proveedor de Geolocalización; Sistema de Alertas → API de Notificaciones).

**Paso 4 — Tecnología/protocolo y validación**
| Origen | Destino | Protocolo / mecanismo |
|---|---|---|
| Usuario Final | App Móvil | HTTPS/JSON |
| Mensajero | App Móvil | HTTPS/JSON |
| Operador Logístico | Portal Web Operadores | HTTPS/JSON |
| App Móvil | Balanceador de Carga | HTTPS |
| Portal Web Operadores | Balanceador de Carga | HTTPS |
| Balanceador de Carga | Módulo de Gestión de Paquetes | Enruta solicitudes |
| Módulo de Gestión de Paquetes | Base de Datos Distribuida | SQL |
| Módulo de Gestión de Paquetes | Motor de Rutas | Solicita ruta óptima |
| Motor de Rutas | Proveedor de Geolocalización | Consulta coordenadas (REST) |
| Módulo de Gestión de Paquetes | Seguimiento GPS | Solicita ubicación en tiempo real |
| Seguimiento GPS | App Móvil | Push/WebSocket |
| Módulo de Gestión de Paquetes | Sistema de Alertas | Dispara evento |
| Sistema de Alertas | API de Notificaciones | Envía alerta (REST) |

✅ Checklist C2: cada contenedor tiene tecnología/tipo indicado, responsabilidades separadas (sin contenedor "que hace de todo"), actores/externos del C1 se mantienen visibles, cada relación tiene protocolo, infraestructura de soporte representada.

---

## Puntos críticos de comunicación identificados
- El **Balanceador de Carga** es un punto único de entrada hacia el backend: si falla, tanto la App Móvil como el Portal Web quedan sin servicio.
- El **Módulo de Gestión de Paquetes** concentra casi todas las llamadas internas (a BD, Motor de Rutas, Seguimiento GPS y Sistema de Alertas), lo que lo convierte en el contenedor más acoplado del sistema.
- La relación **Seguimiento GPS → App Móvil (Push/WebSocket)** es la única de tipo "push" en tiempo real; el resto son solicitud/respuesta síncronas.
- Ambas integraciones externas (Geolocalización y Notificaciones) dependen de disponibilidad de terceros fuera del control del equipo.

## Retroalimentación del docente
> _Pendiente de completar durante la sesión de clase con las observaciones del profesor._

## Pendientes para la Parte 2 (aplicación al cliente real)
- Repetir los 4 pasos de C1 y C2 sobre el sistema real del cliente.
- Redactar `entrega/informe.md` con la plantilla de informe del taller.
- Investigar arquitecturas C4 del sector del cliente para `entrega/referencias.md`.
