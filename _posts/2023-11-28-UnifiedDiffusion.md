# Understanding Diffusion Models : A Unified Perspective

```
이 논문은 Diffusion model에 대한 수식의 유도 과정과 수학적 해석을
step-by-step으로 알려주는 논문이다.

상세한 이해를 위해서라면 [Lillianweng의 Post](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/), [Yang song의 Post](https://yang-song.net/blog/2021/score/)
을 읽고 오는 것을 권장한다. 
``` 

## Part 1. Introduction : Definition and branches of generative model
- Intro에서는 Diffusion model을 설명하기에 앞서 생성 모델의 여러 branch를 언급한다.
- 기본적으로 `Generative model`이란, $p(x)$ 를 배워 새로운 샘플을 생성하고 Likelihood evaluation이 가능하도록 하는 것.

### GAN
- Adversarial manner로 Generator와 Discriminator를 동시에 학습 

### Likelihood-based
- 실제 관측된 데이터 샘플들에게 높은 likelihood를 부여하는 식으로 학습 
- ex) `Autoregressive model` , `Normalizing flows`, `VAE`

### Energy-based / Score-based
- Energy-based : Distribution function을 일종의 Flexible energy functiond로 표현하는 방식
- Score-based : 위의 언급한 Energy-based의 score를 추정하려는 방식

### Diffusion model
- Diffusion model은 Likelihood-based 
- 또한, Diffusion model은 Score-based 
- 이 글은 Diffusion model이 왜 2가지 해석이 모두 가능한가?를 다룬다. 


## Part 2. From ELBO to VAE and HVAE

### Data, reprsented by latent variable z
- 우리가 아는 data란 latent variable $z$에 의해 represent & generate되는 것으로 간주할 수 있음
- 이때 z의 dim이 data의 dim보다 큰지, 작은지에 따라 여러 해석이 가능함.
- 대부분의 경우 latent variable $z$의 dim이 작다고 가정함. 즉, 본래 데이터의 compressed feature를 학습하는 셈
- 그 이유중 하나는, $z$를 higher dim으로 가정하면 strong priors가 필요하기 때문임.  

### In theory, we can get likelihood. In fact it's not.
- Latent variable $z$의 정보를 알면 관찰된 data $x$의 확률을 "이론상" 구할 수 있음.
- 그러나 실제로는 불가능함. 다음과 같은 이유 때문인데 
- 확률의 성질에 의해 $p(x)$는 $p(x) = \int p(x,z)dz$ 이거나(marginalization), $p(x) = \frac{p(x,z)}{p(z|x)}$ 이다.(Chain rule)
- 전자는 모든 z의 정보를 필요로 하며, 후자는 True latent encoder인 $p(z|x)$ 를 필요로 한다.  

### That's why we rely on proxy i.e.ELBO.
- True latent encoder $p(z|x)$는 알 수 없다고 가정하는게 타당함. 그래서 보통 $\Phi$ 로 encode된 Encoder network를 덧붙임.
- 실제 최적화를 할 때는 ELBO(Evidence lower bound)의 최대화를 목적으로 함. 
- ELBO의 최대화는 본질적으로 $q_\phi$와 $p$의 KL divergence의 최소화와 동일함. 

### VAE: Loss , Implementation, and re-parameterization trick.
- VAE에 자세한 설명은 여기서 다루지 않겠음.
- VAE의 loss term은 결국 reconstruction term과 prior matching term으로 분리됌.
- 1번째 reconstruction term은 우리가 학습시킨 Encoder 분포 하에서 Data의 likelihood가 얼마나 되는가?를 나타냄. 이게 높다면 데이터를 잘 만들어낼 수 있다는거니깐.
- 2번째 Prior matching term은 우리가 학습시킨 Encoder 분포가 초기에 가정한 prior distribution $p(z)$와 최대한 가깝게 함. 특정 data point $z$에 편중되어 `mode collapse`가 일어나는 것을 막는다.

- Encoder와 Decoder의 분포는 유연하게 접근할 수 있지만, VAE의 논문저자들이 그래했듯 대부분 Encoder/ Decoder의 분포 형태를 가정시켜서 closed-form으로 목적함수를 모델링함.
- 실제 Training 시에는 몬테카를로 샘플링을 통해 데이터들을 이용하며 Probablistic한 node는 최적화가 안되기에 re-parametrization trick을 이용함.

### HVAE(Hierarchiccal VAE) : Why not stack VAE?
- VAE를 여러개 쌓는다면 어떨까?
- 직관적으로, latent variable이 더 high-level의 abstract한 latent variable에서 온 것으로 간주할 수는 없을까? 

### MHVAE : Markovian property to HVAE?
- 



## Part 3. VDM(Variant Diffusion model)

### 


### MHVAE(Markovian HVAE), and its relation to VDM





