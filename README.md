# Análisis de Fourier y fuga espectral mediante FFT

## Descripción del proyecto

Este repositorio contiene la implementación utilizada para analizar el fenómeno de **fuga espectral (*spectral leakage*)** en la representación de Fourier de una señal sinusoidal.

El desarrollo está basado en el comportamiento descrito en el artículo:

**DDSP: Differentiable Digital Signal Processing**  
Engel et al. (2020)

donde se explica que una señal cuya frecuencia no coincide exactamente con la base discreta utilizada por Fourier puede presentar una distribución de energía entre componentes frecuenciales cercanas.

El objetivo del código es comparar dos escenarios:

1. Una señal cuya frecuencia no coincide con un punto de la FFT, generando fuga espectral.
2. Una señal modificada cuya frecuencia coincide con un punto de la FFT, reduciendo la dispersión espectral.

---

# Versión inicial del código

El código inicial fue desarrollado para generar una señal sinusoidal y analizar su representación frecuencial mediante la Transformada Rápida de Fourier (FFT).

La estructura inicial realizaba los siguientes pasos:

1. Definición de los parámetros de simulación:

- Frecuencia de muestreo.
- Tiempo de análisis.
- Número de muestras.

2. Generación de una señal sinusoidal:

\[
x_1(t)=\cos(2\pi445t)
\]


3. Cálculo de la Transformada Rápida de Fourier:

\[
X[k]=FFT(x[n])
\]


4. Obtención del vector de frecuencias:

\[
f_k=\frac{kf_s}{N}
\]


5. Representación gráfica del espectro obtenido.


El código inicial permitía observar la distribución frecuencial de una señal, pero solamente analizaba un único caso y no permitía comparar el efecto de modificar los parámetros de la señal.

---

# Modificaciones realizadas al código original

Para realizar el análisis completo de fuga espectral fue necesario ampliar la implementación inicial.

Las principales modificaciones fueron:

---

## 1. Inclusión de una segunda señal de comparación

Se agregó una nueva señal modificada:

\[
x_2(t)=1.5\cos(2\pi440t+\frac{\pi}{4})
\]


Esta modificación permitió comparar:

### Señal original

\[
f_0=445Hz
\]


donde:

\[
k=\frac{445}{10}=44.5
\]


La frecuencia queda ubicada entre dos puntos de la FFT y genera fuga espectral.


---

### Señal modificada

\[
f_0=440Hz
\]


donde:

\[
k=\frac{440}{10}=44
\]


La frecuencia coincide exactamente con un punto del espectro, permitiendo una representación más localizada.

---

# 2. Variación de amplitud y fase

Además del cambio de frecuencia se añadieron modificaciones en:

## Amplitud

Se pasó de:

\[
A=1
\]

a:

\[
A=1.5
\]


Esto permitió observar cómo la amplitud afecta directamente la magnitud de los componentes espectrales.


---

## Fase

Se modificó:

\[
\phi=0
\]

a:

\[
\phi=\frac{\pi}{4}
\]


Este cambio permitió analizar el desplazamiento temporal de la señal sin modificar su frecuencia principal.

---

# 3. Obtención de datos espectrales

El código fue ampliado para obtener información adicional del espectro:

- Frecuencia de cada componente.
- Magnitud asociada.
- Ubicación del pico principal.
- Comparación entre señales.


Esto permitió generar tablas con los componentes principales obtenidos mediante FFT.

---

# 4. Análisis de resolución frecuencial

Se añadió el cálculo automático de la resolución frecuencial:

\[
\Delta f=\frac{f_s}{N}
\]


Para el caso principal:

\[
\Delta f=
\frac{16000}{1600}
=
10Hz
\]


Este cálculo permite determinar si una frecuencia coincide con la base discreta de Fourier.


También se añadió el cálculo:

\[
k=\frac{f_0}{\Delta f}
\]


para identificar si la frecuencia analizada corresponde a un bin exacto de la FFT.

---

# 5. Reconstrucción y convergencia

Se agregó una etapa adicional para evaluar la reconstrucción mediante diferentes cantidades de coeficientes de Fourier.


Se analizaron diferentes valores de:

\[
K
\]


permitiendo observar que:

\[
K\uparrow
\Rightarrow
Error\downarrow
\]


Es decir, al aumentar la cantidad de términos utilizados, la señal reconstruida se aproxima más a la señal original.

---

# 6. Evaluación del fenómeno de Gibbs

También se añadió una sección para observar el comportamiento de la reconstrucción cerca de los límites de la señal.

Esto permitió analizar las oscilaciones generadas por la extensión periódica de Fourier cuando existe una discontinuidad.

---

# Estructura del código final

La versión final del código contiene los siguientes módulos:

