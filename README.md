\documentclass[12pt]{article}

\usepackage[spanish]{babel}
\usepackage[utf8]{inputenc}
\usepackage{amsmath}
\usepackage{graphicx}
\usepackage{hyperref}

\title{
Descripción del código implementado para el análisis de fuga espectral mediante FFT
}

\author{
Alix E. Maldonado
}

\date{}


\begin{document}

\maketitle


\section{Descripción general}

El código desarrollado tiene como objetivo analizar la representación de
una señal sinusoidal mediante la Transformada Rápida de Fourier (FFT) y
observar el fenómeno de fuga espectral.

La implementación se basa en el comportamiento descrito en el artículo
\textit{DDSP: Differentiable Digital Signal Processing}, donde se explica
que una señal cuya frecuencia no coincide con la base discreta de Fourier
puede presentar una distribución de energía entre frecuencias cercanas.


Inicialmente el código fue planteado para generar una señal sinusoidal,
calcular su FFT y observar su representación en frecuencia. A partir de
esta primera versión se realizaron modificaciones para poder comparar
diferentes condiciones de análisis.



\section{Código inicial}

La primera versión del código permitía:

\begin{itemize}

\item Definir la frecuencia de muestreo y el tiempo de análisis.

\item Generar una señal sinusoidal.

\item Calcular la Transformada Rápida de Fourier mediante la función
\texttt{fft()}.

\item Obtener la representación frecuencial de la señal.

\item Graficar el espectro obtenido.

\end{itemize}


La señal inicial utilizada fue:


\begin{equation}
x_1(t)=\cos(2\pi445t)
\end{equation}


Esta señal fue seleccionada debido a que, con la resolución utilizada,
su frecuencia no coincide exactamente con un punto de la FFT, permitiendo
observar la fuga espectral.



\section{Modificaciones realizadas}

Para ampliar el análisis se realizaron cambios sobre el código inicial.


La principal modificación fue agregar una segunda señal de comparación:


\begin{equation}
x_2(t)=1.5\cos(2\pi440t+\frac{\pi}{4})
\end{equation}


Con esta modificación fue posible comparar:


\begin{itemize}

\item Una señal con fuga espectral debido a la frecuencia de 445 Hz.

\item Una señal cuya frecuencia coincide con un punto de la FFT utilizando
440 Hz.

\end{itemize}


Además, se modificaron algunos parámetros de la señal:


\begin{itemize}

\item Amplitud.

\item Frecuencia.

\item Fase.

\item Número de muestras.

\item Frecuencia de muestreo.

\end{itemize}


Estos cambios permitieron evaluar cómo la configuración utilizada afecta
la representación obtenida mediante Fourier.



\section{Análisis agregado al código}

Después de la modificación inicial se añadieron cálculos adicionales para
facilitar la interpretación de los resultados.


Se incorporó el cálculo de la resolución frecuencial:


\begin{equation}
\Delta f=\frac{f_s}{N}
\end{equation}


y la relación entre la frecuencia de la señal y la posición dentro del
espectro:


\begin{equation}
k=\frac{f_0}{\Delta f}
\end{equation}



Esto permitió determinar si una frecuencia coincidía o no con un punto
discreto de la FFT.


También se añadieron comparaciones entre la señal original y modificada,
permitiendo generar las gráficas utilizadas en el análisis:

\begin{itemize}

\item Comparación temporal.

\item Comparación frecuencial mediante FFT.

\item Reconstrucción mediante diferentes coeficientes de Fourier.

\item Observación del fenómeno de Gibbs.

\end{itemize}



\section{Pruebas realizadas}

Además del caso utilizado en el informe, se realizaron pruebas cambiando
los valores de frecuencia, amplitud, fase, frecuencia de muestreo y número
de muestras.

Estas pruebas permitieron observar cómo la resolución frecuencial cambia
la representación obtenida y determinar qué configuraciones generaban
mayor o menor fuga espectral.


Los resultados de estas pruebas se encuentran en el archivo:

\begin{center}
\texttt{Pruebas\_Fourier\_50\_experimentos.xlsx}
\end{center}



\section{Resultados obtenidos}

La modificación del código permitió comprobar que una señal de 445 Hz no
queda representada en una única componente debido a que:


\begin{equation}
\frac{445}{10}=44.5
\end{equation}


Por lo tanto, la energía aparece distribuida principalmente alrededor de
440 Hz y 450 Hz.


Al modificar la frecuencia a 440 Hz:


\begin{equation}
\frac{440}{10}=44
\end{equation}


la señal coincide con un punto de la FFT y presenta una representación
más concentrada.



\section{Repositorio}

El código completo utilizado para generar las señales, calcular la FFT y
obtener las gráficas se encuentra disponible en:


\begin{center}
\url{https://github.com/estefaniaM0708/Articulo2_Avanzadas}
\end{center}



\section{Referencia}

Engel, J., Hantrakul, L., Gu, C., \& Roberts, A. (2020).

\textit{DDSP: Differentiable Digital Signal Processing}.

International Conference on Learning Representations (ICLR).


\end{document}

