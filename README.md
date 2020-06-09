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
![fft2](https://i.ibb.co/jTSpWwr/3.png)
3. 2D-CFAR
 1. Determine the number of Training cells
 ```
 Tr =  10;
 Td = 8;
```
2. Determine the number of Guard cells
```
Gr =  4;
Gd =  4;

```
3. offset the threshold by SNR value in dB
```
offset =  1.4 ;
```
4. Create a vector to store noise_level for each iteration on training cells
```
noise_level = zeros(1,1);
```
5. design a loop such that it slides the CUT across range doppler map by
giving margins at the edges for Training and Guard Cells.
For every iteration sum the signal level within all the training
cells. To sum convert the value from logarithmic to linear using db2pow
function. Average the summed values for all of the training
cells used. After averaging convert it back to logarithimic using pow2db.
Further add the offset to it to determine the threshold. Next, compare the
signal under CUT with this threshold. If the CUT level > threshold assign
it a value of 1, else equate it to 0.
```
RDM = RDM/max(max(RDM));

for i = Tr+Gr+1:(Nr/2)-(Gr+Tr)
    for j = Td+Gd+1:Nd-(Gd+Td)
        
       % Create a vector to store noise_level for each iteration on training cells
        noise_level = zeros(1,1);
        
        % Calculate noise SUM in the area around CUT
        for p = i-(Tr+Gr) : i+(Tr+Gr)
            for q = j-(Td+Gd) : j+(Td+Gd)
                if (abs(i-p) > Gr || abs(j-q) > Gd)
                    noise_level = noise_level + 10.^(RDM(p,q)/10);
                end
            end
        end
        
        % Calculate threshould from noise average then add the offset
        threshold = 10*log10(noise_level/(2*(Td+Gd+1)*2*(Tr+Gr+1)-(Gr*Gd)-1));
        threshold = threshold + offset;
        CUT = RDM(i,j);
        
        if (CUT < threshold)
            RDM(i,j) = 0;
        else
            RDM(i,j) = 1;
        end
    end
end
```
6. The process above will generate a thresholded block, which is smaller 
than the Range Doppler Map as the CUT cannot be located at the edges of
matrix. Hence,few cells will not be thresholded. To keep the map size same
set those values to 0.
```
RDM(union(1:(Tr+Gr),end-(Tr+Gr-1):end),:) = 0;  
RDM(:,union(1:(Td+Gd),end-(Td+Gd-1):end)) = 0;
```
7.display the CFAR output using the Surf function like we did for Range
doppler Response output.
![CFAR](https://i.ibb.co/gwbVHCC/1.png)
