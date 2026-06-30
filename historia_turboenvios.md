# 📦 TurboEnvíos: la historia detrás de los datos

> Una forma de entender qué guarda el archivo [turboenvios_entregas.csv](turboenvios_entregas.csv) sin mirar números fríos, sino contando lo que pasa en un día cualquiera.

---

## El día empieza en el almacén

Imagina la central de **TurboEnvíos**, una empresa de mensajería urbana. Cada mañana llegan paquetes de todos los tamaños y, durante el día, salen **50.000 entregas** repartidas por toda la ciudad. El CSV no es más que el **diario de a bordo** de la empresa: una fila por cada paquete que sale a la calle.

Sigamos el viaje de un paquete cualquiera, el **pedido #4**, para entender qué anota la empresa de cada envío.

---

## El viaje del pedido #4

> `4, 2026-06-15, Centro, Express, 3.74, 5.49, 41.4, 11186.0, 2.0`

| Lo que cuenta la historia | Columna en el CSV | Valor |
|---|---|---|
| "Es el cuarto paquete que registramos" | **`pedido_id`** | `4` — número único, como el ticket de cada envío |
| "Salió el 15 de junio de 2026" | **`fecha`** | `2026-06-15` — todos los envíos son de junio de 2026 |
| "Va al barrio Centro" | **`zona`** | `Centro` — la parte de la ciudad donde se entrega |
| "El cliente pagó el servicio rápido" | **`tipo_servicio`** | `Express` — frente al `Estándar` más barato |
| "El repartidor recorrió 3,74 km" | **`distancia_km`** | `3.74` — distancia del trayecto |
| "El paquete pesaba 5,49 kg" | **`peso_kg`** | `5.49` — peso del envío |
| "Tardó 41 minutos en llegar" | **`tiempo_entrega_min`** | `41.4` — minutos puerta a puerta |
| "Costó 11.186 (en la moneda local)" | **`costo_envio`** | `11186.0` — lo que se cobró |
| "El cliente lo valoró con un 2 de 5 😕" | **`calificacion_cliente`** | `2.0` — satisfacción de 1 a 5 |

Con esas **9 columnas**, la empresa puede reconstruir la vida entera de cualquier envío: cuándo, a dónde, cómo, cuánto pesaba, cuánto tardó, cuánto costó y si el cliente quedó contento.

---

## Los personajes de la ciudad (las zonas)

No todos los barrios reciben el mismo número de paquetes. Así se reparte el trabajo:

| Zona | Entregas | ¿Quién vive ahí? |
|---|---|---|
| **Periferia** | 14.171 | La más movida: las afueras piden mucho |
| **Centro** | 13.870 | El corazón de la ciudad, casi tan activo |
| **Comercial** | 8.090 | Tiendas y oficinas |
| **Industrial** | 7.767 | Naves y fábricas |
| **Residencial** | 6.102 | Los barrios de viviendas, los más tranquilos |

---

## Dos velocidades de servicio

Cada cliente elige cómo quiere que llegue su paquete:

- 🚀 **Express** → 14.919 envíos (los que tienen prisa)
- 🚚 **Estándar** → 35.081 envíos (la mayoría, el ritmo normal)

Más del 70 % de la gente elige el envío normal; solo 3 de cada 10 pagan por la rapidez.

---

## ¿Qué preguntas le puedes hacer a este diario?

Como el CSV guarda toda esta información, puedes investigar cosas como:

- 🕐 **¿Tardan más los envíos a la Periferia que al Centro?** (mira `zona` + `tiempo_entrega_min`)
- 💰 **¿Cuánto más cuesta de media un Express?** (compara `tipo_servicio` + `costo_envio`)
- ⭐ **¿Los clientes que esperan mucho califican peor?** (cruza `tiempo_entrega_min` + `calificacion_cliente`)
- ⚖️ **¿Los paquetes más pesados o más lejanos cuestan más?** (`peso_kg`, `distancia_km` vs `costo_envio`)
- 📅 **¿Qué días de junio hubo más actividad?** (agrupa por `fecha`)

---

## Resumen rápido

- **Qué es:** registro de entregas de una empresa de mensajería urbana.
- **Cuántas filas:** 50.000 envíos (+ 1 fila de encabezado).
- **Cuántas columnas:** 9.
- **Cuándo:** junio de 2026.
- **Tipos de dato:**
  - 🔢 **Números:** `pedido_id`, `distancia_km`, `peso_kg`, `tiempo_entrega_min`, `costo_envio`, `calificacion_cliente`
  - 📅 **Fecha:** `fecha`
  - 🏷️ **Categorías (texto):** `zona`, `tipo_servicio`

Cada fila es un paquete con una historia; el CSV es la biblioteca completa de todas esas historias. 📚
