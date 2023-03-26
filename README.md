# Transmitter_and-Receiver_Simulation_in_Digital_Communication

## ***Step #1: Generating Raised-Cosine Pulse***
Consider the following parameters:

![image](https://user-images.githubusercontent.com/125180530/227770773-7aeee6d2-1feb-455d-8b2c-1f1a05703359.png)

The definition of a raised-cosine pulse is:

![image](https://user-images.githubusercontent.com/125180530/227771137-437e1d6f-33f3-49aa-a4b0-bc29c29dd095.png)

One point to be mentioned is that the value of the pulse in the following times should be as it's in the below formula:
![image](https://user-images.githubusercontent.com/125180530/227771348-73a43131-9af1-4f9e-890b-4de7d54da8d4.png)

Now we generate three different pulses. In this case, the influence of sampling error can be shown better and we can make a better comparison in this way.

#### First Pulse:
Generate an ideal pulse (which means that we have no error for sampling) in [-6T,6T].

#### Second Pulse:
Consider the sampling error to be 0.1T. So, we should generate the pulse in [-6.1T,5.9T].

#### Third Pulse:
Consider the sampling error to be 0.2T. So, we should generate the pulse in [-6.2T,5.8T].

The resulting pulses are shown in the following figure:
![image](https://user-images.githubusercontent.com/125180530/227771464-cdd76b5a-ab7b-409d-bae2-4efd0404608b.png)

## ***Step #2: Generating Transmitted Signal***
By using the 'randi' function generate a vector called 'bits' which is contained in zeros and ones with N = 100000. Then create 'modulated_symbols' which is a vector of -1 and 1. -1 corresponds to every zero in 'bits' and 1 corresponds to every 1. 

Now we want to generate a transmitted signal with a raised-cosine pulse and modulated symbols. First of all, we have to upsample the modulated symbols. For this case, we add L-1 zeros between two consecutive symbols.

![image](https://user-images.githubusercontent.com/125180530/227771878-253e5998-6bdd-4747-8cd2-f5f96953b62a.png)

Then we convolve a new vector with each of the three raised-cosine pulses that we generated in the previous step. Now we have transmitted a signal. 

## ***Step #3: AWGN Channel Modeling***
The goal of this step is to evaluate the error probability of binary PAM modulation detection in an AWGN channel for different values of SNR which are as follows:

![image](https://user-images.githubusercontent.com/125180530/227772428-955652bb-c075-474a-8e06-bcee6dbd2fc3.png)

For each value of SNR_dB, we have to calculate noise power. So, consider the power consumption for each bit is 1J.

![image](https://user-images.githubusercontent.com/125180530/227772530-e3fa7f85-d402-49c8-ae04-36492de17ac2.png)

So, the value of SNR in a linear domain is:

![image](https://user-images.githubusercontent.com/125180530/227772581-89637067-ba5c-4523-8e0c-6cfb3592e1ec.png)

For noise generation, we used the 'and' function. This function generates a vector of values that have a normal distribution with:

So, to generate noise with the mentioned power, we have to multiply the noise vector by the following factor:

![image](https://user-images.githubusercontent.com/125180530/227772813-85efc8bf-af09-4b3f-b89c-d3f717680629.png)

Finally, add the noise vector to the transmitted signal's vector that is generated in the previous step to reach 'received_signal'. 

## ***Step #4: Revealing symbols***
The goal of this step is to reveal transmitted symbols with the use of the received signal. First, we have to sample the received signal with:
![image](https://user-images.githubusercontent.com/125180530/227773027-e50ba5e7-7d0e-40c2-ae9e-f0b83425ce21.png)

We call the sampled received signal 'samples'. Next, we have to determine which symbols (-1 or 1) are transmitted. As we considered -1 and 1 for 0 and 1 bits, and the probability of these bits are equal, the optimal threshold is:

![image](https://user-images.githubusercontent.com/125180530/227773237-1a8f49e6-087a-4fbe-afcf-bc30638737fc.png)

By comparing the values of 'samples' with this threshold we can determine which of the symbols (-1 or 1) is transmitted. The new vector is called 'detected_symbols'.

## ***Step #5: Error Probability Calculation***
For calculating error probability 'detected_symbols' and 'modulated_symbols' should be compared. We use the following formula to calculate the error probability:

![image](https://user-images.githubusercontent.com/125180530/227773430-11550731-905d-4114-92ed-0a82daba28c8.png)

The resulting figures show the error probability for different values of SNR_dB and for different values of sampling error. Then, think about the influence of ISI (Inter Symbol Interference)-which is for the existence of sampling error- on the error probability. 

![image](https://user-images.githubusercontent.com/125180530/227773692-2ad843fa-4f25-4df5-bce4-b0160cd13dbd.png)

![image](https://user-images.githubusercontent.com/125180530/227773697-822134a4-6319-499f-b4e7-f2b16b9199f5.png)

![image](https://user-images.githubusercontent.com/125180530/227773701-b355c7e0-4e20-447a-8d2c-86611731aade.png)

![image](https://user-images.githubusercontent.com/125180530/227773715-f92bb875-830e-4303-9146-220af47db066.png)

![image](https://user-images.githubusercontent.com/125180530/227773722-7d7cb3e3-b71d-4b97-b244-9f1772de1c45.png)

### Conclusion:
If we compare the last two figures (generated pulses) with the generated pulses in the first step, we can come to the conclusion that the greater the value of beta, the lower the width of the pulses. However, we can see that the amplitude of the side lobes becomes less. By reducing beta, the shape of pulses becomes closer to the square pulse which is the ideal state. (It means that for beta = 0, we have the ideal pulse shape). 

For greater values of beta, the amplitude of side lobes is less and it results in the reduction of ISI. So, the error also reduces with the increase of beta values as it's obvious in the above figures. 

By comparing the graphs of error probabilities we can conclude that all of them are descending. It means that an increase in SNR value results in the reduction of error values. Another point is that the greater the value of beta, the lower the value of error. 

