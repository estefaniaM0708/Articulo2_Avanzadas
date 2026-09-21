# Articulo2 Matemáticas Avanzadas

import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path

//REPRESENTACION DE FOURIER Y FUGA ESPECTRAL

// 1. Parámetros comunes

fs = 16000.0          # frecuencia de muestreo [Hz]

T = 0.1               # duracion de la ventana [s]

N = int(fs * T)       # numero de muestras = 1600

Delta_f = fs / N      # resolucion frecuencial = 10 Hz

t = np.arange(N) / fs

//2. Datos originales del artículo


A1 = 1.0

f1 = 445.0            # 445 / 10 = 44.5 -> NO coincide con un bin de Fourier

phi1 = 0.0


x1 = A1 * np.cos(2 * np.pi * f1 * t + phi1)


//3. Datos modificados

A2 = 1.5
f2 = 440.0            # 440 / 10 = 44 -> SI coincide con un bin de Fourier

phi2 = np.pi / 4      # 45 grados

x2 = A2 * np.cos(2 * np.pi * f2 * t + phi2)


//FUNCIONES AUXILIARES


def coef_fourier_complejo(k, A, f0, phi, T):

    """
    
    Coeficiente complejo C_k de la serie de Fourier de la extension
    
    periodica, de periodo T, del segmento:
    
        x(t) = A cos(2*pi*f0*t + phi), 0 <= t < T
        

    Convencion:
    
        x(t) = sum_k C_k exp(j*2*pi*k*t/T)
        
        sinc(u) = sin(pi*u)/(pi*u)
        
    """
    
    nu0 = f0 * T
    
    termino_pos = (
    
    
        np.exp(1j * phi)
        
        * np.exp(1j * np.pi * (nu0 - k))
        
        * np.sinc(nu0 - k)
        
    )
    
    termino_neg = (
    
        np.exp(-1j * phi)
        
        * np.exp(-1j * np.pi * (nu0 + k))
        
        * np.sinc(nu0 + k)
        
    )
    
    return (A / 2.0) * (termino_pos + termino_neg)


def reconstruccion_serie(t, K, A, f0, phi, T):

    """Reconstruye la senal usando coeficientes C_k para -K <= k <= K."""
    
    ks = np.arange(-K, K + 1)
    
    Ck = np.array([coef_fourier_complejo(int(k), A, f0, phi, T) for k in ks])
    
    base = np.exp(1j * 2 * np.pi * ks[:, None] * t[None, :] / T)
    
    xr = np.sum(Ck[:, None] * base, axis=0)
    
    return xr.real

def espectro_unilateral(x, fs):

    """FFT unilateral escalada en amplitud para una senal real."""
    
    N = len(x)
    
    X = np.fft.rfft(x)
    
    f = np.fft.rfftfreq(N, d=1 / fs)
    
    amp = 2.0 * np.abs(X) / N
    
    amp[0] /= 2.0
    
    if N % 2 == 0:

        amp[-1] /= 2.0
        
    phase = np.angle(X)
    
    return f, X, amp, phase

//4. FFT DE AMBOS ESCENARIOS

freq1, X1, amp1, phase_fft1 = espectro_unilateral(x1, fs)

freq2, X2, amp2, phase_fft2 = espectro_unilateral(x2, fs)


print("=" * 72)

print("PARAMETROS")

print("=" * 72)

print(f"fs = {fs:.0f} Hz")

print(f"T = {T:.3f} s")

print(f"N = {N}")

print(f"Delta_f = fs/N = {Delta_f:.3f} Hz")

print()

print(f"Original:   A={A1}, f0={f1} Hz, phi={phi1:.4f} rad, f0/Delta_f={f1/Delta_f:.2f}")

print(f"Modificada: A={A2}, f0={f2} Hz, phi={phi2:.4f} rad, f0/Delta_f={f2/Delta_f:.2f}")

//5. COEFICIENTES ANALITICOS CERCA DE LA FRECUENCIA DOMINANTE

print("\n" + "=" * 72)

print("COEFICIENTES ANALITICOS - ESCENARIO ORIGINAL")

print("C_k = a_k/2 - j b_k/2  =>  a_k=2Re(C_k), b_k=-2Im(C_k)")

print("=" * 72)

print(f"{'k':>4} {'f_k[Hz]':>10} {'Re(Ck)':>12} {'Im(Ck)':>12} {'|Ck|':>12} {'2|Ck|':>12}")

for k in range(41, 49):

    ck = coef_fourier_complejo(k, A1, f1, phi1, T)
    
    print(f"{k:4d} {k/T:10.1f} {ck.real:12.6f} {ck.imag:12.6f} {abs(ck):12.6f} {2*abs(ck):12.6f}")
    

print("\n" + "=" * 72)

print("COEFICIENTES ANALITICOS - ESCENARIO MODIFICADO")

print("=" * 72)


for k in [43, 44, 45]:

    ck = coef_fourier_complejo(k, A2, f2, phi2, T)
    
    print(
    
        f"k={k:2d}, f_k={k/T:6.1f} Hz, C_k={ck.real:+.6f}{ck.imag:+.6f}j, "
        
        f"|C_k|={abs(ck):.6f}, fase={np.angle(ck):+.6f} rad"
    )

//6. METRICA SIMPLE DE FUGA ESPECTRAL

//En el del artículo la energia principal queda repartida entre 440 y 450 Hz.

pot1 = np.abs(X1) ** 2

frac_principal_original = (pot1[44] + pot1[45]) / np.sum(pot1)

//En el modificado, el bin principal es k=44 (440 Hz).


pot2 = np.abs(X2) ** 2

frac_principal_mod = pot2[44] / np.sum(pot2)

print("\n" + "=" * 72)

print("CONCENTRACION ESPECTRAL")

print("=" * 72)

print(f"Original - fraccion de energia en bins 44 y 45: {100*frac_principal_original:.3f}%")

print(f"Modificada - fraccion de energia en bin 44:       {100*frac_principal_mod:.3f}%")

print(f"Amplitud FFT original a 440 Hz: {amp1[44]:.6f}")

print(f"Amplitud FFT original a 450 Hz: {amp1[45]:.6f}")

print(f"Amplitud FFT modificada a 440 Hz: {amp2[44]:.6f}")


//7. CONVERGENCIA DE LA SERIE DE FOURIER - ARTÍCULO

Ks = [45, 60, 100, 300]

print("\n" + "=" * 72)

print("CONVERGENCIA DE LA SERIE - ESCENARIO ORIGINAL")

print("=" * 72)

print(f"{'K':>6} {'RMSE total':>14} {'RMSE interior':>16}")

mask_interior = (t > 0.001) & (t < T - 0.001)

reconstrucciones = {}

for K in Ks:

    xr = reconstruccion_serie(t, K, A1, f1, phi1, T)
    
    reconstrucciones[K] = xr
    
    rmse_total = np.sqrt(np.mean((x1 - xr) ** 2))
    
    rmse_interior = np.sqrt(np.mean((x1[mask_interior] - xr[mask_interior]) ** 2))
    
    print(f"{K:6d} {rmse_total:14.8f} {rmse_interior:16.8f}")

//Para la señal modificada, K=44 ya contiene exactamente el armónico necesario.

x2_rec = reconstruccion_serie(t, 44, A2, f2, phi2, T)

rmse_mod = np.sqrt(np.mean((x2 - x2_rec) ** 2))

print(f"\nRMSE modificada con K=44: {rmse_mod:.3e}")


//8. GRAFICAS

out_dir = Path("resultados_fourier")

out_dir.mkdir(exist_ok=True)

# Figura 1: dominio del tiempo

plt.figure(figsize=(10, 5))


plt.plot(t * 1000, x1, label="Original: 445 Hz, A=1, phi=0", linewidth=1.2)

plt.plot(t * 1000, x2, label="Modificada: 440 Hz, A=1.5, phi=pi/4", linewidth=1.0, alpha=0.8)

plt.xlim(0, 20)

plt.xlabel("Tiempo [ms]")


plt.ylabel("Amplitud")

plt.title("Comparacion en el dominio del tiempo")

plt.grid(True, alpha=0.25)

plt.legend()

plt.tight_layout()

plt.savefig(out_dir / "01_tiempo_original_vs_modificada.png", dpi=200)

# Figura 2: espectros cerca de la componente principal

plt.figure(figsize=(10, 5))

sel1 = (freq1 >= 350) & (freq1 <= 540)

plt.stem(freq1[sel1], amp1[sel1], linefmt="C0-", markerfmt="C0o", basefmt=" ", label="Original")

plt.stem(freq2[sel1], amp2[sel1], linefmt="C1-", markerfmt="C1s", basefmt=" ", label="Modificada")

plt.xlabel("Frecuencia [Hz]")

plt.ylabel("Amplitud unilateral")

plt.title("Representacion de Fourier: fuga espectral vs alineacion con el bin")

plt.grid(True, alpha=0.25)

plt.legend()

plt.tight_layout()

plt.savefig(out_dir / "02_espectro_original_vs_modificada.png", dpi=200)

# Figura 3: convergencia por sumas parciales

plt.figure(figsize=(10, 6))

plt.plot(t * 1000, x1, label="Senal original", linewidth=2)

for K in Ks:

    plt.plot(t * 1000, reconstrucciones[K], label=f"Serie truncada K={K}", linewidth=0.9)
    
plt.xlim(0, 5)

plt.xlabel("Tiempo [ms]")

plt.ylabel("Amplitud")

plt.title("Convergencia de la serie de Fourier - escenario original")

plt.grid(True, alpha=0.25)

plt.legend()

plt.tight_layout()

plt.savefig(out_dir / "03_convergencia_serie_original.png", dpi=200)

# Figura 4: detalle del borde para visualizar Gibbs

plt.figure(figsize=(10, 5))

plt.plot(t * 1000, x1, label="Senal original", linewidth=2)

for K in [60, 100, 300]:

    plt.plot(t * 1000, reconstrucciones[K], label=f"K={K}", linewidth=1)
    
plt.xlim(0, 2)

plt.xlabel("Tiempo [ms]")

plt.ylabel("Amplitud")

plt.title("Detalle cercano a la discontinuidad de la extension periodica")

plt.grid(True, alpha=0.25)

plt.legend()

plt.tight_layout()

plt.savefig(out_dir / "04_gibbs_borde.png", dpi=200)

plt.show()

print("\nGraficas guardadas en:", out_dir.resolve())
