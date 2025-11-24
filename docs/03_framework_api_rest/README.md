# Unidad 3: Frameworks para Construcción de APIs de Datos en Python

## 1. Introducción

En esta unidad veremos los principales **frameworks para crear APIs de datos en Python**, sus características, ventajas y desventajas. Finalmente, nos centraremos en **FastAPI**, que será el framework elegido para desarrollar una API de datos sencilla.

Las APIs permiten exponer datos y funcionalidades a través de endpoints accesibles mediante HTTP, facilitando la comunicación entre aplicaciones.

---

## 2. ¿Qué es una API REST?

Una **API REST** (Representational State Transfer) sigue un conjunto de principios arquitectónicos que permiten comunicar sistemas usando peticiones HTTP estándar como **GET, POST, PUT, DELETE**.

Principios básicos:

- **Cliente-servidor**
- **Sin estado (stateless)**
- **Recursos identificados por URLs**
- **Uso común de JSON**

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

### Definir un modelo

```python
from pydantic import BaseModel

class Producto(BaseModel):
    id: int
    nombre: str
```

### Crear nuevos productos

```python
@app.post("/productos")
def crear_producto(producto: Producto):
    data.append(producto.dict())
    return producto
```

---

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

## 9. Próximo paso

En clase construiremos una API completa con FastAPI, conectada a un dataset real y con operaciones CRUD completas.

Estos apuntes sirven como referencia para preparar esa práctica.
