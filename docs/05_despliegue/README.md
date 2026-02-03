# Unidad 5 · Despliegue de aplicaciones Python con Docker

## 1. Introducción

Hasta ahora hemos desarrollado aplicaciones Python que se ejecutan en local, normalmente usando **SQLite** como base de datos.  
Este enfoque es válido para aprender, pero no representa un entorno real de producción.

En esta unidad aprenderemos a:

- Desplegar una aplicación Python de forma reproducible
- Separar la aplicación de la base de datos
- Usar contenedores Docker
- Utilizar una base de datos relacional real (PostgreSQL)

---

## 2. Problemas del despliegue tradicional

El despliegue tradicional presenta varios problemas:

- Diferencias entre sistemas operativos
- Versiones distintas de Python y librerías
- Dependencias mal instaladas
- El clásico problema de _“en mi ordenador funciona”_

Docker surge para resolver estos problemas.

---

## 3. ¿Qué es Docker?

Docker es una plataforma que permite empaquetar aplicaciones y sus dependencias en **contenedores**.

Un contenedor incluye:

- La aplicación
- Las librerías necesarias
- La configuración

Todo ello se ejecuta de forma aislada y consistente en cualquier sistema.

---

## 4. Conceptos básicos de Docker

### 4.1 Imagen

Una **imagen** es una plantilla inmutable que define cómo será un contenedor.

### 4.2 Contenedor

Un **contenedor** es una instancia en ejecución de una imagen.

### 4.3 Dockerfile

Archivo de texto que define cómo se construye una imagen.

### 4.4 Docker Hub

Repositorio público de imágenes (por ejemplo, Python o PostgreSQL).

---

## 5. SQLite vs PostgreSQL

### 5.1 SQLite

- Base de datos basada en un archivo
- No es un servicio
- Muy sencilla de usar
- Ideal para aprendizaje y prototipos

### 5.2 PostgreSQL

- Base de datos relacional cliente-servidor
- Pensada para múltiples usuarios
- Soporta concurrencia real
- Usada en entornos profesionales

Ambas son **bases de datos relacionales**, pero **no están pensadas para el mismo contexto**.

---

## 6. Contenerizar una aplicación Python

### 6.1 Estructura típica del proyecto

```
.
├── main.py
├── requirements.txt
├── Dockerfile
```

---

### 6.2 Dockerfile básico para Python

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "main.py"]
```

---

### 6.3 Construcción de la imagen

```bash
docker build -t app-python .
```

---

### 6.4 Ejecución del contenedor

```bash
docker run -p 8000:8000 app-python
```

---

## 7. Bases de datos en contenedores

En un sistema real:

- La aplicación es un servicio
- La base de datos es otro servicio distinto

Ejemplo de PostgreSQL en Docker:

```bash
docker run -d \\
  --name postgres-db \\
  -e POSTGRES_USER=app \\
  -e POSTGRES_PASSWORD=app123 \\
  -e POSTGRES_DB=appdb \\
  -p 5432:5432 \\
  postgres
```

---

## 8. Docker Compose

### 8.1 ¿Qué es Docker Compose?

Docker Compose permite definir y ejecutar **aplicaciones con varios contenedores** usando un solo archivo YAML.

---

### 8.2 Ejemplo de docker-compose.yml

```yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      DATABASE_URL: postgresql://app:app123@db:5432/appdb

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app123
      POSTGRES_DB: appdb
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## 9. Variables de entorno

Las credenciales **no deben escribirse en el código**.

Ejemplo:

```
DATABASE_URL=postgresql://app:app123@db:5432/appdb
```

---

## 10. Conexión desde Python (SQLAlchemy)

```python
import os

DATABASE_URL = os.getenv("DATABASE_URL")
```

Cuando se usa Docker Compose:

- El host no es localhost
- El host es el nombre del servicio (db)

---

## 11. Persistencia de datos

Los contenedores son efímeros:  
si se eliminan, se pierden los datos.

Para evitarlo se usan **volúmenes Docker**, que permiten mantener los datos aunque el contenedor se destruya.

---

## 12. Buenas prácticas

- No subir credenciales al repositorio
- Usar archivos .env
- No incluir **pycache**
- Separar desarrollo y producción
- Mantener imágenes ligeras

---

## 13. Conclusión

Docker permite desplegar aplicaciones reales y reproducibles.  
SQLite es ideal para aprender, pero PostgreSQL es la opción adecuada para producción.
