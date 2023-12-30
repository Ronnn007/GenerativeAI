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
