# CryptoNET
We propose an adversarial cryptographic model intended to encrypt images being transferred over an insecure communication network using a special key-image. 

## Setup
![Alt text](Images/CryptoNET.png?raw=true "Schematic of Model")

<ol>
  <li> The Plain-Image and the Key-Image are encrypted by Alice into sequence of Latent vectors (PL and KL) using a convolutional encoder. </li>
  <li> PL and KL are concatenated and passed through a neural network to create new sequence of vectors (CL) which is sent to the Bob through an insecure communication channel. KL is sent as key through a secure information channel.</li>
  <li> Bob recieves KL and CL and passes them through a feed forward neural network to get the latent representation of the Plain-Image. This representation is passed through a convolutional decoder to reconstruct the original Plain-Image. </li>
  <li> Eve is a hacker who has access to plain-Image and CL (chosen plaintext attack). Using previous data he tries to reconstruct the Plain-Image.</li>
  </ol>

## Training Procedure 
Download the following github repository
