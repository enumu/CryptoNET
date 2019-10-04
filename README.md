# CryptoGAN
We propose a GAN based encryption model trained using Adversarial Learning. It consists of 3 convolutional autoencoders namely Alice ,Bob and Eve. Alice encrypts the image into a cipherimage using an image-key and sends it to Bob along with the image-key. Using the image-key, Bob decrypts the Cipher-Image using the image-key to retain the original image sent by Alice. Eve is an attacker who can intercept the Cipher Image and tries to obtain the image sent by Alice.


## Steps to Project
<ol>
  <li> Dataset Collection</li>
  <li> Dataset Preprocessing </li>
  <li> Network Desigining </li>
  <li> Training </li>
  <li> Evaluation and Ablation Study </li>
  <li> Code Refactoring </li>
