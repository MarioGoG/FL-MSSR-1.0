# FL-MSSR-1.0
A code for enabling Mean-Shift Super-Resolution microscopy within a single frame of FLIM images it includes phasor analysis

Background:
Fluorescence Lifetime Imaging Microscopy is a contrast technique that gives additional insight to a microscopy image by adding the information about changes in the fluorescence lifetime of a fluorophore residing in a particular microenvironment. Nevertheless, when two fluorophores are near each other at a distance on the order of the diffraction limit, there exists a limitation in the precise localization of each fluorophore as their emission undergo spatial distortion due to the signal being convolved with the Point Spread Function (PSF) of the optical system used. This inherent distortion of nanoscopic features in a FLIM image can be understood in terms of the fundamentals of frequency-domain FLIM (FD-FLIM). Briefly, the FD-FLIM image results from a Fourier analysis of the emission signal as a response to the excitation light. The emission signal is always phase delayed and demodulated in contrast to the excitation light. The former is a direct consequence of the fluorescence lifetime, while the latter depends on the excitation rate of the fluorophore. This implies a loss of information when performing a subpixel analysis, mathematically speaking the signal from two fluorophores will overlap, and thus assigning a photon to a particular fluorophore is thus far from trivial because the larger the fluorescence lifetime, the larger the demodulation will be and thus the lesser the distinguishability of the overlapping signals will be within the limits of diffraction [1].

To address this issue, we propose to use a computational deconvolution methodology, developed in the National Laboratory for Advanced Microscopy (UNAM, Mexico), based on the Mean Shift Theory, named Mean-Shift Super-Resolution (MSSR). MSSR assumes that fluorescence images come from signals collected from individual fluorophores that are convolved with the microscope's point spread function (PSF). To recover the raw intensity distribution, MSSR computes the Mean Shift (MS), which is a blurring process that sharpens the image by utilizing the spatial information contained in the PSF. By calculating the MS, large intensity values in the diffraction-limited image coincide with large positive values in the MSSR image. After algebraic transformations remove possible artifacts, the result is an image with a narrower full width at half maximum (FWHM) that extends spatial information beyond the Sparrow limit [2].

With MSSR it is possible to find the position of the maximum intensity centroids given the MS vector gradient. Given the fact that those centroids with the shorter fluorescence lifetimes give the higher intensity peaks, first we can mask these short lifetime centroids and, in a posterior analysis, find with the same algorithm the centroids from the larger lifetime fluorophores. Consequently, this method (MSSR-FLIM) could, in principle, improve the precision of the localization of a fluorophore given its residency environment avoiding the inherent deformations of a FLIM image due to traditional lifetime analysis. It is worth noting, also, that MSSR is a computational algorithm that can, in principle, work with ine image frame only, extending the resolution to the order of tenths of nanometer, still below the diffraction limit. Thus, diminishing the exposing times when imaging and reducing, therefore, photobleaching and photodamage of the samples imaged.

References:
1. Yuan, X., Mannam, V., Zhang, Y., & Howard, S. (2021, March). Overcoming the fundamental limitation of frequency-domain fluorescence lifetime imaging microscopy spatial resolution. In Single Molecule Spectroscopy and Superresolution Imaging XIV (Vol. 11650, pp. 34-40). SPIE.
2. Torres-García, E., Pinto-Cámara, R., Linares, A., Martínez, D., Abonza, V., Brito-Alarcón, E., ... & Guerrero, A. (2022). Extending resolution within a single imaging frame. Nature Communications, 13(1), 7452.

This project:

1. The archive named 25-01-2023.ipynb is an FD-FLIM simulator with MSSR: 
   It was defined as a function that builds two Gaussian PSFs modified by the modulation from the fluorescence lifetime of two fluorophores according to the FD- 
   FLIM fundamentals. It includes the single-frame MSSR code in order to enhance the resolution of the PSF images.
2. The archive named 02-02-2023.ipynb is an FD-FLIM simulator with MSSR with the distance in terms of sigma instead of nanometers:
   The code from the FD-FLIM-MSSR simulator was modified in order to plot the figures in terms of sigma and not nanometers. So that, calculations can be done for      comparing the Rayleigh and Sparrow limits.
3. The archive named 02-15-2023.ipynb is the first attempt to Phasor Plot analysis:
   The equations for converting the fluorescence lifetimes to the phasor space were programmed. For the modeling, it was implemented a built-in function of cross-        correlation.
4. The archive named 03-30-2023.ipynb contains a possible solution for the issue in the above archive of calculating the fluorophores' lifetimes within the phasor      circle and not in the boundaries. This solution was provided by ChatGPT-4.
5. The archive named FLIM-MSSR.ipynb is the complete first version of this project. This code provides a FLIM simulator for two independent, non-interacting 
   fluorophores separated at a diffraction-limited distance, in particular, at the Sparrow limit (2*sigma) but the distance can be varied by the user. It generates 
   the 2D FLIM image by convolving the lifetime-modulated intensity of the fluorophores with a Gaussian PSF.
   Moreover, this simulation includes the phasor analysis of the image to visualize the fluorescence lifetime distribution of
   the fluorophores interacting with the PSF.
   Finally, with the insertion of the MSSR code (https://github.com/MSSRSupport/MSSR) one can perform the resolution enhancement of
   the generated image. For comparison, it performs deconvolution by Wiener and Richardson-Lucy (RL) algorithms.
   The variables that can be changed by the user are:
   a. The fluorescence lifetimes of both fluorophores
   b. The distance (in terms of sigma) at which the fluorophores are to be positioned
   c. The period of the exciting light (thus modifying the time bin of the fluorescence decay measurement)
   d. The number of MSSR orders (from 0 to 3)
   e. The number of iterations for the RL deconvolution algorithm (by default we use 2)
