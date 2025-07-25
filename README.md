# 🛒 Mini Super "Despensa del Corazón"
---
**EQUIPO 11**
Desarrollado por:

- Melody Nathalie Mendoza Jimenez
- Josue Saul Lopez Trujillo(https://github.com/Saul-LT)
---


**Sistema de Gestión de Ventas con Emisión de Tickets y Envío de Correos Electrónicos**

Este proyecto es una aplicación de escritorio desarrollada en Java con una interfaz gráfica construida con Swing. Está diseñado para gestionar de forma eficiente el proceso de venta en un mini supermercado, permitiendo el registro de clientes, manejo de carrito de compras, generación de tickets en formato PDF, y envío automático de correos electrónicos con los detalles de la transacción.

---

## 📌 Propósito del Proyecto

El sistema busca automatizar las operaciones de un mini supermercado, ofreciendo una interfaz intuitiva para:

- Registrar clientes automáticamente (si son nuevos).
- Realizar ventas y generar tickets digitales.
- Enviar el ticket PDF por correo al cliente.
- Gestionar pagos con métodos: Efectivo y Tarjeta.
- Mantener actualizada la base de datos con control de stock.
  
---

## 📚 Librerías Externas Utilizadas

A continuación se enlistan las librerías externas utilizadas en el proyecto, junto con su propósito específico:

| Librería                        | Uso en el Proyecto                                                                 |
|---------------------------------|-------------------------------------------------------------------------------------|
| `mysql-connector-java`         | Permite la conexión entre la aplicación Java y la base de datos MySQL mediante JDBC. |
| `javax.mail` *(opcional)*      | Si se incluye, podría usarse para envío de correos (no parece estar en el proyecto por defecto, pero se ha visto en otros similares). |
| `javax.swing`                  | Aunque es parte del JDK, se usa extensamente para la construcción de interfaces gráficas (formularios, botones, tablas, etc.). |
| `java.sql`                     | Utilizada para ejecutar consultas SQL, manejar conexiones, `ResultSet`, `PreparedStatement`, etc. |

### 📦 Dependencias Clave

- **mysql-connector-java-X.X.X.jar**
  - Asegúrarse de incluir esta librería en tu el para que funcione la conexión con MySQL.
  - Se utiliza en la clase `ConexionBD.java` dentro del paquete `conexionbd`.

---

## 🚀 Funcionalidades Principales

- 🧑 Registro automático de nuevos clientes con validación de nombre y correo.
- 📧 Verificación de existencia de cliente mediante su correo electrónico.
- 🛍️ Gestión completa del carrito de compras: añadir, eliminar y verificar productos.
- 📦 Control de stock en tiempo real y prevención de stock negativo.
- 🧾 Generación de tickets en formato PDF con detalles completos de la compra:
  - ID, nombre del producto, cantidad, subtotal, precio individual.
  - Monto total y tipo de pago (efectivo o tarjeta).
- 📬 Envío de correo electrónico con el ticket adjunto.
- 💳 Soporte para múltiples métodos de pago:
  - Efectivo (con cálculo de cambio).
  - Tarjeta (con captura de número parcial y NIP).
- 🔒 Validación de método de pago obligatoria antes de procesar una venta.
- 🧹 Limpieza automática del formulario al finalizar la venta.

---

## 🛠️ Tecnologías y Herramientas

- **Lenguaje**: Java SE 8+
- **Interfaz gráfica**: Java Swing
- **Base de datos**: MySQL 
- **PDF**: iTextPDF 5.x
- **Correo**: JavaMail API
- **IDE recomendado**: NetBeans 12+ o IntelliJ IDEA

---

## 🏛️ Arquitectura del Proyecto

El sistema está dividido en las siguientes capas:

### 1. Interfaz Gráfica (Vista)
- Diseñada con **Java Swing**.
- Formulario de venta principal, con campos de cliente, productos, tipo de pago, etc.
- Campos dinámicos según el método de pago.

### 2. Lógica de Negocio (Controlador)
- Controla la inserción de ventas, actualización de stock y validaciones de campos.
- Verifica si el cliente ya existe por su correo.
- Calcula cambio (en caso de pago en efectivo).

### 3. Acceso a Datos (Modelo)
- Conexión y consultas a la base de datos MySQL.
- Tablas: `usuarios`, `productos`, `ventas`, `detalle_venta`, `clientes`.

### 4. Generación de Documentos
- Generación de PDF con **iText** (`CorreoPDF.java`).
- Incluye datos de la venta y método de pago.

### 5. Envío de Correo
- Envío automático del PDF por SMTP usando **JavaMail API**.
- Compatible con cuentas Gmail (requiere contraseña de aplicación).

---

## 🧾 Requisitos Previos

- Tener instalado Java JDK 8 o superior.
- Tener una base de datos MySQL/PostgreSQL activa y configurada.
- Importar librerías externas:
  - iTextPDF (versión 5.x)
  - JavaMail (`javax.mail.jar`)
  - Java Activation Framework (`activation.jar`)

---

## ⚙️ Configuración de la Base de Datos

Edita la clase `Conexion_Base.java` para establecer la conexión:

```java
String url = "jdbc:mysql://localhost:3306/tu_basedatos";
String user = "usuario";
String password = "contraseña";
```

Asegúrate de que la base de datos contenga las siguientes tablas: `usuarios`, `productos`, `ventas`, `detalle_venta`, `clientes`.

---

## ✉️ Configuración del Correo

En la clase `CorreoPDF.java`, define tu cuenta de Gmail y una contraseña de aplicación válida:

```java
final String remitente = "tucorreo@gmail.com";
final String contraseña = "contraseña_de_aplicacion"; // Generada desde tu cuenta de Google
```
---

## ✨ Funcionalidades Principales

- 🔍 **Verificación automática de cliente:** si el correo no existe, solicita nombre y lo registra.
- 🛒 **Gestión de carrito:** agregar productos y calcular total.
- 💳 **Métodos de pago:**
  - Efectivo: solicita cantidad entregada y calcula cambio.
  - Tarjeta: solicita número de cuenta (enmascarado en PDF) y NIP.
- 📧 **Ticket en PDF enviado automáticamente al correo del cliente.**
- 📉 **Descuento de stock según cantidad vendida.**
- 🧾 **PDF incluye nombre del cliente, lista de productos, precios unitarios, subtotales, total, cambio, y tipo de pago.**

---

## 🔍 Métodos Importantes

### `confirmarVenta(int idCajero)`
Controla toda la lógica de una venta:
- Verifica cliente.
- Inserta venta y detalles.
- Actualiza stock.
- Llama a `CorreoPDF.generarPDFVenta(...)`.

### `generarPDFVenta(...)`
Genera el ticket PDF de venta:
- Muestra tabla con: ID, Producto, Cantidad, Precio Unitario, Subtotal.
- Añade tipo de pago.
- Envía por correo automáticamente.

### `obtenerDatosCarrito()`
Convierte la tabla del carrito en un arreglo de `String[][]` para la tabla PDF.

### `registrarCliente(...)` y `buscarClientePorCorreo(...)`
Operaciones básicas para registrar o recuperar clientes según correo electrónico.

---

## 📬 Envío del Ticket PDF

- El ticket se envía desde: `despensadelcorazon@gmail.com`
- Usa `javax.mail` con configuración SMTP (puerto 587).
- Asunto: *Bienvenido a la DESPENSA DEL CORAZÓN*
- Adjunta: `TicketVenta_NombreCliente.pdf`


---

## 🖥️ Ejecución

1. Importa el proyecto en NetBeans.
2. Añade las librerías externas requeridas al classpath.
3. Ejecuta el formulario `Carrito.java` para iniciar el sistema.
4. Realiza ventas y verifica el envío de correos con PDFs adjuntos.

---

## 📌 Consideraciones

- Si se ingresa un correo ya registrado, el sistema no pide nombre nuevamente.
- El cambio solo se calcula si se selecciona pago en efectivo.
- El ticket muestra los precios individuales y el tipo de pago al final del documento.
- El sistema no permite procesar una venta sin haber validado el pago.

---

## 📸 Capturas de Pantalla

A continuación se muestran algunas vistas clave del sistema:

### Uso del CAPTCHA 
![Inicio de sesión(imagenes/IMG-20250725-WA0024.jpg)


### 🧾 Ventana de Registro de Usuario
![Registro de Usuario]<img width="1245" height="790" alt="image" src="https://github.com/user-attachments/assets/34128b2d-c195-4af1-9d23-4db8bf17ea83" />
### 📄  PDF de Bienvenida (CONFIRMACION DE REGISTRO)
![Confirmación de Registro]![Imagen de WhatsApp 2025-07-25 a las 07 07 54_f571f64b](https://github.com/user-attachments/assets/b3d6d8ce-a93e-4006-b88b-50a3a1d9215d)

### 🛒 Panel de Venta con Productos
![Carrito de Compras]<img width="1349" height="985" alt="image" src="https://github.com/user-attachments/assets/053435b1-325d-4899-95ff-196f3b48c5ca" />
### Realizando la Compra
<img width="1325" height="966" alt="image" src="https://github.com/user-attachments/assets/d6a50a8e-10a5-44fa-ba2d-12f2f3e73264" />

### 📄 Ticket PDF generado
![Ticket de Venta]<img width="627" height="808" alt="image" src="https://github.com/user-attachments/assets/1b19c11c-c898-4241-b4ea-61405d298ea2" />

---

## ⚙️ Clonar y Ejecutar el Proyecto

1. Clonar el repositorio:
```bash
git clone https://github.com/tu_usuario/Proyecto_BDt.git
cd Proyecto_BDt
```

2. Abrir el proyecto en NetBeans.

3. Configurar la conexión a la base de datos en la clase `Conexion_Base.java` si es necesario.

4. Crear la base de datos y tablas usando Workbench o phpMyAdmin según el archivo `base_datos.sql` incluido.

5. Ejecutar la clase `Login.java` para iniciar el sistema.

---

## 🔐 Credenciales de Ejemplo
Puedes usar estas credenciales para probar el sistema:

- Usuario: `admin@despensa.com`
- Contraseña: `admin123`

---


## 📜 Licencia

Este proyecto está destinado exclusivamente para fines educativos y académicos.  
© 2025 - Todos los derechos reservados.
