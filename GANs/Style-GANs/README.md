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
The artifacts droplets found in StyleGAN were pinpointed to the AdaIN operation by the authors, hence this operation is revisited.
#
#### Key innovations:
Style Modulation and Demodulation [5]

<img src="https://miro.medium.com/v2/resize:fit:726/1*yA9loND0aVvMNDT62lfwqg.png" width="500"/> <img src="https://miro.medium.com/v2/resize:fit:730/1*RW7QRBRuWiGrj-l9iY5xIg.png" width="500"/>
#### Changes #1 Revised Architecture
The typical line of operations for AdaIN are Normalisation and Demodulation and both operations operate on mean and standard deviation per feature map. 
- Within each style block or AdaIN operation that is active, the authors [2] find that moving the bias and noise operations outside the style block makes the result more predictable (Revised Architecture).
  
- Where these operations operate on normalised data. Additionally, with this change, it is sufficient for Normalisation and Demodulation to operate with Standard deviation alone, excluding the mean.

#### Changes #2 Weight Modulation & Demodulation
The modulation or normalisation scales each input feature map of the convolution based on the incoming style, and for each convolutional layer, the weights 𝑊 are modulated by the style vector 𝑤 before the convolution operation. 

For example [5]
<img src="https://miro.medium.com/v2/resize:fit:754/1*ydbNXYzaFrmni_vcxljc-g.png" width="500"/>

Changes 
- First the Mod std and Conv operations are combined to be W′ = W ⋅ diag(s(w))
- And the Norm std becomes weight demodulation.

The modulation scales each input feature map of the convolution based on the incoming style, which can alternatively be implemented by scaling the convolution weights: w’ᵢⱼₖ = sᵢ ⋅ wᵢⱼₖ
  - w : original weights
  - w’ : modulated weights
  - sᵢ : the scale corresponding to the ith input feature map
  - j, k : spatial indices of the output feature maps

The demodulation operation helps maintain consistent and stable activations, especially after the modulation operation. 
- Stable activation overall help prevent artefacts from the generated images.
- This is revised and implemented as: <img src="https://miro.medium.com/v2/resize:fit:351/1*9_hEaAn0L8LpEzbJnX31jQ.png" width="250"/>
- Where 𝑊′′ are the demodulated weights, and 𝜖 is a small constant to prevent division by zero.
- This is what it looks like in practice [5] : <img src="https://miro.medium.com/v2/resize:fit:496/1*wVRMFwx5lgMoiB8ep_eaJQ.png" width="300"/>

#### Changes #3 Changes within the Architecture
Additionally, the authors also revisited and reviewed the progressive growing technique for training StyleGan2 model. 

It was discovered that progressive growing has a strong location preference. 
- For example, the authors showcase features such as teeth or eyes do not move smoothly over the image. Where even though a face is generated from different angles, the teeth are static throughout rather than accompanying the angel of generation.
- The authors also identify that skip connections within the generator helps configure the Perceptual Path Length regularisation and the residual connections benefit the FID regularisation. Hence, within StyleGan2, the generator utilises skip connections, and the discriminator uses residual connections to benefit both regularisation scores.

#### References
      [1] 1.Karras T, Laine S, Aila T. A Style-Based Generator Architecture for Generative Adversarial Networks [Internet]. arXiv.org. 2018 [cited 2024 Aug 21]. Available from: https://arxiv.org/abs/1812.04948
      [2] 2.Karras T, Laine S, Aittala M, Hellsten J, Lehtinen J, Aila T. Analyzing and Improving the Image Quality of StyleGAN. arXiv:191204958 [cs, eess, stat] [Internet]. 2020 Mar 23 [cited 2024 Aug 21]; Available from: https://arxiv.org/abs/1912.04958
      [3] christianversloot. machine-learning-articles/stylegan-a-step-by-step-introduction.md at main · christianversloot/machine-learning-articles [Internet]. GitHub. 2022 [cited 2024 Aug 21]. Available from: https://github.com/christianversloot/machine-learning-articles/blob/main/stylegan-a-step-by-step-introduction.md
      [4] 4.lernapparat. lernapparat/style_gan/pytorch_style_gan.ipynb at master · lernapparat/lernapparat [Internet]. GitHub. 2024 [cited 2024 Aug 21]. Available from: https://github.com/lernapparat/lernapparat/blob/master/style_gan/pytorch_style_gan.ipynb
      [5] Steins. StyleGAN vs StyleGAN2 vs StyleGAN2-ADA vs StyleGAN3 [Internet]. Medium. Medium; 2022 [cited 2024 Aug 21]. Available from: https://medium.com/@steinsfu/stylegan-vs-stylegan2-vs-stylegan2-ada-vs-stylegan3-c5e201329c8a
