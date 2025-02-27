# Introduccion-Python
Masterclass introductoria a PYTHON
# Ejercicios taller de introduccion a python
A continuacion se presentan los ejemplos de codigo vistos en clases y algunos extra más para que puedan probarlos.

Codigo de Google Colab visto en vivo en el taller [aqui](https://colab.research.google.com/drive/11twsRG7eSNeOGRaLDnIoazjd5B3LB4LU?usp=sharing)

## Sintaxis básica de python
```python
print("Hola mundo python con google colab 🐍")
primer_numero = 15
segundo_numero = 20
suma = primer_numero + segundo_numero
print(f"La suma es {suma}")

#permite el ingreso por teclado de parte del usuario
nombre = input("Ingrese su nombre: ")
print(f"Hola {nombre}")

numero = 30
# resultado = numero + nombre #error de tipo
resultado = numero * nombre
print(resultado)
```

## Automatización de Tareas con Python

**Descargar imagenes**

Imaginemos que necesitamos descargar imágenes de distintos productos para un catálogo, pero hacerlo manualmente nos tomaría mucho tiempo. Con Python, podemos automatizarlo con unas pocas líneas de código.

```python
import requests

# URL de la imagen
url = "https://upload.wikimedia.org/wikipedia/commons/3/3a/Cat03.jpg"

# Descargar imagen
imagen = requests.get(url).content

# Guardar imagen
with open("gato.jpg", "wb") as archivo:
    archivo.write(imagen)

print("Imagen descargada exitosamente.")
```
```python
import requests

with open("links_imagenes.txt", "r") as file:
    for line in file:
        image_url = line.strip()  # Remove leading/trailing whitespace
        if image_url:  # Check if the line is not empty
            filename = image_url.split('/')[-1]
            imagen = requests.get(image_url).content

            with open(filename, 'wb') as file:
                file.write(imagen)
            print(f"Imagen descargada exitosamente: {filename}")
```

**Convertir texto a audio**
Otra tarea que podemos automatizar es la conversión de texto a voz. Esto es útil en accesibilidad, asistentes virtuales o incluso para generar audiolibros.

gTTS (Google Text-to-Speech) es una librería de Python que permite convertir texto en voz utilizando la API de conversión de texto a voz de Google Translate. Es una herramienta sencilla y eficaz para generar archivos de audio a partir de texto en distintos idiomas.

```sh
!pip install gtts
```
```python
from gtts import gTTS
import IPython.display as display

# Texto de ejemplo
#texto = "Hola, este es un mensaje automatizado con Python."
texto = input('Ingrese el mensaje: ')
#Hello! Welcome to the workshop. Python is an interpreted programming language

# Convertir a audio
tts = gTTS(texto, lang="en")
tts.save("mensaje.mp3")

# Reproducir en Google Colab
display.Audio("mensaje.mp3", autoplay=True)
```

# Manipulación de Archivos CSV/Excel

Vamos a leer un archivo Excel simulado con datos de productos y calcular un promedio de ventas. Esto es útil para análisis de inventarios, estadísticas de negocios o cualquier proceso donde necesitemos analizar datos.

```python
import pandas as pd

# Vamos a leer un archivo excel previamente cargado
df = pd.read_excel('Ventas.xlsx')

# Visualizamos los datos del archivo
print("Datos del archivo Excel:")
print(df)

print("\nCalculando suma de las ventas...")
suma_ventas = df["Ventas"].sum()
print(f"La suma de las ventas es: ${suma_ventas}")

print("\nCalculando promedio de ventas...")
promedio_ventas = df["Ventas"].mean()
print(f"El promedio de ventas es: ${promedio_ventas:.2f}")

```

Tambien se pueden generar graficos

```python
import pandas as pd
import matplotlib.pyplot as plt #libreria para poder crear graficos

# Vamos a leer un archivo excel previamente cargado
df = pd.read_excel('Ventas.xlsx')

# Visualizamos los datos del archivo
print("Datos del archivo Excel:")
print(df)

# Creamos el gráfico de barras
plt.figure(figsize=(8, 6))  # Ajustar el tamaño del gráfico si es necesario
#Crea el gráfico de barras utilizando los valores de las columnas "Producto" (eje x) y "Ventas" (eje y)
plt.bar(df['Producto'], df['Ventas'])
plt.xlabel('Producto')
plt.ylabel('Ventas')
plt.title('Ventas por Producto')
plt.xticks(rotation=45, ha='right')  # Rotar las etiquetas del eje x si es necesario
plt.tight_layout()  # Ajustar el diseño para que las etiquetas no se superpongan
plt.show()
```

# Introducción a Python en el Desarrollo Web

Vamos a crear un pequeño servidor con Flask. Flask nos permite responder a solicitudes HTTP, como si estuviéramos construyendo una API que devuelve información al usuario

```python
from flask import Flask
from flask import jsonify


app = Flask(__name__)

@app.route('/')
def home():
    return '<h1>Respondiendo una cadena HTML</h1>'
    #return jsonify({"nombre": "Python", "version": "3.10", "tipo": "Lenguaje de programación"})

# Ejecutar en Colab
from google.colab import output
output.serve_kernel_port_as_iframe(5000)

# Iniciar el servidor
app.run(port=5000)
```

# Introducción a la Inteligencia Artificial con Python

Vamos a usar un modelo de IA preentrenado para analizar una imagen y detectar qué objetos hay en ella. Esto es similar a cómo funcionan los filtros de redes sociales o los sistemas de cámaras de seguridad.
La librería transformers de Hugging Face es una de las más utilizadas en Inteligencia Artificial y Procesamiento de Lenguaje Natural (NLP). Permite cargar, entrenar y usar modelos de aprendizaje profundo preentrenados para tareas como:

* ✅ Generación de texto (chatbots, resúmenes, traducción).
* ✅ Análisis de sentimientos (determinar si un texto es positivo o negativo).
* ✅ Reconocimiento de entidades (NER) (extraer nombres, ubicaciones, fechas de un texto).
* ✅ Traducción automática entre múltiples idiomas.
* ✅ Clasificación de texto (spam o no spam, tema de un artículo).
* ✅ Visión por computadora (detección de objetos, segmentación de imágenes).


```python
from transformers import pipeline
from PIL import Image
import requests

# Descargar una imagen de prueba
url = "https://upload.wikimedia.org/wikipedia/commons/3/3a/Cat03.jpg"
imagen = Image.open(requests.get(url, stream=True).raw)

# Cargar modelo de reconocimiento de objetos
detector = pipeline("object-detection")

# Aplicar IA a la imagen
resultado = detector(imagen)

# Mostrar objetos detectados
for obj in resultado:
    print(f"Objeto: {obj['label']} - Confianza: {obj['score']:.2f}")

```
