## Variational Autoencoder with Mnist
In this architecture the encoder takes the input MNIST images and maps them to a distribution in the latent space and the decoder reconstructs images from samples in the latent space. 
### Model training details:
* LR = 0.0005
* BATCH_SIZE = 32
* EPOCHS = 20
* LATENT_DIM = 2
  
Reconstruction loss indicates the model’s ability to reconstruct the input and Mean Squared error is used for this :

![MSE](https://latex.codecogs.com/svg.latex?\text{MSE}=\frac{1}{N}\sum_{i=1}^{N}\left\|\text{Reconstructed}_i-\text{Original}_i\right\|^2)

Kl divergence on the other hand regularizes the latent space whilst comparing the latent space distribution to the ground truth images:
![KL Divergence](https://latex.codecogs.com/svg.latex?\text{KL&space;Divergence}=-\frac{1}{2}\sum_{i=1}^{K}\left(1&plus;\log(\sigma_i^2)-\mu_i^2-\sigma_i^2\right))


### Training Loss graphes:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Training%20loss%20with%20Kl%20divergence.jpg)

During training these losses were monitored per batch and the following graphs show how well the model is able to reconstruct input images and how close the latent space distribution is to the desired distribution.

### Scatter plot:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Latent%20Space%20Scatter%20Plot.jpg)

A scatter plot of points in the latent space was generated, visualizing the distribution of encoded representations.

### Generated Images:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Generated%20Images.jpg)

Finally sample digits were generated from random points in the latent space. These generated images showcase the model's ability to create new, realistic digit images.

## VAE With CELEBA Dataset
A model with similar architecture was also trained using the Celeba dataset.
Here are few samples from the dataset, different images from various categories (40 total)

![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/celeba%20image%20samples.jpg)

Rest of the model training hyperparameters were the same as during training of the MNIST Dataset, However the following were adjusted:
* LR = 0.0001
* LATENT_DIM = 200

### Training Loss graph:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Training%20loss%20with%20Kl%20divergence-%20Celeba.jpg)

### Generated Images:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Generated%20Images%20-%20Celeba.jpg)

## BETA-VAE
Utilising a Standard Normal Distribution for latent space can lead to entangled representations where multiple latent variables influence same features for reconstructed image. Inorder to encourage sparsity and disentanglement a β hyperparameter is introduced within the latent space.
#### The overall loss function now introduces this Beta Hyperparameter 
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Loss%20function.jpg)

Where β > 1 adds additional penalising to (i),(ii) and (iii) and  forces compression of z at the expense of reconstruction.
#### To simplify now the loss function has changed to:
![Loss](https://latex.codecogs.com/gif.image?\large&space;\dpi{110}L_{B-VAE}=L_{Recon}&plus;\beta\times&space;L_{KL})
#### For the experimentation below, these are the Hyperparameter values:
* LR = 0.0001
* BATCH_SIZE = 32
* EPOCHS = 50
* LATENT_DIM = 400
* β = 5

### Training Loss Graph:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Beta-Training%20plots.jpg)

### Finally, The generated Images:
![](https://github.com/Ronnn007/GenerativeAI/blob/main/VAE/Graphs/Beta-%20Generated%20images.jpg)
We can notice a significant difference within facial expressions compared to previous VAE generated samples. Additionally, we can also notice further details in terms of quality.
