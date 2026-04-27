Aquí tienes un ejemplo claro de cómo diseñar una base de datos **NoSQL (tipo MongoDB)** para una **tienda de videojuegos**, usando **colecciones (equivalentes a tablas)** y **documentos (registros)** con sus atributos y tipos de datos.

---

# 🎮 Base de datos: `tienda_videojuegos`

## 📦 Colección: `videojuegos`

Representa los productos principales.

**Documento de ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "The Legend of Zelda: Breath of the Wild",
  "genero": "Aventura",
  "precio": 1299.99,
  "plataforma": ["Nintendo Switch", "Wii U"],
  "stock": 25,
  "fecha_lanzamiento": ISODate("2017-03-03"),
  "desarrollador": "Nintendo",
  "clasificacion": "E10+",
  "activo": true
}
```

**Atributos y tipos:**

* `nombre`: String
* `genero`: String
* `precio`: Decimal
* `plataforma`: Array de Strings
* `stock`: Int
* `fecha_lanzamiento`: Date
* `desarrollador`: String
* `clasificacion`: String
* `activo`: Boolean

---

## 👤 Colección: `clientes`

Información de los compradores.

**Documento de ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Juan Pérez",
  "email": "juan@email.com",
  "telefono": "6561234567",
  "direccion": {
    "calle": "Av. Tecnológico",
    "ciudad": "Ciudad Juárez",
    "estado": "Chihuahua",
    "cp": "32500"
  },
  "fecha_registro": ISODate("2026-04-20"),
  "activo": true
}
```

**Atributos y tipos:**

* `nombre`: String
* `email`: String
* `telefono`: String
* `direccion`: Object

  * `calle`: String
  * `ciudad`: String
  * `estado`: String
  * `cp`: String
* `fecha_registro`: Date
* `activo`: Boolean

---

## 🧾 Colección: `ventas`

Registro de compras realizadas.

**Documento de ejemplo:**

```json
{
  "_id": ObjectId,
  "cliente_id": ObjectId,
  "fecha": ISODate("2026-04-25"),
  "total": 2599.98,
  "metodo_pago": "Tarjeta",
  "productos": [
    {
      "videojuego_id": ObjectId,
      "nombre": "Zelda BOTW",
      "cantidad": 1,
      "precio_unitario": 1299.99
    },
    {
      "videojuego_id": ObjectId,
      "nombre": "Mario Kart 8",
      "cantidad": 1,
      "precio_unitario": 1299.99
    }
  ]
}
```

**Atributos y tipos:**

* `cliente_id`: ObjectId (referencia a clientes)
* `fecha`: Date
* `total`: Decimal
* `metodo_pago`: String
* `productos`: Array de objetos

  * `videojuego_id`: ObjectId
  * `nombre`: String
  * `cantidad`: Int
  * `precio_unitario`: Decimal

---

## 🏢 Colección: `proveedores`

Empresas que suministran videojuegos.

**Documento de ejemplo:**

```json
{
  "_id": ObjectId,
  "nombre": "Distribuidora Gaming MX",
  "contacto": "Carlos López",
  "telefono": "6569876543",
  "email": "proveedor@gamingmx.com",
  "direccion": "Ciudad Juárez, Chihuahua",
  "activo": true
}
```

**Atributos y tipos:**

* `nombre`: String
* `contacto`: String
* `telefono`: String
* `email`: String
* `direccion`: String
* `activo`: Boolean

---

## 🧩 Relación entre colecciones (concepto)

* `ventas.cliente_id` → referencia a `clientes`
* `ventas.productos.videojuego_id` → referencia a `videojuegos`
* Puedes agregar `proveedor_id` en `videojuegos` si quieres relación con proveedores

---

## 💡 Notas importantes

* MongoDB no usa tablas, usa **colecciones**.
* No necesitas relaciones estrictas como SQL, puedes **incrustar datos** (como en `productos` dentro de `ventas`).
* `ObjectId` es un identificador único automático.

---

Si quieres, puedo darte el mismo ejemplo pero en:

* 🐍 Django (models.py con relaciones)
* 🗄️ SQL (MySQL/PostgreSQL)
* 🧠 Diagrama ER (visual)

Solo dime cuál necesitas 👍
