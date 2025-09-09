# 🐍 Unidad 1: Fundamentos de Python

## 1. **¿Qué es Python?**

Python es un lenguaje de programación de alto nivel, interpretado, multiparadigma y de propósito general.

### 🧠 Características principales:

- Sintaxis clara y legible
- Tipado dinámico
- Gran comunidad y abundante documentación
- Amplia librería estándar
- Muy usado en ciencia de datos, desarrollo web, automatización, inteligencia artificial, entre otros.

### 🏛 Origen:

- Creado por **Guido van Rossum** y lanzado en 1991.
- Su nombre no viene del animal, sino del grupo de comedia británico _Monty Python_.

### 🔧 Aplicaciones comunes:

- Desarrollo web (Django, Flask)
- Ciencia de datos (NumPy, Pandas, Matplotlib)
- Inteligencia Artificial y Machine Learning (TensorFlow, scikit-learn)
- Automatización de tareas (scripts)
- Videojuegos, escritorio, ciberseguridad, IoT, etc.

![Aplicaciones python](img/aplicaciones_python.jpg)

---

## 2. **Instalación de Python**

### Descarga e instalación

- Descargar desde: [https://www.python.org/downloads](https://www.python.org/downloads)
- Asegúrate de marcar la opción **"Add Python to PATH"** en Windows.

### Verificar instalación

```bash
python --version
# o
python3 --version
```

---

## 3. **Estructura de un programa en Python**

- Un programa en Python está compuesto por instrucciones secuenciales.
- No requiere una función `main()` obligatoria, aunque es recomendable usarla en proyectos grandes.
- La indentación es clave: delimita bloques de código.

```python
def saludar():
    print("Hola, mundo")

saludar()
```

---

## 4. **Sintaxis básica y convenciones del lenguaje**

- Python es sensible a la **indentación** (usa 4 espacios por convención).
- Las instrucciones se escriben línea por línea.
- Uso de snake_case para variables y funciones.
- Uso de CamelCase para clases.

```python
nombre_usuario = "Juan"
EdadUsuario = 30  # ❌ No recomendado
```

---

## 5. **Uso de comentarios y documentación en el código**

- Comentarios de una línea: `#`
- Comentarios multilínea: triple comilla `'''` o `"""` (aunque se usan más para docstrings).
- Docstrings: documentación de funciones, módulos o clases.

```python
# Esto es un comentario de una línea

"""
Esto es un comentario
de varias líneas (también usado para documentación)
"""

def sumar(a, b):
    """Devuelve la suma de dos números"""
    return a + b
```

---

## 6. **Tipos de datos y variables**

### Tipos de datos básicos

- Números: `int`, `float`, `complex`
- Texto: `str`
- Booleanos: `bool` (`True`, `False`)

```python
edad = 25
precio = 19.99
nombre = "Python"
activo = True
```

### Colecciones

- Listas: `[]` (modificables)
- Tuplas: `()` (inmutables)
- Diccionarios: `{clave: valor}`
- Conjuntos: `{elemento1, elemento2}`

```python
lista = [1, 2, 3]
tupla = (1, 2, 3)
diccionario = {"nombre": "Ana", "edad": 30}
conjunto = {1, 2, 3}
```

### Conversión de tipos

```python
int("10")       # 10
str(20)         # "20"
float("3.14")   # 3.14
```

---

## 7. **Estructuras de control**

### Condicionales

```python
if edad > 18:
    print("Eres mayor de edad")
elif edad == 18:
    print("Tienes justo 18")
else:
    print("Eres menor de edad")
```

### Bucles

```python
# Bucle for
for i in range(5):
    print(i)

# Bucle while
contador = 0
while contador < 5:
    print(contador)
    contador += 1
```

### Control de flujo

```python
for i in range(10):
    if i == 5:
        break  # sale del bucle
    if i % 2 == 0:
        continue  # salta al siguiente ciclo
    print(i)

pass  # se usa como marcador de posición
```

---

## 8. **Funciones y módulos**

### Funciones

```python
def saludar(nombre):
    return f"Hola, {nombre}"

print(saludar("Lucía"))
```

### Módulos

- Importar un módulo estándar:

```python
import math
print(math.sqrt(16))
```

- Importar una función específica:

```python
from math import sqrt
print(sqrt(25))
```

---

## 9. **Entornos de desarrollo**

### Entornos virtuales con `venv`

```bash
python -m venv mi_entorno
source mi_entorno/bin/activate  # Linux/macOS
mi_entorno\Scripts\activate     # Windows
```

### Instalación de paquetes con `pip`

```bash
pip install nombre_del_paquete
```

Ejemplo:

```bash
pip install requests
```
