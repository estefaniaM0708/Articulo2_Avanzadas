# Análisis de Fourier y Fuga Espectral mediante FFT

## Descripción del proyecto

Este proyecto presenta la implementación y análisis de la representación de Fourier de una señal sinusoidal, enfocándose en el fenómeno de **fuga espectral (*spectral leakage*)**.

El desarrollo está basado en el análisis realizado en el artículo:

**DDSP: Differentiable Digital Signal Processing**  
Engel et al. (2020)

donde se explica que una señal puede presentar una distribución de energía entre diferentes componentes frecuenciales cuando su frecuencia no coincide exactamente con los valores discretos de la base de Fourier.

La implementación permite comparar una señal no alineada con la resolución espectral de la FFT y una señal modificada cuya frecuencia coincide con un punto de la representación discreta.

---

# Objetivo

Analizar cómo la elección de frecuencia, amplitud y fase afecta la representación de una señal en el dominio frecuencial mediante la Transformada Rápida de Fourier (FFT).

---

# Descripción de las señales analizadas

## Señal original

Se utiliza una señal sinusoidal:

\[
x_1(t)=cos(2\pi445t)
\]


Parámetros:

| Parámetro | Valor |
|---|---|
| Frecuencia de muestreo | 16000 Hz |
| Duración | 0.1 s |
| Número de muestras | 1600 |
| Frecuencia | 445 Hz |
| Amplitud | 1 |

La resolución frecuencial obtenida es:

\[
\Delta f=\frac{16000}{1600}=10Hz
\]

Como:

\[
\frac{445}{10}=44.5
\]

la frecuencia no coincide con un punto exacto de la FFT, generando fuga espectral.

---

## Señal modificada

Posteriormente se modifica la señal:

\[
x_2(t)=1.5cos(2\pi440t+\frac{\pi}{4})
\]


Cambios realizados:

| Variable | Original | Modificada |
|---|---|---|
| Amplitud | 1 | 1.5 |
| Frecuencia | 445 Hz | 440 Hz |
| Fase | 0 | π/4 |


En este caso:

\[
\frac{440}{10}=44
\]

por lo tanto la frecuencia coincide exactamente con un bin de Fourier, reduciendo la fuga espectral.

---

# Funcionamiento del código

El programa realiza los siguientes procesos:

1. Definición de parámetros de muestreo.
2. Generación de la señal original y modificada.
3. Cálculo de la Transformada Rápida de Fourier mediante `FFT`.
4. Obtención del vector de frecuencias.
5. Representación del espectro de magnitud.
6. Comparación entre la señal con fuga espectral y la señal alineada con la base de Fourier.

---

# Estructura del proyecto
