# 📋 Todo API — Gestión de Tareas en Go

API REST completa construida en Go puro (`net/http`), con autenticación JWT y persistencia en SQLite.

---

## 🗂️ Estructura del Proyecto

```
todo-api/
├── cmd/
│   └── main.go                  ← Punto de entrada, router
├── internal/
│   ├── auth/
│   │   └── jwt.go               ← Generación y validación de tokens JWT
│   ├── handlers/
│   │   ├── auth.go              ← Register / Login
│   │   └── tasks.go             ← CRUD de tareas
│   ├── middleware/
│   │   └── middleware.go        ← Logger + Auth JWT
│   ├── models/
│   │   ├── models.go            ← Structs: Task, User, Requests
│   │   └── errors.go            ← Errores de dominio
│   └── repository/
│       └── sqlite.go            ← Capa de acceso a datos (SQLite)
├── db/
│   └── todo.db                  ← Base de datos (se crea automáticamente)
├── docs/
│   └── api.http                 ← Ejemplos de peticiones HTTP
├── go.mod
└── README.md
```

---

## 🚀 Cómo ejecutar

```bash
# 1. Instalar dependencias
go mod tidy

# 2. Compilar y correr
go run cmd/main.go

# 3. Verificar que funciona
curl http://localhost:8080/api/health
```

---

## 📡 Endpoints de la API

| Método   | Ruta                          | Auth | Descripción               |
|----------|-------------------------------|------|---------------------------|
| `POST`   | `/api/auth/register`          | ❌   | Registrar usuario         |
| `POST`   | `/api/auth/login`             | ❌   | Iniciar sesión (→ JWT)    |
| `GET`    | `/api/health`                 | ❌   | Estado del servidor       |
| `GET`    | `/api/tasks`                  | ✅   | Listar todas las tareas   |
| `POST`   | `/api/tasks`                  | ✅   | Crear nueva tarea         |
| `GET`    | `/api/tasks/{id}`             | ✅   | Obtener tarea por ID      |
| `PUT`    | `/api/tasks/{id}`             | ✅   | Actualizar tarea          |
| `DELETE` | `/api/tasks/{id}`             | ✅   | Eliminar tarea            |
| `PATCH`  | `/api/tasks/{id}/complete`    | ✅   | Marcar como completada    |

---

## 🔐 Autenticación

La API usa **JWT (JSON Web Tokens)**. Flujo:

1. Registrarse o hacer login → recibir `token`
2. Incluir en cada petición protegida:
   ```
   Authorization: Bearer <token>
   ```

---

## 📦 Ejemplos de uso (curl)

### Registrar usuario
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"juan","email":"juan@email.com","password":"123456"}'
```

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"juan@email.com","password":"123456"}'
```

### Crear tarea
```bash
curl -X POST http://localhost:8080/api/tasks \
  -H "Authorization: Bearer <TU_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{"title":"Estudiar Go","description":"Capítulos 1 al 5","priority":"alta"}'
```

### Listar tareas
```bash
curl http://localhost:8080/api/tasks \
  -H "Authorization: Bearer <TU_TOKEN>"
```

---

## 📅 Plan Semana a Semana (15 horas)

### 📌 Semana 1 — Fundamentos de Go (3h)
**Objetivo:** Entender Go y montar el esqueleto del proyecto

| Actividad | Tiempo |
|-----------|--------|
| Instalar Go, VSCode + extensión Go | 20 min |
| Tipos básicos, structs, interfaces | 40 min |
| `go mod init`, estructura de carpetas | 20 min |
| Servidor HTTP con `net/http` | 40 min |
| Primer endpoint `GET /health` | 40 min |

**Conceptos clave:** `package`, `import`, `struct`, `func`, `error`, `net/http`

---

### 📌 Semana 2 — Modelos y CRUD en Memoria (3h)
**Objetivo:** Implementar el CRUD completo sin base de datos

| Actividad | Tiempo |
|-----------|--------|
| Definir structs Task y User | 30 min |
| Almacenamiento en `map` (en memoria) | 30 min |
| Handlers: Create, GetAll, GetByID | 60 min |
| Handlers: Update, Delete | 45 min |
| Validación de inputs | 15 min |

**Conceptos clave:** `encoding/json`, `http.Handler`, `PathValue`, `map`, `slice`

---

### 📌 Semana 3 — Persistencia con SQLite (3h)
**Objetivo:** Reemplazar el almacenamiento en memoria por SQLite

| Actividad | Tiempo |
|-----------|--------|
| Introducción a `database/sql` | 30 min |
| Instalar `go-sqlite3`, crear tablas | 40 min |
| Implementar repositorio: Create, GetAll | 50 min |
| Implementar repositorio: Update, Delete | 40 min |
| Migrar handlers para usar repositorio | 20 min |

**Conceptos clave:** `database/sql`, `sql.Row`, `sql.Rows`, interfaces, patrón Repository

---

### 📌 Semana 4 — Autenticación JWT (3h)
**Objetivo:** Proteger la API con tokens JWT

| Actividad | Tiempo |
|-----------|--------|
| ¿Qué es JWT? Teoría y estructura | 20 min |
| Instalar `golang-jwt`, generar tokens | 40 min |
| Endpoint Register con bcrypt | 40 min |
| Endpoint Login | 30 min |
| Middleware de autenticación | 50 min |

**Conceptos clave:** `context`, middleware, `bcrypt`, JWT claims, HTTP headers

---

### 📌 Semana 5 — Pruebas y Documentación (3h)
**Objetivo:** Pruebas unitarias, mejoras y presentación

| Actividad | Tiempo |
|-----------|--------|
| Pruebas con `testing` package | 60 min |
| Documentar con archivo `.http` | 30 min |
| Manejo de errores mejorado | 30 min |
| Variables de entorno (`os.Getenv`) | 20 min |
| Demo y presentación final | 40 min |

**Conceptos clave:** `testing`, `httptest`, tablas de prueba, documentación

---

## 🛠️ Tecnologías

| Tecnología | Uso |
|------------|-----|
| Go 1.21+ | Lenguaje principal |
| `net/http` | Servidor HTTP (sin frameworks externos) |
| SQLite + `go-sqlite3` | Base de datos embebida |
| `golang-jwt` | Autenticación con tokens |
| `bcrypt` | Hash seguro de contraseñas |

---

## 🎯 Conceptos de Go aprendidos

- [x] Structs, interfaces y métodos
- [x] Manejo de errores (`error` como valor)
- [x] Punteros
- [x] Goroutines (servidor HTTP usa una por petición)
- [x] Context (`context.WithValue`)
- [x] Packages y módulos
- [x] JSON marshaling/unmarshaling
- [x] Middleware pattern
- [x] Pruebas unitarias

---

## 🔮 Ideas para extender el proyecto

- [ ] Filtrar tareas por prioridad o estado (`?priority=alta&completed=false`)
- [ ] Paginación (`?page=1&limit=10`)
- [ ] Categorías / etiquetas para las tareas
- [ ] Notificaciones por fecha de vencimiento (goroutines + time.Ticker)
- [ ] Frontend en HTML/JS que consuma la API
- [ ] Docker + docker-compose
- [ ] Deploy en Railway o Render
