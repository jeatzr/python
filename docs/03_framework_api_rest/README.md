# Unidad 3: Frameworks para Construcción de APIs de Datos en Python

## 1. Introducción

En esta unidad veremos los principales **frameworks para crear APIs de datos en Python**, sus características, ventajas y desventajas. Finalmente, nos centraremos en **FastAPI**, que será el framework elegido para desarrollar una API de datos sencilla.

Las APIs permiten exponer datos y funcionalidades a través de endpoints accesibles mediante HTTP, facilitando la comunicación entre aplicaciones.

---

## 2. ¿Qué es una API REST?

Una **API REST** (Representational State Transfer) sigue un conjunto de principios arquitectónicos que permiten comunicar sistemas usando peticiones HTTP estándar como **GET, POST, PUT, DELETE**.

Principios básicos:

### 2.1 **Cliente-servidor**

El principio cliente-servidor establece una separación clara entre:

- **Cliente**: navegador, app móvil, frontend, script…
- **Servidor**: la API que expone los datos y la lógica.

Esta separación permite:

- Que el cliente no conozca la implementación interna.
- Que el servidor no se preocupe por cómo se muestran los datos.
- Que múltiples clientes diferentes utilicen la misma API.

Ejemplo: un frontend React consume `GET /productos` y muestra los datos como quiera; el servidor solo envía JSON.

### 2.2 **Sin estado (stateless)**

En REST, cada petición HTTP es **totalmente independiente**.  
El servidor **no mantiene información de sesión** entre peticiones.

Esto significa que:

- Cada petición debe incluir toda la información necesaria.
- Las credenciales o tokens deben enviarse **en cada petición**.
- El servidor no “recuerda” nada entre peticiones.

Esto mejora la escalabilidad, la tolerancia a fallos y la simplicidad del servidor.

Ejemplo de dos peticiones independientes:

```
Request 1:
GET /perfil
Authorization: Bearer abc123

Request 2:
GET /config
Authorization: Bearer abc123
```

Cada una contiene todo lo necesario, sin estado almacenado en el servidor.

### 2.3 **Recursos identificados por URLs**

En REST, todo recurso de la aplicación debe tener una URL única:

- `/users`
- `/users/15`
- `/products`
- `/products/247/reviews`

Las URLs representan **qué** recurso quieres.  
El **cómo** se indica mediante el método HTTP:

- `GET /users` — obtener usuarios
- `POST /users` — crear usuario
- `DELETE /users/7` — borrar usuario

Regla básica:

- **URLs = sustantivos**, nunca verbos.
  - ❌ `/getUsers`
  - ✔ `/users`

### 2.4 **Uso común de JSON**

Aunque REST no obliga al uso de JSON, hoy en día es el formato estándar porque:

- Es ligero y fácil de leer.
- Se integra muy bien con JavaScript.
- Está soportado ampliamente por todos los lenguajes.
- Es muy cómodo de usar con Python y FastAPI.

Ejemplo de JSON típico:

```json
{
  "id": 12,
  "nombre": "Smart TV",
  "precio": 599.99,
  "stock": true
}
```

FastAPI valida automáticamente datos JSON y genera documentación OpenAPI basada en ellos.

---

## 3. Frameworks en Python para crear APIs

### 3.1 Flask

Framework minimalista, flexible y muy extendido.

**Ventajas:**

- Simple de aprender.
- Muy extensible.

**Desventajas:**

- No ofrece validación ni documentación automática por defecto.

**Ejemplo mínimo:**

```python
from flask import Flask
app = Flask(__name__)

@app.route("/hello")
def hello():
    return {"msg": "Hola desde Flask"}
```

---

### 3.2 Django REST Framework (DRF)

Extensión de Django para APIs robustas.

**Ventajas:**

- Sistema completo de autenticación, permisos y serialización.
- Ideal para proyectos grandes.

**Desventajas:**

- Requiere aprender Django.
- Más pesado que otros frameworks.

**Ejemplo mínimo:**

```python
from rest_framework.views import APIView
from rest_framework.response import Response

class Hello(APIView):
    def get(self, request):
        return Response({"msg": "Hola desde DRF"})
```

---

### 3.3 FastAPI

Framework moderno, rápido y basado en tipado.

**Ventajas:**

- Tipado automático con Pydantic.
- Documentación automática (Swagger y ReDoc).
- Muy rápido (ASGI: Starlette + Uvicorn).
- Ideal para APIs de datos.

**Desventajas:**

- Menos maduro que Django.
- Para proyectos enormes se necesita arquitectura adicional.

**Ejemplo mínimo:**

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/hello")
def hello():
    return {"msg": "Hola desde FastAPI"}
```

---

### 3.4 Otros frameworks

- **Falcon:** muy rápido, minimalista.
- **Bottle:** parecido a Flask.
- **Tornado:** orientado a websockets.

---

## 4. ¿Por qué vamos a usar FastAPI?

Elegimos FastAPI porque:

- Produce APIs limpias y modernas.
- Tiene tipado estricto.
- Genera documentación automáticamente.
- Es fácil de desplegar.
- Muy útil en proyectos de datos.

---

## 5. Construyendo una API con FastAPI

### 5.1 Instalación

Instalación recomendada:

```bash
pip install "fastapi[standard]"
```

Esto instala FastAPI + Uvicorn + dependencias recomendadas.

---

### 5.2 Estructura básica del proyecto

```
project/
│-- main.py
│-- requirements.txt
```

---

### 5.3 API mínima

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"mensaje": "Bienvenido a la API"}
```

---

### 5.4 Ejecutar la API (versión moderna)

FastAPI ahora incluye un comando propio para ejecutar la aplicación:

```bash
fastapi run main.py --reload
```

Este comando es un _wrapper_ del servidor ASGI Uvicorn.

También puedes ejecutar Uvicorn directamente:

```bash
uvicorn main:app --reload
```

Ambos son equivalentes.  
`fastapi run` es el recomendado por simplicidad.

---

### 5.5 Endpoints de ejemplo

#### Listado de datos

```python
data = [
    {"id": 1, "nombre": "Producto A"},
    {"id": 2, "nombre": "Producto B"}
]

@app.get("/productos")
def listar_productos():
    return data
```

#### Obtener un elemento por ID

```python
@app.get("/productos/{item_id}")
def obtener_producto(item_id: int):
    for p in data:
        if p["id"] == item_id:
            return p
    return {"error": "Producto no encontrado"}
```

---

## 6. Validación de datos con Pydantic

Cuando trabajamos con APIs, los datos que llegan desde clientes externos **no son fiables**:  
pueden venir incompletos, con tipos incorrectos o con valores que no cumplen las reglas del negocio.

FastAPI utiliza **Pydantic** para resolver este problema de forma automática.

---

### ¿Por qué son necesarios los modelos Pydantic?

Los modelos basados en `BaseModel` permiten:

### 6.1 **Validación automática de datos**

Cada vez que un cliente envía JSON al servidor, Pydantic comprueba:

- tipos (`str`, `int`, `float`, `bool`, `list`, `dict`, etc.)
- campos obligatorios
- valores por defecto
- estructuras más complejas (listas, objetos anidados)
- restricciones (mínimos, máximos, longitudes…)

Si algo no cumple, FastAPI devuelve automáticamente un error 422 con detalles claros.

Esto evita escribir validaciones manuales repetitivas.

---

### 6.2 **Conversión de tipos (“parsing”)**

Pydantic convierte automáticamente los valores al tipo esperado.

Ejemplos:

- `"10"` → `int(10)`
- `"true"` → `bool(True)`
- `"2024-05-10"` → `datetime.date`
- `"12.5"` → `float(12.5)`

Esto hace que tu lógica interna siempre reciba **tipos correctos**.

---

### 6.3 **Modelado de estructuras complejas**

Cuando los datos contienen objetos dentro de objetos, listas, relaciones…  
se necesita una estructura clara y tipada.

Por ejemplo:

```python
from pydantic import BaseModel

class Direccion(BaseModel):
calle: str
ciudad: str

class Usuario(BaseModel):
nombre: str
edad: int
direccion: Direccion
```

FastAPI entiende este modelo, valida la anidación y genera documentación automática.

---

### 6.4 **Documentación automática (OpenAPI / Swagger)**

FastAPI utiliza los modelos Pydantic para generar:

- la documentación interactiva (`/docs`)
- los esquemas OpenAPI
- los ejemplos de entrada y salida

Esto permite que cualquier cliente entienda exactamente qué datos debe enviar.

---

### 6.5 **Consistencia en datos de entrada y salida**

Los modelos pueden usarse tanto para recibir datos (request) como para devolverlos (response):

- Entrada → valida lo que envía el cliente
- Salida → define claramente qué devuelve tu API

Así evitas inconsistencias en tu API.

---

### Ejemplo básico de modelo Pydantic

```python
from pydantic import BaseModel

class Producto(BaseModel):
id: int
nombre: str
```

Este modelo indica:

- `id` debe ser un `int`
- `nombre` debe ser un `str`
- Si falta un campo o el tipo es incorrecto, la API lo rechaza automáticamente

---

### En resumen

Usar modelos Pydantic es necesario porque:

- **validan automáticamente** la información de entrada
- garantizan que tu código recibe **datos correctos y estructurados**
- permiten trabajar con **objetos Python tipados**
- facilitan la **documentación automática**
- hacen que las APIs sean **predecibles y confiables**

## Son la base de la robustez de FastAPI.

## 7. Documentación automática

FastAPI genera automáticamente:

- Swagger UI → `/docs`
- ReDoc → `/redoc`

Permiten probar la API sin escribir código adicional.

---

## 8. Consideraciones modernas sobre servidores ASGI

### Uvicorn como servidor por defecto

Aunque ahora exista `fastapi run`, FastAPI sigue usando Uvicorn internamente.

### Modo multiproceso sin Gunicorn

Actualmente puedes usar workers directamente:

```bash
fastapi run main.py --workers 4
```

O con Uvicorn:

```bash
uvicorn main:app --workers 4
```

Gunicorn **ya no es necesario** en la mayoría de despliegues.

---

---

---

## 9. Organización de la API mediante rutas (routes)

A medida que una API crece, concentrar todo el código en un único archivo (`main.py`) deja de ser una buena práctica.  
FastAPI permite organizar la aplicación en **módulos de rutas** mediante el uso de `APIRouter`.

Esto permite:

- Separar funcionalidades
- Mantener el código ordenado
- Escalar la aplicación con facilidad
- Integrar nuevas capas (como la base de datos) sin mezclar responsabilidades

---

### 9.1 ¿Qué es un router en FastAPI?

Un **router** es un conjunto de endpoints relacionados entre sí.  
Por ejemplo:

- `/productos`
- `/usuarios`
- `/pedidos`

Cada grupo de endpoints puede definirse en un archivo distinto.

---

### 9.2 Creación de un router básico

Ejemplo de router para gestionar productos:

```python
from fastapi import APIRouter

router = APIRouter(
    prefix="/productos",
    tags=["productos"]
)

@router.get("/")
def listar_productos():
    return {"mensaje": "Listado de productos"}
```

- `prefix` añade una ruta base común
- `tags` agrupa los endpoints en la documentación automática

---

### 9.3 Integración del router en la aplicación principal

En el archivo principal de la aplicación:

```python
from fastapi import FastAPI
from routes import productos

app = FastAPI()

app.include_router(productos.router)
```

A partir de este momento, la ruta `/productos/` queda disponible.

---

### 9.4 Estructura recomendada del proyecto

Una estructura típica de proyecto podría ser:

```
app/
├── main.py
├── routes/
│   └── productos.py
├── models/
├── schemas/
└── database/
```

Esta organización facilita el mantenimiento y la escalabilidad del proyecto.

---

### 9.5 Ventajas de usar routers

- Separación clara de responsabilidades
- Código más limpio y legible
- Facilita el trabajo en equipo
- Integración natural con bases de datos y servicios externos

---

## 10. Persistencia de datos y uso de ORM

Hasta ahora, la API ha trabajado con datos almacenados en memoria, lo que implica que los datos se pierden cada vez que se reinicia la aplicación.

Para solucionar este problema se introduce:

- Una **base de datos real**
- Un **ORM** para interactuar con ella desde Python

---

### 10.1 ¿Qué es un ORM?

Un **ORM (Object Relational Mapper)** es una herramienta que permite trabajar con una base de datos relacional usando **clases y objetos de Python**, evitando escribir SQL directamente.

Relación entre conceptos:

| Base de datos | ORM      |
| ------------- | -------- |
| Tabla         | Clase    |
| Columna       | Atributo |
| Fila          | Objeto   |

El ORM se encarga de traducir automáticamente las operaciones en Python a sentencias SQL.

---

### 10.2 Ventajas de usar un ORM en una API

- Reduce la cantidad de SQL manual
- Evita errores comunes
- Mejora la mantenibilidad del código
- Permite cambiar de base de datos con pocos cambios
- Se integra perfectamente con FastAPI y Pydantic

---

### 10.3 SQLite como base de datos

Para esta unidad se utiliza **SQLite**, ya que:

- No requiere servidor
- Usa un único archivo `.db`
- Es ligera y fácil de configurar
- Ideal para prácticas y proyectos educativos

---

### 10.4 SQLAlchemy como ORM

**SQLAlchemy** es uno de los ORM más utilizados en Python.  
Permite definir tablas como clases y realizar operaciones CRUD de forma sencilla.

---

### 10.5 Instalación de SQLAlchemy

```bash
pip install sqlalchemy
```

---

## 11. Configuración de la base de datos con SQLAlchemy

---

### 11.1 Conexión a la base de datos

Para conectarse a una base de datos con SQLAlchemy es necesario definir un **motor de conexión (`engine`)**.  
El motor indica **qué base de datos se va a usar** y **cómo conectarse a ella**.

---

#### Conexión a SQLite (motor incluido en Python)

En el caso de **SQLite**, no es necesario instalar ningún motor adicional, ya que:

- SQLite viene incluido por defecto en Python mediante el módulo estándar `sqlite3`
- SQLAlchemy utiliza internamente ese módulo
- No hay servidor de base de datos

Ejemplo de conexión a SQLite:

```python
from sqlalchemy import create_engine

DATABASE_URL = "sqlite:///./database.db"

engine = create_engine(
DATABASE_URL,
connect_args={"check_same_thread": False}
)
```

Notas importantes:

- El archivo `database.db` se crea automáticamente si no existe
- `check_same_thread=False` es necesario al usar SQLite con FastAPI
- No se requiere ninguna instalación adicional aparte de SQLAlchemy

---

#### Conexión a MySQL (ejemplo teórico)

En bases de datos como **MySQL**, el proceso es distinto:

- Es necesario instalar un **servidor de base de datos**
- Se debe instalar un **driver de conexión**
- La URL de conexión cambia

Ejemplo de instalación del driver:

```bash
pip install pymysql
```

Ejemplo de conexión a MySQL con SQLAlchemy:

```python
from sqlalchemy import create_engine

DATABASE_URL = "mysql+pymysql://usuario:password@localhost:3306/nombre_bd"

engine = create_engine(DATABASE_URL)
```

Donde:

- `usuario` es el usuario de la base de datos
- `password` es la contraseña
- `localhost` es el servidor
- `3306` es el puerto por defecto de MySQL
- `nombre_bd` es la base de datos

---

#### Comparación SQLite vs MySQL

| Característica | SQLite       | MySQL          |
| -------------- | ------------ | -------------- |
| Instalación    | No necesaria | Sí             |
| Servidor       | No           | Sí             |
| Archivo único  | Sí           | No             |
| Uso educativo  | Ideal        | No recomendado |
| Uso producción | Limitado     | Habitual       |

En esta unidad se utilizará **SQLite**, pero el uso de un ORM permite cambiar de motor con mínimos cambios en el código.

---

### 11.2 Base declarativa

SQLAlchemy utiliza una clase base común para todos los modelos ORM:

```python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

---

### 11.3 Definición de modelos ORM

Ejemplo de modelo `Producto`:

```python
from sqlalchemy import Column, Integer, String, Float, Boolean

class Producto(Base):
    __tablename__ = "productos"

    id = Column(Integer, primary_key=True, index=True)
    nombre = Column(String, nullable=False)
    precio = Column(Float, nullable=False)
    stock = Column(Integer, nullable=False)
    disponible = Column(Boolean, default=True)
```

Cada instancia de esta clase representa una fila de la tabla.

---

### 11.4 Creación de las tablas

```python
Base.metadata.create_all(bind=engine)
```

---

## 12. Sesiones y operaciones CRUD con SQLAlchemy

Las **sesiones** permiten comunicarse con la base de datos y ejecutar operaciones de forma segura.

---

### 12.1 Configuración de la sesión

```python
from sqlalchemy.orm import sessionmaker
from database.database import engine

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)
```

- `autocommit=False` → obliga a confirmar cambios con `commit()`
- `autoflush=False` → evita escribir automáticamente cambios pendientes
- `bind=engine` → asocia la sesión con la base de datos configurada

---

### 12.2 Inyección de dependencias con FastAPI

Para usar la sesión en los endpoints de FastAPI de manera segura y automática se define una función **dependencia**:

```python
from fastapi import Depends
from sqlalchemy.orm import Session

def get_db():
    db = SessionLocal()
    try:
        yield db   # devuelve la sesión al endpoint
    finally:
        db.close() # asegura que la sesión se cierra al terminar
```

#### Explicación paso a paso

1. Cada vez que un endpoint que usa `Depends(get_db)` es llamado, FastAPI ejecuta `get_db()`.
2. `db = SessionLocal()` → crea una nueva sesión para esa petición.
3. `yield db` → la sesión se pasa **como argumento** al endpoint.
4. Cuando el endpoint termina (o si ocurre un error), se ejecuta el bloque `finally` → la sesión se cierra automáticamente.
5. Esto asegura que **cada petición tiene su propia sesión** y no quedan conexiones abiertas.

---

### 12.3 Uso de la sesión en un endpoint

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/", response_model=list[UserResponse])
def get_users(db: Session = Depends(get_db)):
    return db.query(User).all()
```

Flujo de ejecución:

1. La petición HTTP llega al endpoint `/users/`.
2. FastAPI llama a `get_db()` y obtiene `db` (sesión).
3. `db.query(User).all()` consulta todos los usuarios en la base de datos.
4. FastAPI convierte los objetos ORM `User` a Pydantic `UserResponse`.
5. Se devuelve la respuesta al cliente.
6. Se ejecuta `finally: db.close()` y se cierra la sesión.

---

### 12.4 Diagrama conceptual

```
[Petición HTTP]
       |
       v
[FastAPI Endpoint] ---Depends(get_db)--> [get_db() crea sesión]
                                            |
                                            v
                                  [db.query(...)]
                                            |
                                            v
                           [Resultado devuelto al endpoint]
                                            |
                                            v
                                [finally: db.close()]
```

---

### 12.5 Ventajas de este enfoque

- No hay que abrir/cerrar la sesión manualmente en cada endpoint.
- Funciona de manera segura con múltiples endpoints concurrentes.
- Evita errores por conexiones abiertas o sesiones compartidas entre peticiones.
- Compatible con **routers** y modularización del proyecto.

---

### 12.6 Operaciones CRUD básicas

#### Crear un registro

```python
producto = Producto(
    nombre="Ratón",
    precio=19.99,
    stock=25,
    disponible=True
)

db.add(producto)
db.commit()
db.refresh(producto)
```

---

#### Leer registros

```python
productos = db.query(Producto).all()
```

Obtener un registro por ID:

```python
producto = db.query(Producto).filter(Producto.id == 1).first()
```

---

#### Actualizar un registro

```python
producto.precio = 17.99
db.commit()
```

---

#### Eliminar un registro

```python
db.delete(producto)
db.commit()
```

---

### 12.7 Relación entre Pydantic y SQLAlchemy

En una API con FastAPI se utilizan dos tipos de modelos:

- **Modelos Pydantic**  
  Validan y estructuran los datos que entran y salen de la API.

- **Modelos SQLAlchemy (ORM)**  
  Definen cómo se almacenan los datos en la base de datos.

Esta separación de responsabilidades mejora la claridad, la seguridad y el mantenimiento del proyecto.

#### Conclusión

La combinación de **FastAPI + routers + Pydantic + SQLAlchemy + SQLite** permite crear APIs REST bien estructuradas, con persistencia real y listas para ser consumidas por aplicaciones frontend.

---

## 13. Acceso a la API desde frontend y problema de CORS

Cuando una API desarrollada con FastAPI se consume desde una aplicación frontend (por ejemplo, **React**), es habitual encontrarse con el llamado **error de CORS**.

---

### 13.1 ¿Qué es CORS?

**CORS** (_Cross-Origin Resource Sharing_) es un mecanismo de seguridad implementado por los **navegadores web**.

Un navegador considera que una petición es **cross-origin** cuando:

- El frontend y la API no comparten:
  - el mismo dominio
  - o el mismo puerto
  - o el mismo protocolo

Ejemplo típico en desarrollo:

- Frontend React:  
  `http://localhost:5173`
- API FastAPI:  
  `http://localhost:8000`

Aunque ambos estén en `localhost`, **el puerto es distinto**, por lo que el navegador bloquea la petición si la API no lo permite explícitamente.

---

### 13.2 Por qué aparece el error de CORS

El error **no lo genera FastAPI**, sino el navegador.

El navegador:

1. Detecta que la petición va a otro origen
2. Comprueba si la respuesta incluye cabeceras CORS válidas
3. Si no están presentes, bloquea la respuesta por seguridad

Esto evita, por ejemplo, que una web maliciosa acceda a datos privados de otra web sin permiso.

---

### 13.3 Solución: Middleware CORS en FastAPI

FastAPI permite configurar CORS de forma sencilla mediante un **middleware**.

- Podemos ver en la documentación de FastApi la [manera de solucionar dicho problema de CORS](https://fastapi.tiangolo.com/tutorial/cors/).

Este middleware añade las cabeceras necesarias a las respuestas de la API para indicar qué orígenes están permitidos.

---

### 13.4 Configuración básica de CORS

En el archivo principal (`main.py`) se debe añadir lo siguiente:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
CORSMiddleware,
allow_origins=[
"http://localhost:5173", # React (Vite)
"http://localhost:3000" # React (Create React App)
],
allow_credentials=True,
allow_methods=["*"],
allow_headers=["*"],
)
```

---

### 13.5 Explicación de los parámetros

- **allow_origins**  
  Lista de orígenes permitidos para acceder a la API.  
  Es recomendable especificarlos explícitamente.

- **allow_credentials**  
  Permite enviar cookies o credenciales si fuera necesario.

- **allow_methods**  
  Métodos HTTP permitidos (`GET`, `POST`, `PUT`, `DELETE`, etc.).  
  Con `"*"` se permiten todos.

- **allow_headers**  
  Cabeceras HTTP permitidas.  
  Con `"*"` se permiten todas.

---

### 13.6 Configuración en desarrollo vs producción

Durante el desarrollo se puede permitir cualquier origen:

```python
allow_origins=["*"]
```

⚠️ **No recomendado en producción**, ya que elimina las restricciones de seguridad.

En producción:

- Se deben especificar los dominios reales del frontend
- Nunca usar `"*"` si hay datos sensibles

---

### 13.7 Resumen

- CORS es una **protección del navegador**, no un error de FastAPI
- Aparece cuando frontend y backend están en orígenes distintos
- Se soluciona añadiendo el **middleware CORS**
- FastAPI incluye soporte CORS de forma nativa y sencilla

---
