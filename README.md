# DSB--SC-MODULATION-AND-DEMODULATION-USING-PYTHON

__AIM__:

To generate a Double Sideband Suppressed Carrier (DSB-SC) signal in Python (Google Colab), transmit it (optionally add noise), and recover the message using coherent (synchronous) demodulation with a low-pass filter. Observe time and frequency domain waveforms and measure demodulation performance

__APPARATUS REQUIRED__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)

__Theory__:

DSB-SC signal: s(t) = m(t) · cos(2πf_c t)
Coherent demodulation: multiply received s(t) by a synchronized carrier cos(2πf_c t) then low-pass filter (LPF) to remove double-frequency components:

r(t) = s(t)·cos(2πf_c t) = m(t)·cos²(2πf_c t) = 0.5 m(t) + 0.5 m(t)·cos(4πf_c t)
LPF extracts 0.5·m(t) → scale by 2 to recover m(t).

__Procedure__:

1) Import libraries and set parameters
2) Define message and carrier signals
3) Generate DSB-SC signal (modulation)
4) View spectra (FFT) of message and DSB-SC
5) (Optional) Add noise
6) Coherent demodulation (multiply by synchronized carrier)
7) Low-pass filter to recover message
8) 
Program:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Correct sampling frequency
fs = 10000
t = np.arange(0, 1, 1/fs)

# Parameters
fm = 30
fc = 300
Am = 11.1
Ac = 22.2

# Message and carrier signals
message = Am * np.cos(2 * np.pi * fm * t)
carrier = Ac * np.cos(2 * np.pi * fc * t)

# DSB-SC modulation
dsb_sc = message * carrier

# Demodulation (coherent detection)
demodulated_signal = dsb_sc * carrier

# Low-pass filter functions
def butter_lowpass(cutoff, fs, order=5):
    nyq = 0.5 * fs
    normal_cutoff = cutoff / nyq
    b, a = butter(order, normal_cutoff, btype='low')
    return b, a

def lowpass_filter(data, cutoff, fs, order=5):
    b, a = butter_lowpass(cutoff, fs, order)
    y = lfilter(b, a, data)
    return y

cutoff_freq = 100
rec_message = lowpass_filter(demodulated_signal, cutoff_freq, fs)

# Plotting
plt.figure(figsize=(12, 10))

plt.subplot(5, 1, 1)
plt.plot(t, message)
plt.title("Original Message Signal (30 Hz)")
plt.xlabel("Time (sec)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(5, 1, 2)
plt.plot(t, carrier)
plt.title("Carrier Signal (300 Hz)")
plt.xlabel("Time (sec)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(5, 1, 3)
plt.plot(t, dsb_sc)
plt.title("DSB-SC Modulated Signal")
plt.xlabel("Time (sec)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(5, 1, 4)
plt.plot(t, demodulated_signal)
plt.title("Demodulated Signal (Before LPF)")
plt.xlabel("Time (sec)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(5, 1, 5)
plt.plot(t, rec_message)
plt.title("Recovered Message Signal (After LPF)")
plt.xlabel("Time (sec)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()
```
   __Tabulation__:

<img width="616" height="891" alt="image" src="https://github.com/user-attachments/assets/44ba68c2-6064-4c29-809f-5b1b42dc0d47" />

   __Output__:

<img width="481" height="918" alt="image" src="https://github.com/user-attachments/assets/74cca082-a344-441f-8348-193a6e54ad40" />

   __Result__:

<img width="579" height="879" alt="image" src="https://github.com/user-attachments/assets/9d103a04-b528-49d1-8b81-857fc5ff6da3" />
