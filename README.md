# CryptoNET
We propose an adversarial cryptographic model intended to encrypt images being transferred over an insecure communication network using a special key-image. 

## Setup
![Alt text](Images/CryptoNET.png?raw=true "Schematic of Model")

<ol>
  <li> The Plain-Image and the Key-Image are encrypted by Alice into sequence of Latent vectors (PL and KL) using a convolutional encoder. </li>
  <li> P<sub>L</sub> and K<sub>L</sub> are concatenated and passed through a neural network to create new sequence of vectors (CL) which is sent to the Bob through an insecure communication channel. K<sub>L</sub> is sent as key through a secure information channel.</li>
  <li> Bob recieves K<sub>L</sub> and C<sub>L</sub> and passes them through a feed forward neural network to get the latent representation of the Plain-Image. This representation is passed through a convolutional decoder to reconstruct the original Plain-Image. </li>
  <li> Eve is a hacker who has access to plain-Image and C<sub>L</sub> (chosen plaintext attack). Using previous data he tries to reconstruct the Plain-Image.</li>
  </ol>

## Training Procedure 
Download the following github repository
```terminal
git clone https://github.com/avinsit123/CryptoNET.git
cd CryptoNET
virtualenv mypython
source mypython/bin/activate
```

Download the Dataset. For training and testing purposes, we have currently used the <a href="http://www.vision.caltech.edu/Image_Datasets/Caltech101/">Caltech-101 Dataset</a> with 101 different types of objects having an average of  50 images per class.

```terminal
cd Dataset
wget http://www.vision.caltech.edu/Image_Datasets/Caltech101/101_ObjectCategories.tar.gz
gunzip /content/101_ObjectCategories.tar.gz
tar -xvf /content/101_ObjectCategories.tar
cd ../
```
![Alt text](Images/3ean3l.gif?raw=true "Schematic of Model")
First, we train an autoconvolutional encoder with an <b>Encoder-Decoder</b> mechanism to reconstruct images in the Caltech101 Dataset. 

```terminal
python train_autoencoder.py -batch_size 32 -lr 0.001 -n_epochs 10 -dataset_path Dataset/101_ObjectCategories -chkpt_file ConvAutoEncoder.pth
```

The autoconvolutional encoder is split into its Encoder and Decoder parts with the Encoder portion being sent to the Alice and the Decoder portion sent to Bob. Next we train the feed forward neural networks on the reciever and sender sides.

```terminal
python train_ffn.py -batch_size 32 -lr 0.001 -n_epochs 5 -dataset_path Dataset/101_ObjectCategories -convencoder_path ConvAutoEncoder.pth -deep_FFN_path FFN.pth
```
The feed forward neural network learns to mix the key and image on Alice's side  and retrieve this image vector on Bob's side. Now we train Eve to reconstruct the original image from the ciphertext using his own convolutional network.

```terminal
python train_Eve.py -batch_size 32 -lr 0.001 -n_epochs 5 -dataset_path Dataset/101_ObjectCategories -convencoder_path ConvAutoEncoder.pth -deep_FFN_path FFN.pth -Eve_checkpt Eve.pth 
```
Now finally we need to adversarially train the FFN and Eve jointly.

### Adversarial Training
First, we train freeze Eve's parameters, and train the FFN in with the aim to minimize the following loss function:
   L1<sub>adv</sub> = L(Alice<sub>Image</sub>,Bob<sub>Image</sub>) - L(Alice<sub>Image</sub>,Eve<sub>Image</sub>) 

Next we freeze FFN paramters, and train Eve to minimize the loss function
   L1<sub>adv</sub> = L(Alice<sub>Image</sub>,Eve<sub>Image</sub>) - L(Alice<sub>Image</sub>,Bob<sub>Image</sub>)  
