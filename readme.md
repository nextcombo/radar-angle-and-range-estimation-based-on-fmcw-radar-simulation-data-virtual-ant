# Radar Angle and Range Estimation Based on FMCW Radar Simulation Data: Virtual Antenna Array Beam Direction Estimation [with MATLAB Code]

FMCW radar is widely used in automotive radar systems. The fundamental idea of FMCW radar is to generate a linear frequency ramp as the transmit signal. The difference frequency (i.e., beat frequency) is determined after downconversion. Performing an FFT on the beat frequency signal allows for recognition of targets at different distances and speeds. The growing demand for safety features has led to the estimation of arrival direction (DOA) to distinguish closely located targets. Therefore, this project investigates the angle estimation problem for 77GHz FMCW automotive radar simulation data. Specifically, we study the performance of FFT, MUSIC, and compression-sensing in the angle estimation task, finding that although FFT is the fastest algorithm, it has very poor angle resolution compared to other super-resolution algorithms.
Millimeter wave technology has great applicability in automotive radar. Typical frequencies for millimeter wave automotive radar are 76-81 GHz. High frequencies allow for small antennas that can be installed behind the bumper of a vehicle. Additionally, the wide available bandwidth enables high target distance resolution [1]. Advanced automotive radars transmit FMCW at millimeter wave frequencies, which allows for high-resolution target distance and velocity estimation at a cost lower than LiDAR technology. Autonomous driving requires automotive radars with high angle discrimination capabilities. Using a large antenna array will improve angle resolution, but the resulting large package size will make integration on vehicles difficult. Although for traditional phased array radars, small package sizes mean low angle resolution, for MIMO radars [2], package size is not a limiting factor. This is because MIMO radars can use only a few transmit and receive antennas to synthesize a virtual array with a large aperture. Automotive MIMO radars use FMCW waveforms along with some mechanism ensuring orthogonality between waves.
Through this semester's project, we designed an FMCW radar simulator operating at 77 GHz and a bandwidth of 150 MHz. We utilized the unit average constant false alarm rate on the distance-Doppler frequency shift plot to identify two different stationary moving targets. Subsequently, three methods for DoA estimation were employed: based on FFT, MUSIC, and compression-sensing.
In Section II, the FMCW radar simulation was detailed. Section III introduced CA-CFAR detection. Section IV discussed the DoA estimation methods based on FFT, MUSIC, and compression-sensing. Finally, Section V concluded the paper.
FMCW Radar Simulation
Fig. 1. Frequency modulated continuous wave radar beep. Using 2-D FFT for distance and Doppler estimation.
Figure 2. Transmission, reception, and hybrid signal.
Figure 3. Distance profile (left) and distance-Doppler plot (right). See Figure 4. CA-CFAR detection located targets at 50 and 100 meters with radial velocities of 10 and -15 meters/second respectively.
Figure 5. TX and RX antenna positions.
See Figure 6. Estimating target angles using FFT.
See Figure 7. Estimating target angles using MUSIC.
See Figure 8. The distance-angle plot calculated by the MUSIC algorithm.
See Figure 9. Estimating target angles using CS.
MATLAB Code
clear; clc; close all\n​\n%% Radar parameters\nc = physconst('LightSpeed'); %speed of light\nBW = 150e6; %bandwidth\nfc = 77e9; % carrier frequency\nnumADC = 256; % # of adc samples\nnumChirps = 256; % # of chirps per frame\nnumCPI = 10;\nT = 10e-6; % PRI \nPRF = 1/T;\nF = numADC/T; % sampling frequency\ndt = 1/F; % sampling interval\nslope = BW/T;\nlambda = c/fc;\nN = numChirps*numADC*numCPI; % total # of adc samples\nt = linspace(0,T*numChirps*numCPI,N); % time axis, one frame\n% t= 0:dt:dt*numADC*numChirps-dt;\nt_onePulse = 0:dt:dt*numADC-dt;\nnumTX = 1;\nnumRX = 8;\nVmax = lambda/(T*4); % Max Unamb velocity m/s\nDFmax = 1/2*PRF; % = Vmax/(c/fc/2); % Max Unamb Dopp Freq\ndR = c/(2*BW); % range resol\nRmax = F*c/(2*slope); % TI's MIMO Radar doc\nRmax2 = c/2/PRF; % lecture 2.3\ndV = lambda/(2*numChirps*T); % velocity resol, lambda/(2*framePeriod)\nd_rx = lambda/2; % dist. between rxs\nd_tx = 4*d_rx; % dist. between txs\n​\nN_Dopp = numChirps; % length of doppler FFT\nN_range = numADC; % length of range FFT\nN_azimuth = numTX*numRX;\nR = 0:dR:Rmax-dR; % range axis\nV = linspace(-Vmax, Vmax, numChirps); % Velocity axis\nang_ax = -90:90; % angle axis\n​\n%% Targets\n​\nr1_radial = 50;\ntar1_angle = -15;\nr1_y = cosd(tar1_angle)*r1_radial;\nr1_x = sind(tar1_angle)*r1_radial;\nv1_radial = 10; % velocity 1\nv1_y = cosd(tar1_angle)*v1_radial;\nv1_x = sind(tar1_angle)*v1_radial;\nr1 = [r1_x r1_y 0];\n​\nr2_radial = 100;\ntar2_angle = 10;\nr2_y = cosd(tar2_angle)*r2_radial;\nr2_x = sind(tar2_angle)*r2_radial;\nv2_radial = -15; % velocity 2\nv2_y = cosd(tar2_angle)*v2_radial;\nv2_x = sind(tar2_angle)*v2_radial;\nr2 = [r2_x r2_y 0];\n​\ntx_loc = cell(1,numTX);\nfor i = 1:numTX\n   tx_loc{i} = [(i-1)*d_tx 0 0];\n   scatter3(tx_loc{i}(1),tx_loc{i}(2),tx_loc{i}(3),'b','filled')\n   hold on\nend\n​\nrx_loc = cell(1,numRX);\nfor i = 1:numRX\n   rx_loc{i} = [tx_loc{numTX}(1)+d_tx+(i-1)*d_rx 0 0];\n   scatter3(rx_loc{i}(1),rx_loc{i}(2),rx_loc{i}(3),'r','filled')\nend\n​\ntar1_loc = zeros(length(t),3);\ntar2_loc = zeros(length(t),3);\ntar1_loc(:,1) = r1(1) + v1_x*t;\ntar2_loc(:,1) = r2(1) + v2_x*t;\ntar1_loc(:,2) = r1(2) + v1_y*t;\ntar2_loc(:,2) = r2(2) + v2_y*t;\n​\n  

## Images

![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](01.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](02.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](03.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](04.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](05.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](06.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](07.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](08.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](09.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](10.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](11.jpg)
![(Radar Angle and Range Estimation)Virtual Antenna Array Beam Direction Estimation Based on FMCW Radar Simulation Data [with MATLAB Code]](12.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
