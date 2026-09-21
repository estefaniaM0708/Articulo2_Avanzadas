# Análisis de fuga espectral mediante FFT

## Descripción

Este repositorio contiene el código utilizado para analizar el fenómeno de **fuga espectral (*spectral leakage*)** en la representación de Fourier de una señal sinusoidal.

El desarrollo se basa en el comportamiento descrito en el artículo:

**DDSP: Differentiable Digital Signal Processing**  
Engel et al. (2020)

El objetivo es comparar dos señales:

- Una señal cuya frecuencia no coincide exactamente con los puntos disponibles de la FFT.
- Una señal modificada cuya frecuencia coincide con un punto de la representación discreta.

---

# Código inicial

La primera versión del código permitía generar una señal sinusoidal, calcular su Transformada Rápida de Fourier (FFT) y observar su representación en frecuencia.

El procedimiento inicial consistía en:

- Definir la frecuencia de muestreo.
- Crear la señal en el dominio temporal.
- Aplicar la función `fft()`.
- Obtener el vector de frecuencias.
- Graficar el espectro obtenido.

La señal inicial utilizada fue:

$$
x_1(t)=cos(2\pi445t)
$$


Esta frecuencia fue seleccionada porque, con la resolución utilizada, no coincide exactamente con un punto de la FFT.

---

# Modificaciones realizadas

A partir del código inicial se realizaron modificaciones para poder comparar diferentes condiciones de análisis.

## Segunda señal implementada

Se agregó una señal modificada:

$$
x_2(t)=1.5cos(2\pi440t+\frac{\pi}{4})
$$


Los cambios realizados fueron:

| Parámetro | Original | Modificado |
|---|---|---|
| Amplitud | 1 | 1.5 |
| Frecuencia | 445 Hz | 440 Hz |
| Fase | 0 | π/4 |


La modificación principal corresponde a la frecuencia.

Para la señal inicial:

$$
k=\frac{445}{10}=44.5
$$

La frecuencia queda entre dos puntos de la FFT.


Para la señal modificada:

$$
k=\frac{440}{10}=44
$$

La frecuencia coincide con un punto exacto del espectro.

---

# Cálculo de resolución frecuencial

El código fue ampliado para calcular la resolución del sistema:

$$
\Delta f=\frac{f_s}{N}
$$


Para el caso principal:

$$
\Delta f=\frac{16000}{1600}=10Hz
$$


Esto permite determinar si una frecuencia coincide con la base discreta de Fourier.

---

# Datos adicionales agregados

Además del cálculo inicial de la FFT, se añadieron:

- Magnitud espectral.
- Vector de frecuencias.
- Comparación entre señales.
- Análisis de diferentes configuraciones.


También se realizaron pruebas modificando:

- Frecuencia de la señal.
- Amplitud.
- Fase.
- Frecuencia de muestreo.
- Número de muestras.


Estas pruebas permitieron identificar qué configuraciones producían fuga espectral y cuáles generaban una representación más concentrada.

El registro de pruebas se encuentra en:
