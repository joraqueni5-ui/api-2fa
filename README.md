# API REST Laravel 2FA

API REST con autenticación de dos factores (2FA), dispositivos de confianza y CRUD de servicios con soporte Base64.

## Tecnologías

- Laravel 11
- Laravel Sanctum (tokens)
- MySQL
- Gmail (envío de correos OTP)

## Requisitos previos

- PHP 8.2.12
- Composer
- MySQL
- XAMPP

## Instalación paso a paso

### 1. Clonar el repositorio

```bash
git clone https://github.com/joraqueni5-ui/api-2fa.git
cd api-2fa
```

### 2. Instalar dependencias

```bash
composer install
```

### 3. Copiar archivo de entorno

```bash
cp .env.example .env
```

### 4. Generar clave de aplicación

```bash
php artisan key:generate
```

### 5. Configurar base de datos en .env

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=api_2fa
DB_USERNAME=root
DB_PASSWORD=
```

### 6. Configurar Gmail en .env

```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=tucorreo@gmail.com
MAIL_PASSWORD=tu_clave_de_aplicacion
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="tucorreo@gmail.com"
MAIL_FROM_NAME="API 2FA"
```

### 7. Ejecutar migraciones y seeder

```bash
php artisan migrate:fresh --seed
```

### 8. Levantar el servidor

```bash
php artisan serve
```

El servidor estará disponible en: `http://127.0.0.1:8000`

## Credenciales de prueba

| Campo    | Valor          |
| -------- | -------------- |
| Email    | admin@test.com |
| Password | password123    |

## Endpoints disponibles

### Autenticación (públicos)

| Método | Endpoint             | Descripción                |
| ------ | -------------------- | -------------------------- |
| POST   | /api/auth/login      | Login con email y password |
| POST   | /api/auth/verify-otp | Verificar código OTP       |

### Autenticación (protegidos)

| Método | Endpoint         | Descripción             |
| ------ | ---------------- | ----------------------- |
| POST   | /api/auth/logout | Cerrar sesión           |
| GET    | /api/auth/me     | Ver usuario autenticado |

### Servicios (protegidos con Bearer Token)

| Método | Endpoint           | Descripción                     |
| ------ | ------------------ | ------------------------------- |
| GET    | /api/services      | Listar servicios activos        |
| POST   | /api/services      | Crear servicio con foto Base64  |
| GET    | /api/services/{id} | Ver un servicio                 |
| PUT    | /api/services/{id} | Actualizar servicio             |
| DELETE | /api/services/{id} | Eliminar servicio (Soft Delete) |

## Flujo de autenticación 2FA

1. POST /api/auth/login → Sistema envía OTP al correo (válido 10 minutos)
2. POST /api/auth/verify-otp → Retorna Bearer Token
3. Próximo login desde dispositivo de confianza → Se omite el OTP

## Repositorio Frontend

https://github.com/joraqueni5-ui/Frontend-web
