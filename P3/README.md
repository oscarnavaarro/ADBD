# Práctica 3. Modelo entidad/relacion. Viveros

**Nombre y apellidos**: [Oscar Navarro Mesa](https://github.com/oscarnavaarro, "Enlace GitHub")

**Asignatura**: Administración y Diseño de Bases de Datos

[Enunciado de la práctica 3](https://docs.google.com/document/d/1_f9N4aYIWdDzIXCMY-D0ik8T9tAyWEbaFK6nMRIZnOk/edit?tab=t.0)

---

## Entidades y atributos

### 1. **VIVERO**
Representa cada centro o establecimiento de venta de Tajinaste S.A.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_vivero` *(PK)* | Identificador único del vivero | 1 |
| `nombre` | Nombre del vivero | "Vivero Norte" |
| `latitud` | Coordenada geográfica (grados decimales) | 28.4682 |
| `longitud` | Coordenada geográfica (grados decimales) | -16.2546 |

---

### 2. **ZONA**
Cada vivero se divide en zonas donde se ubican productos o trabajan empleados.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_zona` *(PK)* | Identificador único de la zona | 10 |
| `nombre` | Descripción de la zona | "Exterior plantas" |
| `latitud` | Coordenada de ubicación de la zona | 28.4685 |
| `longitud` | Coordenada de ubicación de la zona | -16.2549 |
| `id_vivero` *(FK)* | Referencia al vivero al que pertenece | 1 |

---

### 3. **PRODUCTO**
Artículos disponibles para la venta o el stock interno.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_producto` *(PK)* | Identificador único del producto | 101 |
| `nombre` | Nombre del producto | "Maceta de cerámica" |
| `tipo` | Categoría del producto | "Decoración" |
| `precio` | Precio unitario en euros | 12.50 |

---

### 4. **STOCK**
Representa la cantidad disponible de cada producto en cada zona.  
Entidad intermedia entre **ZONA** y **PRODUCTO**.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_zona` *(FK)* | Zona donde se encuentra el producto | 10 |
| `id_producto` *(FK)* | Producto disponible | 101 |
| `cantidad_disponible` | Cantidad en stock | 35 unidades |

---

### 5. **EMPLEADO**
Representa al personal de la empresa.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_empleado` *(PK)* | Identificador único | 2001 |
| `nombre` | Nombre completo | "Lucía Morales" |
| `dni` | Documento nacional de identidad | "98765432K" |
| `fecha_contratacion` | Fecha de alta en la empresa | 2020-03-15 |

---

### 6. **HISTORICO_EMPLEADO**
Registra los destinos y productividad del empleado a lo largo del tiempo.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_historico` *(PK)* | Identificador del registro histórico | 501 |
| `id_empleado` *(FK)* | Empleado al que pertenece el registro | 2001 |
| `id_vivero` *(FK)* | Vivero asignado | 1 |
| `id_zona` *(FK)* | Zona donde trabaja | 10 |
| `fecha_inicio` | Fecha de inicio del destino | 2023-06-01 |
| `fecha_fin` | Fecha de finalización (puede ser NULL) | NULL |
| `productividad` | Índice o métrica de desempeño | 0.92 |

---

### 7. **CLIENTE**
Datos de los clientes que realizan compras.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_cliente` *(PK)* | Identificador único | 3001 |
| `nombre` | Nombre completo | "Carlos Pérez" |
| `telefono` | Teléfono de contacto | 678123456 |
| `email` | Correo electrónico | "carlos.perez@gmail.com" |

---

### 8. **TAJINASTE_PLUS**
Clientes inscritos en el programa de fidelización.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_cliente` *(PK, FK)* | Cliente inscrito en el programa | 3001 |
| `fecha_ingreso` | Fecha de alta en el programa | 2023-05-12 |
| `bonificacion_mensual` | Porcentaje de bonificación aplicado | 5% |
| `volumen_compras_mensual` | Total de compras del mes (€) | 320.00 |

---

### 9. **PEDIDO**
Representa una orden de compra de un cliente gestionada por un empleado.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_pedido` *(PK)* | Identificador del pedido | 7001 |
| `fecha_pedido` | Fecha en que se realiza el pedido | 2024-04-05 |
| `importe_total` | Importe total del pedido (€) | 58.00 |
| `id_cliente` *(FK)* | Cliente que realiza el pedido | 3001 |
| `id_empleado` *(FK)* | Empleado responsable | 2001 |

---

### 10. **DETALLE_PEDIDO**
Productos incluidos en cada pedido.  
Entidad intermedia entre **PEDIDO** y **PRODUCTO**.

| Atributo | Descripción | Ejemplo |
|-----------|-------------|---------|
| `id_pedido` *(FK)* | Pedido al que pertenece | 7001 |
| `id_producto` *(FK)* | Producto incluido | 101 |
| `cantidad` | Unidades solicitadas | 3 |
| `precio_unitario` | Precio de cada unidad (€) | 12.50 |

---

## Relaciones y cardinalidades

| Relación | Entidades implicadas | Cardinalidad | Descripción |
|-----------|----------------------|---------------|--------------|
| **Tiene** | VIVERO – ZONA | 1:N | Un vivero tiene varias zonas; cada zona pertenece a un solo vivero. |
| **Asigna (Stock)** | ZONA – PRODUCTO | N:M | Un producto puede estar en varias zonas y una zona contiene varios productos. |
| **Registra** | VIVERO – HISTORICO_EMPLEADO | 1:N | Un vivero registra la asignación de varios empleados a lo largo del tiempo. |
| **Ubica** | ZONA – HISTORICO_EMPLEADO | 1:N | Un empleado desarrolla su labor en una zona concreta durante un periodo. |
| **Tiene histórico** | EMPLEADO – HISTORICO_EMPLEADO | 1:N | Un empleado puede tener varios registros históricos de destino. |
| **Pertenece** | CLIENTE – TAJINASTE_PLUS | 1:0..1 | Un cliente puede o no pertenecer al programa Tajinaste Plus. |
| **Realiza** | CLIENTE – PEDIDO | 1:N | Un cliente puede realizar varios pedidos. |
| **Gestiona** | EMPLEADO – PEDIDO | 1:N | Cada pedido tiene un empleado responsable. |
| **Contiene (Detalle)** | PEDIDO – PRODUCTO | N:M | Un pedido puede incluir varios productos y un producto puede estar en varios pedidos. |

---

## Restricciones semánticas

1. **No solapamiento de destinos de empleados**
   > Para cada empleado, los intervalos de fechas (`fecha_inicio`, `fecha_fin`) en la entidad `HISTORICO_EMPLEADO` **no pueden solaparse**.  
   > Si `fecha_fin` es `NULL`, no puede existir otro registro con `fecha_fin` `NULL` para ese mismo empleado.

2. **Bonificaciones Tajinaste Plus**
   > Solo los clientes registrados en `TAJINASTE_PLUS` pueden acumular bonificaciones y volumen de compras mensual.

3. **Integridad de stock**
   > La combinación `(id_zona, id_producto)` en `STOCK` debe ser única para evitar duplicidades en el conteo de productos por zona.

4. **Integridad de detalle de pedido**
   > La combinación `(id_pedido, id_producto)` en `DETALLE_PEDIDO` debe ser única y su suma debe coincidir con el `importe_total` del pedido.


