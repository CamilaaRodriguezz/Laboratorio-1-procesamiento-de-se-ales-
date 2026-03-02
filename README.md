# LABORATORIO 1 PROCESAMIENTO DIGITAL DE SEÑALES
# Análisis estadísticos de las señales 
## Docente : Manuel Fernando Torres
## Integrantes : Maria Camila Rodriguez Gomez 


## Fecha : Febrero 2026
# Introduccion: 
Las señales biomédicas constituyen una fuente esencial de información clínica, ya que permiten analizar el funcionamiento de diferentes sistemas fisiológicos. Sin embargo, dichas señales suelen estar afectadas por diversos tipos de ruido, lo que dificulta su interpretación. En este laboratorio se trabajó con señales fisiológicas reales y generadas, con el objetivo de calcular sus principales parámetros estadísticos, evaluar la relación señal-ruido (SNR) y comparar los resultados obtenidos en diferentes condiciones de adquisición.
# Marco teorico:
Las señales biomédicas son variaciones eléctricas, mecánicas o químicas generadas por el cuerpo humano que permiten analizar la actividad fisiológica. Entre las más comunes se encuentran el electrocardiograma (ECG), el electroencefalograma (EEG) y la electromiografía (EMG).

El análisis de estas señales requiere considerar tanto la información útil (amplitud, frecuencia, patrones característicos) como la presencia de ruido, entendido como cualquier perturbación que distorsiona la señal original y puede provenir de fuentes internas (movimientos, interferencias fisiológicas) o externas (equipos, ambiente).

Para caracterizar cuantitativamente una señal, se emplean estadísticos descriptivos, entre los que destacan:

·Media: valor promedio de la señal, indica la tendencia central.

·Desviación estándar: mide la dispersión de los datos respecto a la media.

·Coeficiente de variación: relaciona la desviación estándar con la media, expresando la variabilidad relativa.

·Histograma: representa gráficamente la distribución de frecuencias.

·Función de probabilidad: describe la probabilidad de ocurrencia de ciertos valores de la señal.

Además, la relación señal-ruido (SNR) es un parámetro fundamental que compara la potencia de la señal útil frente a la potencia del ruido. Un SNR alto implica una señal clara y confiable, mientras que un SNR bajo indica una señal fuertemente contaminada.

El uso de herramientas computacionales como Python, con librerías como NumPy, SciPy y Matplotlib, permite importar, procesar y analizar señales fisiológicas de manera eficiente. Asimismo, plataformas como PhysioNet ofrecen bases de datos abiertas que facilitan el acceso a señales reales para fines académicos e investigativos.
# Objetivo:
-Identificar los estadísticos que describen una señal biomédica, obtenerlos a partir de algoritmos de programación y mostrarlos. 
- Implementar algoritmos de Python.
- comparara señales fisiologicas de bases de datos con señales que fueron generadas en un laboratorio
- analizar el efecto del ruido sobre las señales capturadas 
# Procedimiento:
Se descargaron señales fisiologicas desde una base de datos estas señales fueron importadas en Python para luego hacer un calculo estadisticos de esta señal como la medioa, la desviacion, coeficientes de variacion entre otro.
se tomaron datos fisiologicos en el laboratorio y a estos se les realizo el mismo poceso que a la señal tomada de la base de datos para luego hacer una comparacion de estas señales.
por ultimo se implemento ruido a la señal analizando el efecto que tenia sobre esta

## Parte A
Se descargó una señal fisiológica desde PhysioNet, la cual fue importada y graficada en Python utilizando Matplotlib. Posteriormente, se calcularon los estadísticos descriptivos (media, desviación estándar, coeficiente de variación, histograma y función de probabilidad) tanto mediante fórmulas programadas como con funciones predefinidas de NumPy y SciPy.
```
#Descarga de librerias
!pip install wfdb
!pip install numpy matplotlib scipy pandas

```
```
# Se importan las librerias instaladas anteriormente
import wfdb
import numpy as np
import matplotlib.pyplot as plt
# skew y curtosis son para calcular asimetria y curtosis respectivamente
from scipy.stats import skew, kurtosis

```
```
# Con drive.mount se conecta google colab con Google drive para importar la señal guardada en .txt 
from google.colab import drive
drive.mount('/content/drive')
# Con la libreria anteriormente instalada de wfdb podemos leer la señal fisiologica
record = wfdb.rdrecord('/content/drive/MyDrive/Colab Notebooks/121001_ECG')
# p_signal es un tipo de matriz que contiene la señal
signal = record.p_signal[:,0]

```
```
# Se grafico la señal mediante la libreria Matplotlib, ademas se puso el titulo de la grafica, una cuadricula y se añadieron las etiquetas de los ejes
plt.figure()
plt.plot(signal)
plt.title("Señal ECG PhysioNet")
plt.xlabel("Muestras")
plt.ylabel("Amplitud")
plt.grid()
plt.show()

```
<img width="600" height="455" alt="image" src="https://github.com/user-attachments/assets/ac1a5ba5-27ca-476e-8408-da80fdb6f113" />


```
#Se recorta la señal para tener una grafica con mejor definicion, recortamos la grafica anterior de la muestra 1000 a la 5999
señal=signal[1000:6000]
plt.plot(señal)

```
<img width="560" height="413" alt="image" src="https://github.com/user-attachments/assets/6543d84c-4fb8-47de-81cf-32b5705da9ad" />

Inicio cálculos estadísticos
```
# Número de datos
#len() cuenta cuantos datos tiene la señal
N = len(señal)
# 1. MEDIA
#Se suman todos los datos de la señal y se dividen entre el numero total de datos
media = sum(señal) / N
# 2. DESVIACIÓN ESTÁNDAR
#Se recorre cada dato x iniciando desde 0 y se va restando a la media y elevando al cuadrado.
#finalmente se suman todos esos datos en suma_var
suma_var = 0
for x in señal:
    suma_var += (x - media)**2
#Para tener la varianza dividmios suma_var sobre el numero de datos
varianza = suma_var / N
#sacamos la raiz cuadrada de la varianza para obtener desviacion estandar
desv_std = np.sqrt(varianza)
# 3. COEFICIENTE DE VARIACIÓN
cv = desv_std / media
# 4. ASIMETRÍA (SKEWNESS)
#Calculamos hacia que lado se inclina la distribucion
suma_skew = 0
for x in señal:
    suma_skew += (x - media)**3

skewness = (suma_skew / N) / (desv_std**3)
# 5. CURTOSIS
Mide si tenemos una distribucion leptocurtica, mesocurtica o platicurtica usando la formul ade curtosis
suma_kurt = 0
for x in señal:
    suma_kurt += (x - media)**4
kurt = (suma_kurt / N) / (desv_std**4)

```
```
# RESULTADOS
print(" ESTADÍSTICOS (DESDE CERO) ")
print("Media:", media)
print("Desviación estándar:", desv_std)
print("Coeficiente de variación:", cv)
print("Asimetría:", skewness)

```
```
#Vamos a graficar el histogrma de un tamaño de 10 de ancho y 5 de alto
#El histograma sera de los datos señal
#bins es para dividir los datos en intervalos de 50 datos
# Ademas agrega caracteristicas como titulo de la grafica, color del histograma y nombres de los ejes
plt.figure(figsize=(10,5))
plt.hist(señal, bins=50, color='pink', edgecolor='black', density=True)
plt.title("Histograma de la señal 1 physioNet")
plt.xlabel("Amplitud (mS)")
plt.ylabel("Frecuencia (Hz)")
plt.grid(True)
plt.show()

```
<img width="872" height="470" alt="image" src="https://github.com/user-attachments/assets/83050ce5-67ed-49ab-a89d-dda1b27b91ad" />

## Parte B
Se generó una señal ECG a partir del generador de señales y se capturó con un DAQ para posteriormente imporatrla a spyder y asi poder graficarla en el histograma y graficar la señla del generador de señales en el computador.

```

# Cargar archivo .txt:
#delimiter='\t' separa columnas por tabulaciones
#skiprows=1 ignora la primera fila (encabezado) 
datos = np.loadtxt('/content/drive/MyDrive/Colab Notebooks/senal_ecg.txt', delimiter='\t', skiprows=1)

# Separar columnas
# t las separa en tiempo
# senal amplitud del ECG
t = datos[:, 0]
senal = datos[:, 1]

# Calcular frecuencia de muestreo automáticamente
fs = 1 / (t[1] - t[0])

# Graficar
#Grafica la señal en funcion de tiempo
plt.figure(figsize=(12,4))
plt.plot(t, senal)
plt.xlabel("Tiempo (s)")
plt.ylabel("Voltaje (V)")
plt.title("Señal ECG")
plt.grid()
plt.show()

```
<img width="1012" height="393" alt="image" src="https://github.com/user-attachments/assets/518c1289-ff1a-4733-99a3-d963cfa35cee" />

```
#Hacemos los msimos calculos de la parte A pero con la señal capturada por el DAQ
N = len(senal)

# 1. Media
media = sum(senal) / N

# 2. Desviación estándar
suma_var = 0
for x in senal:
    suma_var += (x - media)**2

varianza = suma_var / N
desv_std = np.sqrt(varianza)

# 3. Coeficiente de variación
cv = desv_std / media if media != 0 else 0

# 4. Asimetría
suma_skew = 0
for x in senal:
    suma_skew += (x - media)**3

skew_manual = (suma_skew / N) / (desv_std**3)

# 5. Curtosis
suma_kurt = 0
for x in senal:
    suma_kurt += (x - media)**4

kurt_manual = (suma_kurt / N) / (desv_std**4)

print("=== MÉTODO MANUAL ===")
print("Media:", media)
print("Desviación estándar:", desv_std)
print("Coeficiente de variación:", cv)
print("Asimetría:", skew_manual)
print("Curtosis:", kurt_manual)
print("Curtosis:", kurt)
```
=== MÉTODO MANUAL ===
Media: -0.2755946689535922

Desviación estándar: 0.13148674086055573

Coeficiente de variación: -0.4771019024417231

Asimetría: 4.174142421525527

Curtosis: 22.28656812149953
```
plt.figure(figsize=(10,5))
plt.hist(senal, bins=50, color='purple', edgecolor='black', density=True)
plt.title("Histograma de la señal 2 (Parte B Generador)")
plt.xlabel("Amplitud (mV)")
plt.ylabel("Densidad de probabilidad ")
plt.grid(True)
plt.show()

```
<img width="841" height="470" alt="image" src="https://github.com/user-attachments/assets/ab7fbd70-15a1-4ed0-8d45-b5b39589bf33" />

