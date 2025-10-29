# Enhancing-UWA-OFDM-Channel-Estimation-A-Robust-CRNet-Framework-with-Dynamic-Signal-Decomposition

# UWA-OFDM Communication System with CRNet Channel Estimation  
This repository implements an end-to-end underwater acoustic (UWA) OFDM communication system, including data generation, transmitter, channel propagation, CRNet-based channel estimation, and performance analysis—strictly aligned with the parameters and methodology in *frontiers.pdf*.  


## 1. Data Generation  
- **Purpose**: Generate synthetic UWA channel data mimicking Bellhop ray-tracing results (per Section 3.1 of the paper).  
- **Details**: Produces paired received OFDM signals, transmitted pilot symbols, and true channel impulse responses (CIR) for two UWA environments:  
  - *Shallow Coastal Channel* (28 multipaths, 132.9 ms delay spread, ±0.6 Hz Doppler).  
  - *Continental Shelf Channel* (63 multipaths, 100 ms delay spread, ±0.45 Hz Doppler).  
- **Output**: Training/validation/test splits (80/20) with 6,000 samples (consistent with Section 2.1.3).  


## 2. OFDM Transmitter  
- **Purpose**: Generate OFDM signals following the paper’s Table 4 parameters.  
- **Details**:  
  - 1024 total subcarriers (767 data, 256 comb-type pilots, 1 DC guard).  
  - 128-sample cyclic prefix (CP) to mitigate multipath.  
  - QPSK modulation (gray-coded) for data subcarriers.  
- **Output**: Real-valued baseband OFDM signals compatible with UWA transducers.  


## 3. UWA Channel Propagation  
- **Purpose**: Simulate signal distortion in underwater channels (per Section 2 and Table 3).  
- **Key Effects**:  
  - Multipath propagation (Bellhop-inspired impulse response).  
  - Doppler shift (±0.6 Hz for shallow, ±0.45 Hz for shelf).  
  - AWGN noise (ship-induced or mixed, 0–25 dB SNR range).  
  - Transmission loss (60 dB for shallow, 80 dB for shelf).  


## 4. CRNet Channel Estimation  
- **Purpose**: Estimate channel coefficients using the CRNet framework (Section 2.1.1).  
- **Architecture**:  
  - Input: Received signals + transmitted pilots (shape: (1152, 1024, 10)).  
  - Layers: 2 Conv1D (64 filters), 2 LSTM (64 units), 1 Dense (128 units), and a custom complex output layer.  
- **Training**: Adam optimizer (lr=1e-3), batch size=64, 100 epochs, early stopping (patience=5) (Section 2.1.3).  


## 5. Results & Discussion  
- **Metrics**: Implements key metrics from Section 4:  
  -  MSE for channel estimation accuracy.  
  - BER (QPSK/QAM demodulation) for end-to-end performance.  
  - Amplitude/phase error for detailed channel analysis.  
- **Comparisons**: CRNet vs. baselines (LS, MMSE, BPNN) across SNR (0–25 dB), matching Table 5 and Figs. 3–5.  


## Dependencies  
`numpy>=1.23`, `tensorflow>=2.10`, `matplotlib>=3.6`, `seaborn>=0.12`  , 'bellhop model'


This implementation replicates the paper’s UWA-OFDM system and CRNet channel estimation, enabling direct validation of the proposed methodology.
