# Laboratorio 4: Microservicio GraphQL con Node.js, Express y MySQL

Este proyecto implementa un microservicio **GraphQL** en Node.js que administra recursos de **Usuarios** y **Productos** conectándose a una base de datos MySQL mediante el driver `mysql2`. Aplica esquemas tipados, resolutores con consultas SQL parametrizadas, manejo de variables de entorno y validaciones de datos.

---

## 📋 Requisitos Previos

Asegúrate de contar con lo siguiente instalado en tu equipo:

* **Node.js**: v18.0.0 o superior
* **npm**: v9.0.0 o superior
* **MySQL Server**: v8.0 o compatible (MySQL Workbench, phpMyAdmin o consola)
* **Postman**: Para la ejecución de pruebas del servicio

---

## 🛠️ Instalación

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/Juan-Zambrano5/Laboratorio-4-Taller-GraphQL-Mysql-Postman.git](https://github.com/Juan-Zambrano5/Laboratorio-4-Taller-GraphQL-Mysql-Postman.git)
   cd Laboratorio-4-Taller-GraphQL-Mysql-Postman
Instalar dependencias del proyecto:

Bash
npm install
⚙️ Configuración
1. Base de Datos MySQL
Abre tu cliente de MySQL (MySQL Workbench, phpMyAdmin o la consola).

Ejecuta el script incluido en el archivo database.sql para crear la base de datos graphql_db, las tablas users y products, e insertar los datos iniciales de prueba:

SQL
CREATE DATABASE IF NOT EXISTS graphql_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE graphql_db;

CREATE TABLE IF NOT EXISTS users (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(150) NOT NULL UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS products (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(150) NOT NULL,
  description TEXT,
  price DECIMAL(10,2) NOT NULL CHECK (price > 0),
  stock INT NOT NULL DEFAULT 0 CHECK (stock >= 0),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO users (name, email) VALUES
  ('Ana Torres', 'ana@example.com'),
  ('Carlos Parra', 'carlos@example.com')
ON DUPLICATE KEY UPDATE email=email;

INSERT INTO products (name, description, price, stock) VALUES
  ('Teclado Mecánico', 'Teclado RGB switch blue', 45.99, 15),
  ('Mouse Gamer', 'Mouse óptico 16000 DPI', 25.50, 30);
2. Variables de Entorno
Duplica el archivo .env.example y renómbralo a .env:

Bash
cp .env.example .env
Edita el archivo .env agregando tus credenciales locales de acceso a MySQL:

Fragmento de código
PORT=4000
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=tu_contraseña_mysql
DB_NAME=graphql_db
🚀 Ejecución
Modo Desarrollo (con reinicio automático vía Nodemon)
Bash
npm run dev
Modo Producción
Bash
npm start
Verificación del Servicio
Health Check: Abre http://localhost:4000/health en tu navegador. Debe retornar:

JSON
{
  "status": "ok",
  "service": "usuarios-productos-graphql"
}
Endpoint GraphQL: Disponible en http://localhost:4000/graphql para solicitudes de tipo POST.

📁 Estructura del Proyecto
Plaintext
usuarios-graphql/
├── src/
│   ├── config/
│   │   └── db.js               # Conexión y Pool a MySQL con mysql2
│   ├── graphql/
│   │   ├── schema.js           # Esquema GraphQL (Tipos, Query, Mutation)
│   │   └── resolvers.js        # Lógica de negocio y consultas SQL parametrizadas
│   └── index.js                # Servidor Express y endpoint GraphQL
├── .env                        # Variables de entorno (excluido de Git)
├── .env.example                # Plantilla de variables de entorno
├── .gitignore                  # Exclusión de node_modules y .env
├── database.sql                # Script de creación e inicialización de BD
├── Postman_Collection.json     # Colección exportada de pruebas en Postman
├── package.json                # Configuración del proyecto y scripts
└── README.md                   # Documentación del proyecto
🧪 Pruebas en Postman
Importa el archivo Postman_Collection.json en Postman.

Asegúrate de configurar la variable de colección baseUrl = http://localhost:4000.

Selecciona el método POST para las peticiones dirigidas a {{baseUrl}}/graphql.

Ejecuta las pruebas del flujo CRUD completo para Usuarios y Productos, incluyendo los casos negativos (emails duplicados, precios de producto <= 0 e IDs inexistentes).


---

### Pasos finales en tu terminal para guardarlo y subirlo a GitHub:

```bash
git add README.md
git commit -m "docs: agregar README completo del proyecto"
git push origin main
