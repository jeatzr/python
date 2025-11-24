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

- Un programa en Python está compuesto por instrucciones secuenciales. Cada instrucción está definica en una línea lógica
- El final de una línea lógica está representado por el caracter especial NEWLINE (salto de línea).
- Unión de líneas explícita: dos o más líneas físicas pueden unirse en una única línea lógica utilizando el caracter de barra invertida **(\)**. Una línea que termina en barra invertida no puede llevar un comentario.

```python
if 1900 < year < 2100 and 1 <= month <= 12 \
   and 1 <= day <= 31 and 0 <= hour < 24 \
   and 0 <= minute < 60 and 0 <= second < 60:   # Parece una fecha válida
        return 1
```

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
- Uso de **snake_case** para variables y funciones.
- Uso de **PascalCase** para nombres de clases.

```python
nombre_usuario = "Juan"
EdadUsuario = 30  # ❌ No recomendado

class Persona:
  species = "Homo Sapiens"

p = Persona()
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
de varias líneas (también usado para documentación o docstrings)
"""

def sumar(a, b):
    """Devuelve la suma de dos números (esto es un docstring)"""
    return a + b
```

---

## 6. Comunicación por consola en Python

En Python, la forma más básica de interactuar con el usuario en la consola es mediante las funciones `print()` e `input()`.

### Salida de datos `print()`

La usaremos para mostrar datos por la consola.

```py
print("Hola, mundo")      # Imprime un texto
print(42)                 # Imprime un número
print(3.14, "es pi")      # Imprime varios valores separados por espacio

nombre = "Ana"
edad = 25
print(f"Hola, me llamo {nombre} y tengo {edad} años.") # esto es una f-string.
```

[--> Más información de f-strings en python.org](https://docs.python.org/3.13/reference/lexical_analysis.html#formatted-string-literals)

```py
a = 5
b = 3
print(f"La suma de {a} + {b} = {a+b}")
# → La suma de 5 + 3 = 8

pi = 3.14159265
print(f"Pi con 2 decimales: {pi:.2f}")   # Pi con 2 decimales: 3.14
print(f"Pi con 6 decimales: {pi:.6f}")   # Pi con 6 decimales: 3.141593
```

#### Entrada de datos con `input()`

La función `input()` permite al usuario introducir datos desde la consola.
Siempre devuelve una cadana de texto (`str`) por lo que a veces será necesario convertirla.

Ejemplos básicos:

```python
nombre = input("¿Cómo te llamas? ")
print("Encantado de conocerte,", nombre)
```

---

## 7. **Tipos de datos y variables**

### Tipos de datos básicos

- Números: `int`, `float`, `complex`
- Texto: `str`
- Booleanos: `bool` (`True`, `False`)

```python
edad = 25
precio = 19.99
nombre = "Python"
activo = True

print(type(edad))
print(type(precio))
print(type(nombre))
print(type(activo))
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

## 8. Tratamiento de errores en python.

En programación es habitual que ocurran errores (por ejemplo, al pedir un dato al usuario o al acceder a un archivo que no existe).
Python permite manejar esos errores de forma controlada con las sentencias `try` y `except`.

#### `try` y `except`. Estructura básica

```py
try:
    # Código que puede producir un error
    ...
except:
    # Código que se ejecuta si ocurre un error
    ...
```

#### Ejemplo: División entre 0:

```py
try:
    x = int(input("Introduce un número: "))
    resultado = 10 / x
    print("El resultado es:", resultado)
except:
    print("Ha ocurrido un error")
```

Si el usuario introduce 0 o algo que no sea número, el bloque except se encarga de mostrar un mensaje en lugar de que el programa se detenga.

#### Manejo de errores específicos

En el ejemplo anterior no distinguimos el error que se ha dado pero podemos ser más específicos

```py
try:
    x = int(input("Introduce un número: "))
    resultado = 10 / x
    print("El resultado es:", resultado)
except ValueError:
    print("⚠️ No has introducido un número válido.")
except ZeroDivisionError:
    print("⚠️ No se puede dividir entre cero.")
```

[--> Excepciones Incorporadas en Python](https://docs.python.org/es/3/library/exceptions.html)

## 9. **Estructuras de control**

### Condicionales

#### Sentencia `if elif else`

```python
if edad > 18:
    print("Eres mayor de edad")
elif edad == 18:
    print("Tienes justo 18")
else:
    print("Eres menor de edad")
```

#### Sentencia `match`

Similar a las sentencias swith/case de otros lenguajes aunque es mucho más potente. Se ejecuta cuando se encuentra una expresión que concuerda con algún patrón de los propuestos. El caso `_` actuaría como comodín.

```py
def describir_dato(dato):
    match dato:
        case 0:
            return "Es un cero."
        case 1 | 2 | 3:
            return "Es un número pequeño (1, 2 o 3)."
        case int() as n if n > 100:
            return f"Es un entero grande: {n}."
        case float():
            return f"Es un número decimal (float)."
        case str() as texto if texto.isupper():
            return f"Es un texto en MAYÚSCULAS: {texto}"
        case str():
            return f"Es una cadena de texto: '{dato}'"
        case [x, y]:  # patrón de lista con dos elementos
            return f"Es una lista de 2 elementos: {x} y {y}"
        case _:
            return "No sé qué es."

# Probar:
print(describir_dato(0))
print(describir_dato(2))
print(describir_dato(150))
print(describir_dato(3.14))
print(describir_dato("HOLA"))
print(describir_dato("hola"))
print(describir_dato([10, 20]))
print(describir_dato({"a": 1}))

```

### Bucles

#### Bucles `for`

`for .. in` nos permite iterar sobre colecciones de cuaquier tipo.

- Para iterar sobre números podemos usar la función [**range**](https://docs.python.org/3.13/tutorial/controlflow.html#the-range-function).
- Podemos iterar sobre una lista de cualquier tipo de elementos.

[For Statement @ python.org](https://docs.python.org/3.13/tutorial/controlflow.html#for-statements)

```python
# Bucle for iterando en un rango numérico
for i in range(5):
    print(i)

lista = ["gato","perro","canario","caimán","oso polar","serpiente pitón"]
# iteramos sobre los elementos de la lista
for pet in lista:
  print(pet)

```

#### Bucle `while`

Ejemplo de un bucle sencillo. La condición es en base a un contador que se va decrementando.

```python
contador = 0
while contador < 5:
    print(contador)
    contador += 1
```

Ejemplo de cómo usar un bucle while infinito junto con un bloque `try except` y un `break` para solicitar un valor hasta que el valor sea correcto.

```py
while True:
    try:
        edad = int(input("Introduce tu edad: "))
        break
    except ValueError:
        print("⚠️ Eso no es un número, inténtalo de nuevo.")

print(f"Tu edad es {edad}")
```

#### Control de flujo

Podemos controlar el flujo de la ejecución con:

- `break`: Sale del bucle.
- `continue`: salta al siguiente ciclo del bucle sin ejecutar lo que hay detrás.

[--> Break and Continue Statements @ python.org](https://docs.python.org/3.13/tutorial/controlflow.html#break-and-continue-statements)

```python
for i in range(10):
    if i == 5:
        break  # sale del bucle
    if i % 2 == 0:
        continue  # salta al siguiente ciclo
    print(i)
```

- La sentencia `else` en un bucle se ejecuta **solo cuando no se ejecuta break**.

Podemos verlo en este ejemplo donde se comprueban números para ver si son primos ([ejemplo tomado de docs.python.org](https://docs.python.org/3.13/tutorial/controlflow.html#else-clauses-on-loops))

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n//x)
            break
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```

#### Sentencia `pass`

Esta sentencia no hace nada, literalmente. Se usa cuando necesitamos meter código para que algo sea sintácticamente correcto pero aún no hemos definido qué hay que hacer.

Ejemplos:

```python
while True:
    pass  # Busy-wait for keyboard interrupt (Ctrl+C)

class MyEmptyClass:
    pass

def initlog(*args):
    pass   # Remember to implement this!
```

---

## 10. **Funciones y módulos**

### Funciones

- Las funciones se definen mediante la palabra reservada **def**.
- La primera sentencia de la función puede ser un **docstring** que nos indique el propósito de la función y el modo de uso. Es recomendable definirlo.
- Opcionalmente la función puede devolver algo con la palabra reservada `return`.

```python
def saludar(nombre):
    """Función muy simple que solo sirve para saludar al nombre que le pases como argumento"""
    return f"Hola, {nombre}"

print(saludar("Lucía"))
```

```python
def fib(n):    # escribir la serie de fibonacci por debajo de n
    """Print a Fibonacci series less than n."""
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

# Llamamos la función
fib(2000)
```

[--> Functions @ python.org](https://docs.python.org/3.13/tutorial/controlflow.html#function-examples)

#### Tipos de parámetros

##### 1. Parámetros posicionales

Se pasan en el orden exacto en que fueron definidos

```py
def presentacion(nombre, edad):
    print(f"{nombre} tiene {edad} años")

presentacion("Ana", 25)
```

##### 2. Por palabra clave (keyword arguments)

Se especifica el nonmbre del parámetro al llamar a la función. No importa el orden.

```py
presentacion(edad=25, nombre="Ana")
```

##### 3. Valores por defecto

Se puede asignar un valor si el parámetro no se proporciona.

```py
def saludar(nombre="Usuario"):
    print(f"¡Hola, {nombre}!")

saludar()       # ¡Hola, Usuario!
saludar("Ana")  # ¡Hola, Ana!
```

##### 4. Parámetros arbitrarios (`*args`)

Permite pasar cualquier número de argumentos posicionales.
Dentro de la función se recibe como una tupla.

```py
def sumar(*numeros):
    total = sum(numeros)
    print(f"La suma es {total}")

sumar(2, 5, 7)
sumar(1, 2, 3, 4, 5)
```

##### 5. Parámetros arbitrarios por palabra clave (`**kwargs`)

Permite pasar cualquier número de argumentos con nombre.
Dentro de la función se recibe como un diccionario.

```py
def mostrar_info(**info):
    for clave, valor in info.items():
        print(f"{clave}: {valor}")

mostrar_info(nombre="Ana", edad=25, ciudad="Madrid")
```

##### 6. Combinación de tipos

Se pueden combinar todos los tipos, pero en orden:

```py
def funcion(posicional, default=1, *args, **kwargs):
    ...
```

Ejemplo:

```py
def ejemplo(a, b=10, *args, **kwargs):
    print(a, b, args, kwargs)

ejemplo(1, 2, 3, 4, x=100, y=200)
# Salida: 1 2 (3, 4) {'x': 100, 'y': 200}
```

### Módulos estándar

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

### Módulos creados por el usuario

```python
# calculos.py
def sumar(a: int, b: int) -> int:
    return a + b

def restar(a: int, b: int) -> int:
    return a - b
```

Importar desde otro archivo (han de estar en el mismo directorio):

```python
# main.py
import calculos
print(calculos.sumar(3, 5))

# o funciones concretas
from calculos import sumar, restar
print(sumar(3, 5))
```

### Subcarpetas como paquetes

Estructura de ejemplo:

```
mi_proyecto/
├── main.py
└── utils/
    ├── __init__.py
    └── helpers.py
```

Importación desde `main.py`:

```python
from utils.helpers import funcion_auxiliar
funcion_auxiliar()
```

- `__init__.py` marca la carpeta como paquete.
- Usa `if __name__ == "__main__":` para evitar ejecutar código de prueba al importar.

---

## 11. Uso de `if __name__ == "__main__":`

```python
if __name__ == "__main__":
    # Código que se ejecuta solo si el archivo se ejecuta directamente
```

- Permite **separar código ejecutable del código importable**.
- Mejora la **reutilización y legibilidad**.

---

## 12. **Entornos de desarrollo**

### Entornos virtuales con `venv`

### ¿Que es?

- Es un directorio aislado que contiene una instalación de Python y sus propias librerías y paquetes. Su principal objetivo es aislar las dependencias entre diferentes proyectos para evitar conflictos entre diferentes versiones de paquetes y asegurar que cada proyecto tenga unicamente las bibliotecas que necesita.

### Trabajar con entornos virtuales en VsCode

#### :one: Pasos Previos

- En primer lugar crearemos el directorio físico donde se alojaran los archivos de nuestro proyecto.
- Luego abrimos ese directorio en el Explorador de VsCode:
  ![Explorador VsCode](img/explorador.jpg)

#### :two: Creación

- Desde la pestaña **Terminal** abrimos un nuevo terminal y ejecutamos el siguiente comando, que creará nuestro entorno virtual:

```bash
python -m venv envMiEntorno
```

#### :three: Activación

- Una vez hemos creado el entorno, debemos activarlo. Para ello escribiremos lo siguiente:

```bash
envMiEntorno\Scripts\activate
```

    - :information_source: Es posible que recibamos un error al ejecutar el comando anterior, ya que Windows por defecto tiene deshabilitada la ejecución de scripts. En ese caso, abriremos PowerShell como Administrador y ejecutamos los siguientes comandos:
        ```powershell
        Get-ExecutionPolicy -List
        Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
        ```

#### :four: Instalación de librerías externas

- Mediante el comando **pip** accedemos al gestor de paquetes estándar de Python, que nos permite instalar y administrar software de terceros fácilmente, para que podamos usarlo en nuestro proyecto. Para ello hace uso del repositorio [Python Package Index (PyPI)](https://pypi.org/). Por ejemplo:

```bash
pip install nombre_del_paquete
```

```bash
pip install mkdocs
pip install mkdocs-material
```

#### :five: Listar dependencias

- Una **dependencia** es cualquier paquete software o librería externa que tu proyecto requiere para funcionar, pero que no está incluido en la instalación básica de Python.
- Por lo tanto, cada vez que instalamos un nuevo paquete, estamos añadiendo dependencias a nuestro proyecto.
- Con el siguiente comando podemos ver cada una de las dependencias instaladas en nuestro entorno virtual, con sus correspondientes versiones:

```bash
pip freeze
```

#### :six: Exportar dependencias

- Por buenas prácticas de programación, los entornos virtuales nunca se suben a los repositorios de _"git"_ o de cualquier otro manejador de versiones.
- Por ello, siempre es muy recomendable crear un archivo llamado **requirements.txt**, en el cual almacenaremos el listado de todas las dependencias que necesita nuestro proyecto:

```bash
pip freeze > requirements.txt
```

- Este archivo SI se sube al repositorio. De modo que, podremos generar nuevamente nuestro entorno en cualquier dispositivo o sistema operativo que deseemos, mediante el siguiente comando:

```bash
pip install -r requirements.txt
```

```bash
python -m venv mi_entorno
source mi_entorno/bin/activate  # Linux/macOS
mi_entorno\Scripts\activate     # Windows
```

[--> Entornos Virtuales @ python.org](https://docs.python.org/es/3/tutorial/venv.html)

## 12. Tipado en Python (Type Hints)

Python es un lenguaje **dinámicamente tipado**, lo que significa que no es obligatorio declarar el tipo de las variables:

```python
nombre = "Ana"   # str
edad = 30        # int
```

Sin embargo, podemos usar **type hints** para indicar tipos de forma opcional:

### Tipado de variables

```python
nombre: str = "Ana"
edad: int = 30
precio: float = 19.99
activo: bool = True
```

### Tipado en funciones

```python
def sumar(a: int, b: int) -> int:
    return a + b

def saludar(nombre: str) -> None:
    print(f"Hola, {nombre}")
```

- `-> int` indica que la función devuelve un entero.
- `-> None` indica que no devuelve nada (solo efecto secundario, como imprimir).

### Tipos más complejos (sintaxis moderna, desde python 3.5)

- Listas:

```python
numeros: list[int] = [1, 2, 3, 4]
nombres: list[str] = ["Ana", "Luis"]
```

- Diccionarios:

```python
edades: dict[str, int] = {"Ana": 30, "Luis": 25}
```

- Tuplas y sets:

```python
coordenadas: tuple[float, float] = (10.5, 20.3)
valores: set[int] = {1, 2, 3}
```

- Tipos combinados:

```python
valor: int | float = 3.14  # equivalente a Union[int, float]
```

Antes de Python 3.9 para declarar estos tipos más complejos había que importar desde la librería

```py
# Antes (Python < 3.9)
from typing import List, Dict

numeros: List[int] = [1, 2, 3]
edades: Dict[str, int] = {"Ana": 30, "Luis": 25}

# Ahora (Python >= 3.9)
numeros: list[int] = [1, 2, 3]
edades: dict[str, int] = {"Ana": 30, "Luis": 25}
```

### Ventajas del tipado

- Mejora **legibilidad** y documentación del código.
- Ayuda a **linters y editores** a detectar errores antes de ejecutar.
- Facilita la comprensión en proyectos grandes o colaborativos.

---

## Ejercicio práctico completo

Crea un programa que:

1. Pida nombre, edad y ciudad.
2. Guarde información en variables con **tipado**.
3. Use condicionales y bucles.
4. Imprima mensajes formateados.
5. Tenga funciones en un **módulo propio** (`funciones.py`) y se importe desde el script principal.
6. Use `if __name__ == "__main__":` en el módulo.
