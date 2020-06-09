# Radar-Target-Generation-and-detection-SensorFusionND
Target generation and detection using Matlab
## Description
this project is one of sensor fusion Nano-degree projects.
** I implemented five major tasks ** :
1. Configure the FMCW waveform based on the system requirements.
2. Define the range and velocity of target and simulate its displacement.
3. process the transmit and receive signal to determine the beat signal.
4. Perform Range FFT on the received signal to determine the Range.
5. perform the CFAR processing on the output of 2nd FFT to display the target.
![req](https://i.ibb.co/tsyHz4r/image11.png)

### Results
1. after performing 1st FFT

![fft1](https://i.ibb.co/K2MwtjJ/2.png)

2. the second FFT

![f_f_t](https://i.ibb.co/jTSpWwr/3.png)

3. 2D-CFAR
- first, i choosed CFAR parameters which are training and guard cells
- then , offset the threshold
- i created a vector to store noise level each iteration
- i designed a loop to slide  Cell Under Test (CUT) across the complete cell matrix
- through every iteration we calculate the signal level within the training cells
- then i added the offset to determine the threshold
- if CUT level < 1 ground assign the value to 0 else assign to 1
- we use RDM[x,y] as a matrix for implementing CFAR
- if the value is not 0 or 1 assign it to 0
- display the 2D-CFAR
![CFAR](https://i.ibb.co/gwbVHCC/1.png)
### System requirements
matlab or octave setup
