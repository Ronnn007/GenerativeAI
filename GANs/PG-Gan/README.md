### Progressive Growing of GANs (PRO-GAN)
##### Continuing the progress of the Image Generative process in this repository, next I have utilised Pro-Gan model architecture [1] to generate new faces using the CELEB-A HQ dataset.
#
As the name suggests, this Generative Adversarial Network (GAN) progressively constructs itself over the course of training. Beginning with Low resolutions of 4x4  -> 8x8 -> 16x16 and progressing towards 1024 x 1024 of resolution. The authors of Pro-Gan [1] introduce several training methodologies that tackle challenges faced by training a GAN network.

##### These are:
Equalised learning rate:  during weight initialisation for each layer scale, the weights accordingly to input and output channels, which results in normalising the gradients.

Mini-batch Standard Deviation : calculates the Standard Deviation of the feature map during batch training, appending the information before the classification layer. This enables Discriminator to make better judgements between real and fake images and overall results in more diversity.

Pixel-wise Feature Vector Normalisation: a normalization technique introduced in Generator and Discriminator that ensures that Mean and Standard deviation of feature vectors are consistent throughout training.

The Wasserstein Loss or W-Loss with Gradient Penalty (GP): a single objective function used to train this model. Here, the Generator aims to minimise the W-Loss, and the discriminator aims to maximise it, whilst also trying to satisfy the Lipschitz constraint implemented by GP. Overall, this leads to better convergence and more stable training, as proposed by [3].

The W-Loss function on its own is defined as [3]:

>> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <img src="https://latex.codecogs.com/svg.image?\mathcal{L}_{\text{Wass}}(P,Q)=\sup_{\|f\|_L\leq&space;1}\left(\mathbb{E}_{x\sim&space;P}[f(x)]-\mathbb{E}_{y\sim&space;Q}[f(y)]\right)" alt="W-LOSS" width="400"/>

Training this original W-Loss function can be challenging as it requires the critic or discriminator function (f) to be 1-Lipschitz continuous throughout the training process, which is in fact difficult to achieve.

Whereas Gradient Penalty enforces this by encouraging the critic to remain close to 1-Lipschitz continuously during training, which makes this the overall loss function: 

>> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <img src="https://sassafras13.github.io/images/2020-08-04-Wasserstein-eqn6.png" alt="W-loss with Gradient Panelty" width="500"/>

The gradient penalty term penalises the critic when the norm of its gradient becomes significantly larger than 1. This helps the critic maintain a smoother decision boundary and improves the training stability of WGAN.
#### Results:

Following this, here are the  faces generated using the PG-Gan model. The training methodology and model code can be found at:
[PG-GAN Model.ipynb](https://github.com/Ronnn007/GenerativeAI/blob/main/GANs/PG-Gan/PG-GAN%20Model.ipynb)

#### Here is the progressive growing result of the faces generated at each resolution starting at 4x4 until 256x256:

<img src="https://github.com/Ronnn007/GenerativeAI/blob/main/GANs/PG-Gan/results/generated_faces(300px).gif" width="500" />

#### Finally here are some selected results that truly showcase the model and its capabilities:

<img src="https://github.com/Ronnn007/GenerativeAI/blob/main/GANs/PG-Gan/results/Results_grid.png" alt="Results png" width="700" />

#### References

    1.Karras T, Aila T, Laine S, Lehtinen J. Progressive Growing of GANs for Improved Quality, Stability, and Variation [Internet]. arXiv.org. 2017 [cited 2024 Aug 21]. Available from: https://arxiv.org/abs/1710.10196

    2.aladdinpersson. Machine-Learning-Collection/ML/Pytorch/GANs/ProGAN at master · aladdinpersson/Machine-Learning-Collection [Internet]. GitHub. 2020 [cited 2024 Aug 21]. Available from: https://github.com/aladdinpersson/Machine-Learning-  Collection/tree/master/ML/Pytorch/GANs/ProGAN

    3.Gulrajani I, Ahmed F, Arjovsky M, Dumoulin V, Courville A. Improved Training of Wasserstein GANs. arXiv:170400028 [cs, stat] [Internet]. 2017 Dec 25 [cited 2024 Aug 21]; Available from: https://arxiv.org/abs/1704.00028
