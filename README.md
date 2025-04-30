# Variabilidad-de-la-Frecuencia-Cardiaca-usando-la-Transformada-Wavelet


## Fundamentos Teoricos

### Nuestro corazón no late siempre a la misma velocidad; entre un latido y el siguiente se producen pequeñas aceleraciones y desaceleraciones. Estas variaciones, conocidas como Variabilidad de la Frecuencia Cardíaca (HRV), nos indican cómo el cuerpo gestiona el estrés, el descanso y las respuestas al entorno.

## Control autonómico

**Sistema simpático (acelerador):** eleva las pulsaciones cuando hacemos ejercicio, nos asustamos o nos estresamos.

**Sistema parasimpático (freno):** reduce las pulsaciones cuando estamos relajados, descansando o durmiendo.

## ¿Qué mide la HRV?

**La HRV cuantifica las diferencias de tiempo entre latidos sucesivos (intervalos R-R).**

**Una HRV alta sugiere un sistema nervioso autónomo flexible y bien adaptado.**

**Una HRV baja puede reflejar fatiga, estrés prolongado o posibles alteraciones de salud.**

## Métodos de análisis

**Dominio del tiempo:** calculamos la media y la desviación estándar de los intervalos R-R para evaluar su consistencia.

**Dominio tiempo-frecuencia:** aplicamos la Transformada Wavelet —un “microscopio” dinámico— para detectar cómo cambian las frecuencias  a lo largo de los 5 minutos de registro.


# Adquisicion de la señal 


### La señal ECG fue adquirida utilizando un sensor de ECG de superficie conectado a un sistema de adquisición basado en la placa STM32. La grabación se realizó en un sujeto en estado de reposo, sin movimiento, durante un periodo continuo de 5 minutos.

### Características de la adquisición:

- **Frecuencia de muestreo:** 400 Hz

- **Tiempo total de adquisición:** 300 segundos

- **Cantidad total de muestras:** 120,000

- **Nivel de cuantificación:** 12 bits (valores entre 0 y 4095 ADC)

- **Condiciones:** sujeto en reposo, en ambiente controlado, algun juego o actividad de respiracion para aumentar frecuencias por ciertos periodos

  *La señal fue almacenada en un archivo de texto (.txt) y posteriormente procesada con Python. A continuación se muestra la señal cruda sin filtrar, representando los valores directamente adquiridos del sensor:*

```python
  import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, filtfilt, find_peaks
import pywt
import time as tm

# ========= 1. Cargar los datos =========

with open('DANIEL01.txt', 'r') as file:
    data = file.readlines()

# Convertir datos a números
ecg_signal = np.array([int(x.strip()) for x in data])

# ========= 2. Definir parámetros =========

sampling_rate = 400  # Hz
lowcut = 0.5  # Hz
highcut = 40.0  # Hz
order = 4

time = np.arange(len(ecg_signal)) / sampling_rate

adc_max = 4095
v_ref = 3.3  # voltios
ecg_mv = (ecg_signal / adc_max) * v_ref * 1000  # señal en milivoltios

plt.figure(figsize=(15, 5))
plt.plot(time, ecg_mv, color='gray')
plt.title('ECG ORIGINAL Escala mv')
plt.xlabel('Tiempo (s)')
plt.ylabel('Amplitud (mV)')
plt.grid(True)
plt.show()
```

![image](https://github.com/user-attachments/assets/7a1cd509-b354-4069-a14a-12db32e42009)
![image](https://github.com/user-attachments/assets/3c9db63d-d91d-4b42-bd0e-408eaf12558f)




