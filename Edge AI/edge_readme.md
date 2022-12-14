# EDGE AI (TinyML) with Helium

## What is EDGE AI?

Edge AI software is large number of machine learning algorithms that run on a physical hardware device. The idea is to run AI algorithms on a local device or machine. Edge AI software allows users to get data in real-time because it does not need other systems or internet connections to connect to others.

## Why EDGE AI

We can only expext that a small percentage of lions/livestock will be equipped with GPS/BLE collar.
Therefor we had to look into other technologies that can integrate with our proposed Helium/Tracking-Solution.

Latest sensor developments make it possible to run AI models on the sensor and transmit results via LoRa. This means that for a vision sensor, not the full video/image needs to be transmitted anymore and we can use the deployed Helium-Network. Battery life is expected to last months. 

## Privacy

With images beeing analysed on devices and only resulting data beeing send to cloud platforms, possible privacy concerns are beeing mitigated. 

## What we did so far

We trained a simple image classification model to identify lions, hyenas, zebras and some more examples.

#### Go to [Arduino Library](https://github.com/blck-iot/SNIxKWT/tree/main/Edge%20AI/wildlife-1_inferencing)

#### Test the model with your phone:

<img width="159" alt="QR Code to run model on phone" src="https://user-images.githubusercontent.com/42295932/199683881-3b0288b6-3284-402c-9eeb-755382e3ac04.png">


## Next Steps

[ ] Create object detection model  
[ ] Train/Test in real environment with mobile phones  
[ ] Deploy first test sensor [SenseCapA1101](https://www.seeedstudio.com/SenseCAP-A1101-LoRaWAN-Vision-AI-Sensor-p-5367.html)  
