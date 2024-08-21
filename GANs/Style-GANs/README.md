### Exploring StyleGans
#
#### Introduction
Continuing the exploration of Generative Adversarial networks in this Project I explore the contributions from Progressive GANs to StyleGANs architectures. In doing so: I specifically discuss key innovations, functionality and showcase the architectures of StyleGANs.

Key differences and improvements between StyleGANs and previous GANs architectures:

StyleGan builds on ideas of progressive GAN by controlling image synthesis process by redesigning the generator architecture and in doing so separates the style and content of high-level attributes such as freckles and hairs which are separable in the generated images. This is achieved by injecting style vectors in the network at different points which are produced by different densely connected layers A. These layers stem from an additional Mapping network that maps the input latent code 𝑧 to an intermediate latent space 𝑤 [3].

The StyleGan2 [2] addresses minor artifacts that were produced in the generated images by the original StyleGan. In order to achieve this, the authors redesigned both the generator and discriminator architectures and reviewed the progressive growing model architecture used until now.

#### Understanding StyleGANs

<img src="https://miro.medium.com/v2/resize:fit:875/1*MPrahfIiW0zrO_yNW5ql5Q.png" width="700"/>

#### Key Innovations:

Mapping Network [1], [3]: 
- The Mapping Network is a simple 8-Layer MLP network which takes in an input latent vector z that is sampled from a normal distribution and transforms it into an intermediate latent space 𝑤. Both the original latent vector and the intermediate transformed vector 𝑤 are of 512 dimensions. This mapping network prevents sampling from a fixed distribution and the learned distribution is as disentangled as possible, providing more control over the style transfer.

AdaIN Operation [1], [3]: 
- The style vector is produced by affine transformation from the intermediate latent space 𝑤. The style vector produces two vectors which are : a bias vector y<sub>b,i</sub> and a scaling vector y<sub>s,i</sub>. These vectors act as parameters for the AdaIN operation.

To apply styles at each convolution layer,  Adaptive Instance Normalisation (AdaIN) is a normalisation technique employed by the StyleGan model.

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <img src="https://latex.codecogs.com/svg.image?\text{AdaIN}(x,\text{style})=\gamma(\text{style})\cdot\text{InstanceNorm}(x)&plus;\beta(\text{style})" width="500"/>
- Instance Norm normalises the input tensor x to have zero mean and unit variance
- Therefore x^hat is the instance normalised feature map.
- γ (style) is the scale parameter. β (style) is the shift parameters. Both derived from the style vector.
- By multiplying the normalised features with Gamma, the scale parameter can control the “spread” or “intensity” of activations. This helps capture the stylistic elements like textures and patterns.
- Also, by adding Beta with gamma, we can control the overall brightness or colour balance as the shift parameter adjusts the mean of the feature map.

Style Mixing [1], [3]:

To prevent the generator from utilising the same latent space during training, instead of a single Z two are sampled, Z<sub>1</sub> and Z<sub>2</sub> which ultimately correspond to two different style vectors W<sub>1</sub> and W<sub>2</sub>. Therefore at each layer, a random style vector is selected. This helps eliminate correlations between the vectors where the styles are disentangled as possible.


### StyleGAN 2
#
Key innovations:

