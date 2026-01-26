# Frequency mixer Project

## Introduction

This python notebook implements a frequency based image fusion system to combine high-frequency features (fine details) from a cat image with low-frequency features (overall structure) from a dog image . The project uses the 2D Discrete Fourier Transform to analyze spatial frequencies, visualize magnitude spectra, study rotation effects, and fuse images using a frequency mixer. I explored multiple filtering approaches, with the Gaussian filter providing the best result due to its smooth frequency separation.

## Mathematical Info: 2D DFT (DISCREET FOURIER TRANSFORM)

The 2D DFT for an image $ I(x, y) $ of size $ M \times N $ is given by:

$$
F(u, v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} I(x, y) \cdot e^{-j2\pi \left( \frac{ux}{M} + \frac{vy}{N} \right)}
$$

The inverse transform is:

$$
I(x, y) = \frac{1}{MN} \sum_{u=0}^{M-1} \sum_{v=0}^{N-1} F(u, v) \cdot e^{j2\pi \left( \frac{ux}{M} + \frac{vy}{N} \right)}
$$

The magnitude spectrum is:

$$
S(u, v) = |F(u, v)|
$$

The dB spectrum is:

$$
S_{dB}(u, v) = 20 \log_{10} (|F(u, v)| + \epsilon)
$$

where $$ ( \epsilon ) $$ is a small constant to avoid log(0).

## Steps

1. **Load and Preprocess the Images**:
   - Load grayscale images using PIL.
   - Display the cat image.

2. **Compute 2D DFT and Magnitude Spectra**:
   - Apply `np.fft.fft2` to compute the 2D DFT of the cat image.
   - Shift the spectrum using `np.fft.fftshift` to center low frequencies.
   - Compute and display normal and dB magnitude spectra.

3. **Analyze Rotation Effects**:
   - Rotate the cat image $90^\circ$ counterclockwise using `np.rot90`.
   - Compute the 2D DFT and magnitude spectra of the rotated image.
   - Display spectra, observing a $90^\circ$ rotation in the frequency domain.

4. **Frequency Mixer**:
   - Fuse high frequencies (details) from the cat image with low frequencies (structure) from the dog image.
   - Tested square, circular, and bilateral filters for fusion, noting artifacts and inefficiencies, with the Gaussian filter as the final approach.
   - Display results.


## Reasoning

- **Square/Circular Filters**: Frequency domain approaches were good but introduced artifacts due to sharp cutoffs.
- **Bilateral Filter**: Preserved edges but computationally heavy and poor at frequency isolation.
- **Gaussian Filter**: Has smooth, artifact-free filtering, computationally efficient in the spatial domain, and easily tunable via $\sigma$.
