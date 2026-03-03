# LABORATORIO 1 PROCESAMIENTO DIGITAL DE SEÑALES
# Análisis estadísticos de las señales 
## Docente : Manuel Fernando Torres
## Integrantes : Maria Camila Rodriguez Gomez, Valery Guerra Mendez, Danna Gabriela Moyano Cano



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
```
```
# 1. MEDIA
#Se suman todos los datos de la señal y se dividen entre el numero total de datos
media = sum(señal) / N
```
```
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
```
```
# 3. COEFICIENTE DE VARIACIÓN
cv = desv_std / media
```
```
# 4. ASIMETRÍA (SKEWNESS)
#Calculamos hacia que lado se inclina la distribucion
suma_skew = 0
for x in señal:
    suma_skew += (x - media)**3

skewness = (suma_skew / N) / (desv_std**3)
```
```
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
Con el método manual tenemos los siguientes resultados: 

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

 Y a esto que nos dió se le hace la distribución de los datos

```
x = np.linspace(min(senal), max(senal), 1000)
pdf = (1/(desv_std*np.sqrt(2*np.pi))) * np.exp(-(x-media)**2/(2*desv_std**2))

plt.figure(figsize=(10,5))
plt.hist(senal, bins=50, density=True, edgecolor='black')
plt.plot(x, pdf)
plt.title("Histograma + Distribución normal")
plt.xlabel("Amplitud (mV)")
plt.ylabel("Densidad de probabilidad")
plt.grid()
plt.show()
```
 <img width="841" height="471" alt="image" src="https://github.com/user-attachments/assets/dc0c84ff-d0fa-40db-a0f6-e417eec78630" />

## Parte C
Para esta última parte de la práctica se centró en el SNR, tomando la señal usada en la parte B y contaminarla de distintas maneras y así mismo sacar su SNR.

Primero se contaminó con un ruido gaussiano:
```
def calcular_snr(senal_original, senal_ruido):
    ruido = senal_ruido - senal_original
    potencia_senal = np.mean(senal_original**2)
    potencia_ruido = np.mean(ruido**2)
    snr = 10 * np.log10(potencia_senal / potencia_ruido)
    return snr
     

# Generar ruido gaussiano
ruido_gauss = np.random.normal(0, 0.1, len(senal))

# Señal contaminada
senal_gauss = senal + ruido_gauss

# Calcular SNR
snr_gauss = calcular_snr(senal, senal_gauss)

print("SNR con ruido gaussiano:", snr_gauss, "dB")

# Graficar
plt.figure(figsize=(12,4))
plt.plot(senal_gauss)
plt.title("Señal con ruido gaussiano")
plt.grid()
plt.show()
     
SNR con ruido gaussiano: 9.483493409250972 dB
```
<img width="993" height="374" alt="image" src="https://github.com/user-attachments/assets/c40b48e3-5afa-476d-a36b-62e7450010b9" />

Y se hizo una comparativa entre la señal original con el ruido administrado:
```
plt.figure(figsize=(12,4))
plt.plot(senal, label="Original")
plt.plot(senal_gauss, alpha=0.7, label="Con ruido")
plt.legend()
plt.title("Comparación señal original vs ruido gaussiano")
plt.grid()
plt.show()
```

<img width="993" height="374" alt="image" src="https://github.com/user-attachments/assets/702ee19a-e870-470a-a24f-1c8f5f36a760" />

En segundo lugar se contaminó la señal con ruido impulso quedando que:

```
senal_impulso = senal.copy()

# Crear impulsos aleatorios
num_impulsos = int(0.01 * len(senal))  # 1% de la señal
indices = np.random.randint(0, len(senal), num_impulsos)

senal_impulso[indices] += np.random.choice([3, -3], size=num_impulsos)

# SNR
snr_impulso = calcular_snr(senal, senal_impulso)

print("SNR con ruido impulso:", snr_impulso, "dB")

# Graficar
plt.figure(figsize=(12,4))
plt.plot(senal_impulso)
plt.title("Señal con ruido impulso")
plt.grid()
plt.show()
     
SNR con ruido impulso: 0.15365272915878503 dB
```
Y se realizó su comparativa:

```
plt.figure(figsize=(12,4))
plt.plot(senal, label="Original")
plt.plot(senal_impulso, alpha=0.7, label="Con impulso")
plt.legend()
plt.title("Comparación señal original vs ruido impulso")
plt.grid()
plt.show()   
```
<img width="980" height="374" alt="image" src="https://github.com/user-attachments/assets/7b37f92b-5e79-4461-8ab3-3d1bda93c3bc" />

Finalmente, se contaminó la señal con un ruido tipo artefacto:

```
t = np.arange(len(senal))

# Ruido tipo artefacto (baja frecuencia)
ruido_artefacto = 0.5 * np.sin(0.01 * t)

senal_artefacto = senal + ruido_artefacto

# SNR
snr_artefacto = calcular_snr(senal, senal_artefacto)

print("SNR con ruido artefacto:", snr_artefacto, "dB")

# Graficar
plt.figure(figsize=(12,4))
plt.plot(senal_artefacto)
plt.title("Señal con ruido tipo artefacto")
plt.grid()
plt.show()
     
SNR con ruido artefacto: -1.0687714480082486 dB
```
<img width="1002" height="374" alt="image" src="https://github.com/user-attachments/assets/c7c325a5-83a8-48b7-aeae-a4ca7cf502ba" />

Y se comparó con la señal original:

```
plt.figure(figsize=(12,4))
plt.plot(senal, label="Original")
plt.plot(senal_artefacto, alpha=0.7, label="Con artefacto")
plt.legend()
plt.title("Comparación señal original vs artefacto")
plt.grid()
plt.show()
```
<img width="1002" height="374" alt="image" src="https://github.com/user-attachments/assets/fe8c1e8d-5ebd-4029-84c9-652e31ff1ae9" />

# Conclusion
En esta práctica se analizaron diferentes características de una señal utilizando medidas estadísticas como el promedio, la desviación estándar, la asimetría y la curtosis. Estas herramientas permitieron describir su comportamiento general y comprender mejor cómo se distribuyen sus valores, sin depender únicamente de puntos específicos.

Además, al comparar una señal descargada con una adquirida por el generador de señales, se evidenciaron diferencias asociadas principalmente al ruido y a las condiciones de su captura. El análisis de la relación señal-ruido permitió observar cómo distintos tipos de contaminación afectan la calidad de la señal y, en consecuencia, los resultados obtenidos. En conclusión, el uso de la estadística resulta útil para evaluar la calidad de una señal y facilitar su interpretación, aunque es necesario complementarlo con otros métodos para lograr un análisis más completo.
