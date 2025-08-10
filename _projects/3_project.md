---
layout: page
title: reconstruting atmospheric fields with conditional data assimilation 
description: "[cee506] - final project"
img: assets/img/p3_fig-framework.png
redirect: 
importance: 1
category: climate
toc:
  sidebar: left
---

<h2 id="introduction">Introduction</h2>

Developed in the 1960s, data assimilation is a concept that aims to provide an estimation of the state of a system by merging previous model estimates with present observations. This process is performed sequentially so that model parameters are adjusted dynamically to minimize error and satisfy physical constraints typically prescribed by the type of problem. Assuming a stochastic formulation of both the model and observation, we can represent them as probability density functions (PDFs).
<p>Using a Bayesian framework, the goal of data assimilation is to derive a posterior distribution
  <span class="math inline">\(p(x\mid y)\)</span>, as expressed by Bayes’ rule:
</p>

<div class="equation" id="eq-bayes_da">
  \[
    p(x\mid y)
      = \frac{p(y\mid x)\,p(x)}{p(y)},
    \tag{1}
  \]
</div>

<p>
  where <span class="math inline">\(p(x)\)</span> refers to the information before assimilation described by the prior PDF, <span class="math inline">\(p(y\mid x)\)</span> refers to the likelihood of \(y\) conditioned upon state \(x\), and
  <span class="math inline">\(p(y)\)</span> refers to the probability of the data \(y\) happening.
</p>

<p>
Since its inception, data assimilation has evolved to encompass various methodologies, including the Kalman filter (KF <a href="#ref-KalmanWeather">[3]</a>), 4DVar <a href="#ref-4DVar">[4]</a>, ensemble Kalman smoother (EnKS <a href="#ref-EnKS">[5]</a>), and hybrid forms which combine some form of the variational and ensemble Kalman filter (EnKF) method <a href="#ref-HybridEnKS">[6]</a>. Specifically in reference to weather and climate applications, data assimilation has proven successful for numerical weather prediction and ocean forecasting systems. In particular, it is used to provide the model with an initial condition that can guide the future evolution of the solution in space and time. Still, while data assimilation tools have been fruitful in some respects, they face limitations with model bias, data sparsity, and the computational demands of large-scale datasets.
</p>

<p>
More recently, diffusion models have gained prominence for tasks in image and video generation, super-resolution, and other computer vision applications. Originating from the work of Sohl-Dickstein <em>et al.</em>, who proposed a thermodynamics-inspired approach using Langevin dynamics <a href="#ref-Langevin">[2]</a>, diffusion models have since undergone significant advancements. In 2019, Song and Ermon introduced a noise-conditioned score network, trained based on matching the gradient of the training data distribution to the model itself <a href="#ref-SongErmon">[1]</a>. Subsequently, Ho <em>et al.</em> <a href="#ref-DDPM">[7]</a> refined this approach in 2020 with the introduction of denoising diffusion probabilistic models (DDPMs), building off of Sohl-Dickstein’s work and transforming the training process to achieve greater accuracy.
</p>


<h2 id="data">Data</h2>
The data utilized in this study was obtained from the European Centre for Medium-Range Weather Forecasts (ECMWF) fifth-generation reanalysis dataset (ERA5). The dataset provides global coverage at a horizontal resolution of 0.25° × 0.25°. For this analysis, 13 vertical pressure levels were selected: 1 hPa, 50 hPa, 100 hPa, 300 hPa, 500 hPa, 700 hPa, 800 hPa, 850 hPa, 900 hPa, 950 hPa, and 1000 hPa. Data was sampled for five specific days every month (the 1st, 7th, 14th, 21st and 28th) at three-hour intervals spanning 00:00 to 21:00 UTC. The timeline of this dataset ranged from 2021 to 2023. 

The ERA5 dataset was retrieved from the Climate Data Store implemented by ECMWF as part of the Copernicus Earth Observation Program. A few preprocessing steps were performed to prepare the raw data for input into the guided diffusion model, which are described in greater detail in the following paragraph.

First, altitude was computed based on Eqn. \ref{eq:alt} below:
$$
\begin{equation}\label{eq:alt}
    \text{alt} = R_e\cdot h/(R_e-h),
\end{equation}
$$
where $$R_e$$ represents radius of the Earth and $$h$$ represents the geopotential height.

Since only 11 vertical levels were downloaded, remaining "gaps" between the vertical levels were linearly interpolated using $$\texttt{RegularGridInterpolator}$$ from the $$\texttt{scipy.interpolate}$$ library. To accommodate the computer vision-based algorithms used in the guided diffusion and RePaint models, all data was converted into RGB images. In the 2D case, 256x256 images were generated covering the extent of North America. In the 3D case, 64x64x64 cubes were generated covering the extent of the state of California. The same extent was applied for 4D, but at dimensions of 8x32x32x32, corresponding respectively to time, latitude, longitude, altitude. The decreasing spatial granularity in higher-dimensional representations was mainly motivated by computational constraints.

The variables of interest included relative vorticity ($$v_o$$), specific humidity ($$q$$), and the $$u-$$component of wind ($$u$$). These were arbitrarily chosen after a few visualizations of other raw data variables, mostly due to the fact that they exhibited substantial variability, providing an opportunity to test the accuracy and robustness of the proposed framework. While this study focused on these specific variables, the proposed theoretical framework for guided diffusion and RePaint can be generalized to other spatiotemporal datasets. 

<h2 id="methods">Methods</h2>
Our framework follows three steps: a training phase (following <a href="#ref-improved-diffusion">[9]</a>), a mask generation phase, and an inpainting phase (following <a href="#ref-RePaint">[10]</a>).

<h3>Diffusion Models</h3>
To understand how we can train diffusion models to denoise, we provide a formulation of Gaussian diffusion models as given by the seminal paper by Ho et. al. We are given an initial $$d-$$dimensional image drawn from some probability distribution $$q(x)$$: $$x_0 \in \mathbb{R}^{n_1\times n_2 \times \cdots \times n_d} \sim q(x)$$, represented by a $$d-$$dimensional tensor with dimensions $$n_1\times n_2\times \cdots \times n_d$$. Here, $$q$$ is some arbitrary Markovian noising process -- it gradually adds noise to the data to produce "noised" samples $$x_1,x_2,\dots, x_T$$. This is also called the \textit{forward diffusion process}. That is, for each timestep $$1 \leq t \leq T$$, 

$$
\begin{align*}
q(x_t | x_{t-1}) = \mathcal{N}\Big(x_t, \sqrt{1-\beta_t} x_{t-1}, \beta_t \mathbf{I}\Big).
\end{align*}
$$

Here, $$\beta_t$$ is some variance schedule (often chosen to be linear or sinusoidal in $$t$$). Ideally we would like to find $$q(x_T)$$, but this proves to be difficult for large $$T$$, as we find that
$$
\begin{equation}\label{eq:fwd}
    q(x_{1:T}|x_0)=\prod_{t=1}^T q(x_t|x_{t-1}).
\end{equation}
$$

By reparameterizing using $$\alpha_t=1-\beta_t$$ and $$\bar{\alpha}_t=\prod_{s=1}^t \alpha_s$$ with the noise $$\epsilon_t\sim \mathcal{N}(0, \mathbf{I})$$, $$x_t$$ follows a closed form
$$
\begin{equation}\label{eq:reparam}
    x_t=\sqrt{\bar{\alpha}_t}x_0+\sqrt{1-\bar{\alpha}_t} \epsilon_0,
\end{equation}
$$

and so to produce a sample at any timestep $$t$$ it suffices to sample from $$x_t\sim q(x_t \mid x_0)=\mathcal{N}(x_t;\sqrt{\bar{\alpha}_t}x_0,(1-\bar{\alpha}_t)\mathbf{I})$$. As $$\beta_t$$ is predetermined, we can precompute our $$\alpha$$ coefficients for all timesteps.

When taking $$T\to\infty$$ (and assuming some regularity conditions on $$\beta_t$$), $$x_T$$ approaches an isotropic Gaussian distribution. This means that if we wish to sample from $$q(x_0)$$, it suffices to sample from $$q(x_T)$$ and then sample reverse steps $$q(x_{t-1} \mid x_t)$$ until we reach $$x_0$$, which is what we refer to as the \textit{backward diffusion process}. However, in practice $$q(x_{t-1} \mid x_t)$$ is intractable, as finding a closed form would require information about $$q(x_0)$$, which we do not know. Thus, we must approximate it with a parameterized model $$p_\theta$$, which in our case is a neural network. Knowing that $$q(x_{t-1} \mid x_t)$$ is Gaussian, we can choose the mean $$\mu_\theta$$ and covariance $$\Sigma_\theta$$ matrices as our learned parameters in $$p_\theta$$:

$$
\begin{equation}\label{eq:bwd_nn}
p_\theta(x_{t-1} \mid x_t)=\mathcal{N}\Big(x_{t-1};\mu_\theta(x_t,t),\Sigma_\theta(x_t,t)\Big).
\end{equation}
$$

<p>
  <span class="math inline">\( \Sigma_\theta(x_t, t) \)</span> is learned through an interpolation between
  <span class="math inline">\( \beta_t \)</span> and
  <span class="math inline">\( \tilde{\beta}_t = \frac{1-\bar{\alpha}_{t-1}}{1-\bar{\alpha}_t}\,\beta_t \)</span>
  by predicting a mixed model vector <span class="math inline">\( v \)</span>:
</p>

<div class="equation" id="eq-learn_sigma">
  \[
    \Sigma_\theta(x_t, t)
      = \exp\bigl(v\log\beta_t \;+\;(1-v)\log\tilde{\beta}_t\bigr).
    \tag{5}
  \]
</div>

$$
\begin{equation}\label{eq:learn_sigma}
    \Sigma_\theta(x_t,t) = \exp\left(v\log \beta_t + (1-v)\log ~{\beta}_t\right).
\end{equation}
$$

Now, analogously to the forward diffusion process found in Eq. \ref{eq:fwd}, the backward diffusion process is given by

$$
\begin{equation}\label{eq:bwd}
    p_\theta(x_{0:T})=p_\theta(x_T)\prod_{t=1}^T p_\theta(x_{t-1} \mid x_t).
\end{equation}
$$

To train the model such that $$p(x_0)$$ learns the data distribution $$q(x_0)$$, we follow the techniques found in variational autoencoders (which diffusion models happen to be equivalent to!). We aim to minimize the variational lower bound $$\mathcal{L}_{\text{vlb}}$$ given by 
$$
\begin{equation}\label{eq:vlb}
    \mathcal{L}_{\text{vlb}} = \mathcal{L}_0 + \underbrace{\mathcal{L}_1 + \cdots + \mathcal{L}_{T-1}}_{\mathcal{L}_t} + \mathcal{L}_T.
\end{equation}
$$

Here,

$$
\begin{align*}
    \mathcal{L}_0 &:= -\log p_\theta(x_0 \mid x_1), \\
    \mathcal{L}_t &:= D_{\text{KL}}(q(x_{t-1} \mid x_t, x_0) \mid\mid p_\theta(x_T)), \\
    \mathcal{L}_{T} &:= D_{\text{KL}}(q(x_T \mid x_0) \mid \mid p(x_T)),
\end{align*}
$$

where $$D_{\text{KL}}(p \mid\mid q)$$ is the KL-divergence between two probability distrubitions $$p$$ and $$q$$. Intuituively, $$\mathcal{L}_0$$ follows the reconstruction term found in the ELBO of a variational autoencoder, and in practice is learned using a separate decoder. $$\mathcal{L}_t$$ formulates the difference between the approximated denoising steps $$p_\theta(x_{t-1} \mid x_t)$$ and the desired ones $$q(x_{t-1} \mid x_t, x_0)$$. $$\mathcal{L}_T$$ shows how close $$x_T$$ is to the standard Gaussian.

While this objective is well-justified, it was found in \citep{DDPM} that a simpler objective performs better in practice. In particular, they do not directly parameterize $$\mu_\theta(x_t, t)$$ as a neural network, but rather train a model $$\epsilon_\theta(x_t,t)$$ to predict the noise $$\epsilon$$ from Equation \ref{eq:reparam}. This transforms our loss into 

$$
\begin{equation}\label{eq:simp_loss}
    \mathcal{L}_{\text{simple}} = \mathbb{E}_{t \sim [1,T], x_0\sim q(x_0), \epsilon \sim \mathcal{N}(0, 1)}\Big[\mid \mid \epsilon - \epsilon_\theta(x_t, t) \mid \mid^2\Big].
\end{equation}
$$

During sampling, we can use substitution to derive $$\mu_\theta(x_t, t)$$ from $$\epsilon_\theta(x_t, t)$$:

$$
\begin{align}
\mu_\theta(x_t, t)=\frac{1}{\sqrt{\alpha_t}}\left(x_t - \frac{1-\alpha_t}{\sqrt{1-\bar{\alpha}_t}}\epsilon_\theta(x_t, t)\right).
\end{align}
$$

Finally, when we are learning the covariance $$\Sigma_\theta$$, we must also modify the loss function to incorporate $$\Sigma_\theta$$, in which case we would have 

$$
\begin{align}
\mathcal{L}_{\text{final}} = \mathcal{L}_{\text{simple}} + \lambda \mathcal{L^\prime}_{\text{vlb}},
\end{align}
$$

where $$\mathcal{L^\prime}_{\text{vlb}}$$ is a modified version that only learns $$\Sigma_\theta$$, and $$\lambda=0.001$$ is a small tuning parameter. 

<h3 id="mask-generation">Mask Generation</h3>

<p>
  Our masks are generated to simulate four different types of observations found in the real world: in-situ, weather balloons, airplanes, and satellite swaths. We shall denote the amount of each type of observation by 
  \(N_{\text{in-situ}}\), \(N_b\), \(N_p\), and \(N_s\) respectively.  
  To start, let our cube be defined by its dimensions 
  \(\mathcal{C} = \{1,2,\dots, L\}\times\{1,2,\dots, W\}\times\{1,2,\dots, H\}\), 
  where \(L\) is the number of latitude indices, \(W\) the number of longitude indices, and \(H\) the number of altitude indices. We shall denote the altitude grid as \(\mathcal{A}=\{a_1,a_2,\dots, a_h\}\), where \(a_h\) is the altitude corresponding to index \(h\). A mask \(\mathbf{M}\in \mathbb{R}^{L \times W\times H}\) is defined by \(\mathbf{M}(l_i,w_i,h_i)=1\), and denotes unknown points in the cube. Our observations will aim to set \(\mathbf{M}=0\) to indicate that they are known observations, and thus "unmasked".
<br><br>
  Further mathematical details regarding the formulation of these observation modalities may be provided upon request.
</p>

<h3 id="repaint">RePaint</h3>

<p>
  For the purposes of image inpainting, <a href="#ref-RePaint">[10]</a> used the following idea: to predict missing pixels in an image, one can use the mask region as a condition. Since at each reverse step \(x_t \to x_{t-1}\) it only depends on \(x_t\), we can keep the “correct” properties of \(q(x_t)\). In other words, rather than generating \(q(x_{t-1})\) uniformly, we decompose the sample into <em>known</em> and <em>unknown</em> regions, each having a different distribution.
</p>

<p>
  In this section, we denote the ground truth image as \(x\), the unknown pixels as \(m\odot x\) and the known pixels as \((1-m)\odot x\), where \(\odot\) refers to the Hadamard product. Then:
</p>

<div class="equation" id="eq-repaint-known-unknown">
$$
\begin{aligned}
  x_{t-1}^{\mathrm{known}}   &\sim \mathcal{N}\bigl(\sqrt{\bar\alpha_t}\,x_0,\;(1-\bar\alpha_t)\mathbf{I}\bigr),\\
  x_{t-1}^{\mathrm{unknown}} &\sim \mathcal{N}\bigl(\mu_\theta(x_t,t),\;\Sigma_\theta(x_t,t)\bigr),\\
  x_{t-1}                     &= m\odot x_{t-1}^{\mathrm{known}}
                                + (1-m)\odot x_{t-1}^{\mathrm{unknown}}.
\end{aligned}
$$
</div>

<p>
  However, the authors showed that naively applying this yielded images that were discombobulated; the sampling of the known pixels is performed without considering the generated parts of the image, which obstructs any potential synergy between the two. To allow more time for the conditional known pixels and the generated unknown pixels to harmonize, a resampling method is enacted by diffusing the output \(x_{t-1}\) back into \(x_t\) using the same forward process 
  $$x_t \sim \mathcal{N}\bigl(x_t; \sqrt{\bar\alpha_t}\,x_0,\,(1-\bar\alpha_t)\mathbf{I}\bigr).$$
  This is done \(r\) times before continuing to the next time step. While this process introduces noise to the known regions, some information from the generated \(x_{t-1}^{\mathrm{unknown}}\) is preserved in \(x_t^{\mathrm{unknown}}\), leading to a better \(x_t^{\mathrm{unknown}}\) overall that is more aligned with the information contained in \(x_{t}^{\mathrm{known}}\). This resampling is performed every \(j\) timesteps.
</p>

<h3 id="framework">Framework</h3>

<p>
  Each variable was trained through the following process: first, the test dataset is entirely “blueified” by extracting solely the blue channel of each voxel, netting single-channel data which is then input into the diffusion framework to learn \(\mu_\theta(x_t,t)\) and \(\Sigma_\theta(x_t,t)\). This is done to simplify the model and reduce potential errors. Next, the generated masks are applied to the test dataset, all while the resulting model from the first step is given to the RePaint framework as the backwards diffusion process. Finally, RePaint outputs an inpainted image that we then project back into an RGB colormap to yield the final result. This process is illustrated in Figure <a href="#p3_fig-framework">1</a>.
</p>

<div id="p3_fig-framework" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p3_fig-framework.png"
         alt="Diagram of proposed guided diffusion + RePaint pipeline."
         style="max-height:400px;object-fit:contain;"
         class="img-fluid rounded z-depth-1"/>
  </div>
</div>
<div class="caption">Figure 1: Diagram of proposed guided diffusion + RePaint pipeline.</div>

<p>
  Regarding the architecture used within the training process, notice that the model’s input and output should be of the same size— to this end, <a href="#ref-DDPM">[7]</a> employed a U-Net, a symmetric architecture with input and output of the same spatial size that uses skip connections between encoder and decoder blocks of corresponding feature dimension. The input image is first downsampled and then upsampled until reaching its initial size. This was left unchanged within our experiments, other than converting 2D convolutional layers to 3D ones (among other tweaks) when generalizing to cubes.
</p>

<h3 id="experiments">Experiments</h3>

<p>
  Each model was trained with 3000 diffusion steps, a learning rate of \(\alpha = \texttt{1e-4}\), 20,000 epochs, and with \(\texttt{learn\_sigma=True}\), meaning the model learns the noise scale \(\Sigma\) as part of the training process. Mathematically, this determines whether \(\Sigma\) in Equation <a href="#eq-bwd_nn">4</a> is learned (\(\Sigma_\theta(x_t,t)\)) or fixed (\(\Sigma_t\)). We also choose a linear noise scheduler:
</p>

<div class="equation" id="eq-lin_noise">
$$
\beta_t = \beta_1 + t\cdot\frac{\beta_T - \beta_1}{T},
$$
</div>

<p>
  where \(\beta_1 = 10^{-4}\) and \(\beta_T = 0.02\). The cosine scheduler showed no noticeable improvements in our case. Attention resolutions were chosen at \(n=16,8\), although ideally \(n=32\) would also be incorporated with more compute power. Due to RAM limitations, we could only attempt a batch size of 1. The original paper found optimal results with \(r = 20\) and \(j = 10\), so we kept these values consistent.
</p>

<h2 id="results">Results</h2>

<h3 id="results-2d">2D Fields</h3>
<p>
  Predictions from RePaint were evaluated against the ERA5 data as ground truth. This model was trained on select samples from 2020, with all pressure levels from the first of March being omitted as the test dataset. Figure <a href="#p3_fig-2d_vo">2</a> below provides a visual representation of the results at pressure levels 1000 hPa, 500 hPa, and 125 hPa. As shown from the rightmost column of the figure, it is clear that error magnitudes were maintained within 0.3 units of the desired value. Here, error was computed by taking the difference between each pixel on the reproduced image and the corresponding pixel from the ground truth image. Comparing the 2nd and 4th column of the figure, it makes intuitive sense that in places where swaths or in-situ observations were located (e.g. places where data is known), the corresponding area in the error visualization is effectively zero.
</p>

<div id="p3_fig-2d_vo" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p3_fig-vo_2d_results.png"
         alt="Visualization of reconstructed 2D vorticity fields at various pressure levels."
         style="max-height:500px;object-fit:contain;"
         class="img-fluid rounded z-depth-1"/>
  </div>
</div>
<div class="caption">Figure 2: Visualization of reconstructed 2D vorticity (<em>v<sub>o</sub></em>) fields at various pressure levels on 2020-03-01.</div>

<h3 id="results-3d">3D Fields</h3>
<p>
  All 2021 timestamps from the ERA5 dataset were taken as training data for each respective variable—as mentioned in Section <a href="#data">Data</a>, this included 5 days/every month, 7 hours/day, yielding 420 samples. Random arbitrary samples were selected from 2022 and 2023 for the test dataset.
</p>
<p>
  Masks were generated for varying levels of observations—specifically, 1.9%, 3.75%, 7.5%, 15%, 30%, and 60%. The makeup of these masks are detailed in Table <a href="#p3_tab-mask_params">1</a>. To illustrate the performance of the framework at a glance, Figure <a href="#p3_fig-mask_comparison">3</a> provides the reconstructed <em>v<sub>o</sub></em> fields for a single timestamp at different percentages of known data. Clearly, the model goes down in performance with less known data, so much so that at 3.75% and 1.9% known configurations the reconstructed field does not show much resemblance to the ground truth.
</p>

<table id="p3_tab-mask_params" class="table">
  <caption>Table 1: Parameters used to generate masks in 3D</caption>
  <thead>
    <tr>
      <th>Mask Parameters</th>
      <th>1.9 %</th>
      <th>3.75 %</th>
      <th>7.5 %</th>
      <th>15 %</th>
      <th>30 %</th>
      <th>60 %</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>num_in_situ</td><td>200</td><td>200</td><td>300</td><td>500</td><td>1000</td><td>100</td></tr>
    <tr><td>num_balloons</td><td>100</td><td>200</td><td>200</td><td>200</td><td>500</td><td>500</td></tr>
    <tr><td>num_planes</td><td>5</td><td>25</td><td>25</td><td>100</td><td>150</td><td>100</td></tr>
    <tr><td>plane_thickness</td><td>3</td><td>3</td><td>3</td><td>3</td><td>3</td><td>3</td></tr>
    <tr><td>num_sat_swaths</td><td>0</td><td>0</td><td>1</td><td>4</td><td>5</td><td>20</td></tr>
    <tr><td>swath_thickness</td><td>0</td><td>0</td><td>5</td><td>5</td><td>5</td><td>9</td></tr>
  </tbody>
</table>

<div id="p3_fig-mask_comparison" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p3_fig-mask_comparison.png"
         alt="Reconstructed vorticity 3D field for varying known-data levels."
         style="max-height:400px;object-fit:contain;"
         class="img-fluid rounded z-depth-1"/>
  </div>
</div>
<div class="caption">Figure 3: Reconstructed <em>v<sub>o</sub></em> 3D field for varying levels of known observations on 2022-04-01 at 00:00:00.</div>

<p>
  To showcase more representative results for 3D field reconstruction, Figure <a href="#p3_fig-3d_results">4</a> provides a visualization of the synthetic ground truth, synthetic observations, reproduced cubes, and error cubes for each of the variables <em>v<sub>o</sub></em>, <em>q</em>, and <em>u</em> at timestamp 2022-04-01 at 00:00:00. These results were obtained from a benchmark mask of 60% known data out of the entire 64×64×64 cube.
</p>

<div id="p3_fig-3d_results" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p3_3d_results_60.png"
         alt="Visualization of reconstructed 3D fields for all variables."
         style="max-height:500px;object-fit:contain;"
         class="img-fluid rounded z-depth-1"/>
  </div>
</div>
<div class="caption">Figure 4: Visualization of reconstructed 3D fields for all variables on 2022-04-01 at 00:00:00.</div>

<p>
  In Figure <a href="#p3_fig-avg_abs_error">5</a>, we see the average absolute errors plotted against the mask percentages for the three variables. These values were computed from the <em>mean absolute error</em> (MAE):
</p>
<div class="equation" id="p3_eq-MAE">
$$
\mathrm{MAE} = \frac{1}{n} \sum_{i=1}^n \bigl|\hat{x}_i - x_i\bigr|,
$$
</div>
<p>
  where \(n\) is the number of voxels, \(x_i\) is the ground truth bluescale value for a voxel \(i\), and \(\hat{x}_i\) is the inpainted bluescale value. This can be interpreted as on average, how far on the bluescale from 0 to 255 each reproduced value was from the true value.
</p>

<div id="p3_fig-avg_abs_error" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p3_error_vs_masked.png"
         alt="Plot of average absolute error vs. % known across all variables."
         style="max-height:500px;object-fit:contain;"
         class="img-fluid rounded z-depth-1"/>
  </div>
</div>
<div class="caption">Figure 5: Plot of average absolute error vs. % known across all variables.</div>

<p>
  Another common error metric is the <em>root mean squared error</em> (RMSE):
</p>
<div class="equation" id="p3_eq-RMSE">
$$
\mathrm{RMSE} = \sqrt{\frac{1}{N}\sum_{i=1}^N (\hat{x}_i - x_i)^2},
$$
</div>
<p>
  where \(N\) now is the number of non-missing voxels. Normalized RMSE values were computed and listed in Table <a href="#p3_tab-rmse">2</a>.
</p>

<table id="p3_tab-rmse" class="table">
  <caption>Table 2: Normalized average RMSE values for different mask % across variables</caption>
  <thead>
    <tr>
      <th>Variable</th>
      <th>1.9 %</th>
      <th>3.75 %</th>
      <th>7.5 %</th>
      <th>15 %</th>
      <th>30 %</th>
      <th>60 %</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><em>v<sub>o</sub></em></td><td>0.99</td><td>0.36</td><td>0.24</td><td>0.13</td><td>0.08</td><td>0.04</td></tr>
    <tr><td><em>q</em></td>       <td>1.14</td><td>0.44</td><td>0.31</td><td>0.16</td><td>0.07</td><td>0.07</td></tr>
    <tr><td><em>u</em></td>       <td>1.02</td><td>0.50</td><td>0.31</td><td>0.14</td><td>0.08</td><td>0.04</td></tr>
  </tbody>
</table>

<h2 id="discussion">Discussion</h2>
<p>
  Interestingly, even though specific humidity had the highest MAE for very sparse observations, it had the lowest MAE when given more data except for at 60%. This may be due to the observed data being in “harder to predict” areas (for example, it is easier to predict in the middle parts of the cubes that were interpolated due to them being relatively uniform). It could also mean that specific humidity is more “predictable” than the other variables due to less variation, but more test runs would be necessary.
</p>
<p>
  As shown in Table <a href="#p3_tab-rmse">2</a>, the RMSE monotonically decreases for all three variables as more of the data is known. This intuitively makes sense, as the more data we start with, the better the reconstruction will be. In particular, doubling at the extreme low end significantly reduces the RMSE. We also note that we get diminishing returns in the RMSE with each doubling of the observed data—this is consistent with sublinear or power-law-like learning curve. Still, we would need more data points to comfortably assess and estimate a relationship between the sparsity of the data and the accuracy of the algorithm.
</p>
<p>
  It is also important to note that the RMSE values at 1% known data exceed one, which is theoretically inconsistent with normalized RMSE values; this discrepancy can be attributed to the slight variability in the generated masks. Specifically, the masks are created to target a given percentage of known data with tolerance ±0.5%. For instance, a mask with 70% unknown data can range between 69.5% and 70.5% unknown. Since the RMSE calculation directly depends on the number of known observations, this variability can lead to averaged values slightly exceeding the expected range. Here, the base percentage of known data was used without explicitly accounting for the precise number of known observations. While incorporating the exact values would be ideal, this was omitted for simplicity and time efficiency.
</p>
<p>
  One small change that may be worth testing is the conversion of the RGB images into greyscale rather than isolating one of the RGB channels as done now. Greyscale processing would allow us to maintain all information from the images while still working with a single channel, reducing computational cost and retaining the overall structure of the image.
</p>

<h2 id="future-work">Future Work</h2>

<h3 id="4d-hypercubes">4D Hypercubes</h3>
<p>
  The natural next step would be to extend this idea into the temporal dimension by training on hypercubes that represent the evolution of each of the variables through time. However, we encountered many roadblocks in the implementation of both guided diffusion and RePaint. <code>pytorch</code> has no native support for 4D convolutional layers, nor does it have 4D average pooling or 4D interpolation. As such, these would need to be implemented from scratch directly from the source code, posing quite a challenge. This would still be an interesting path to pursue, and a solution may involve creating a wrapper for TensorFlow within our code, as TensorFlow does have native support for some of these functions.
</p>

<h3 id="graph-neural-networks">Graph Neural Networks</h3>
<p>
  Modeling the data as cubes is unrealistic in the real world for many reasons: it assumes that each observation is evenly spaced, when in reality there are often observation centers clustered sporadically throughout several locations. Additionally, 3D representations rely heavily on the accuracy of interpolation when dealing with incomplete data, which can introduce unwanted errors. In its current state, there exists no unified way of encapsulating more than one variable within a single cube. In contrast, graphs are capable of incorporating spatial and temporal relationships directly within their nodes and edges, allowing us to eliminate the dimensionality of the data and seamlessly incorporate multiple variables of interest if desired.
</p>
<p>
  To formalize this, let us define the data as a graph <em>\(\mathcal{G}=(V,E)\)</em>. The nodes <em>V</em> represent spatial locations indexed by (lat, lon, alt), and the edges <em>E</em> represent spatial relationships (e.g. adjacency or connectivity). Each node <em>n<sub>i</sub>∈V</em> has a feature vector <em>\(\mathbf{x}_i \in \mathbb{R}^{F_n}\)</em>, where <em>F<sub>n</sub></em> is the number of node features. For example, in our case we could take <em>\(\mathbf{x}_i^{(t)}=[v_{o_i}^{(t)},q_i^{(t)},u_i^{(t)},\dots]^\intercal\)</em>. Similarly, each edge <em>e<sub>ij</sub>∈E</em> has an associated feature vector <em>\(\mathbf{e}_{ij} \in \mathbb{R}^{F_e}\)</em>, where <em>F<sub>e</sub></em> is the number of edge features. Following the above example, we could take <em>\(\mathbf{e}_{ij}=[d_{ij},\theta_{ij},\Delta v_{o_{ij}},\Delta q_{ij},\Delta u_{ij},\dots]^\intercal\)</em>, where <em>d<sub>ij</sub></em> represents the geodesic between nodes, <em>θ<sub>ij</sub></em> is the relative orientation or bearing between nodes, and <em>Δv<sub>o_{ij}</sub></em> is the difference in vorticity between nodes, etc. Then at each timestep <em>t</em>, we can redefine a dynamic graph:
</p>
<div class="equation" id="p3_eq-graph-dynamic">
$$
\mathcal{G}_t = (V, E, \mathbf{X}_t, \mathbf{E}_t),
$$
</div>
<p>
  where <em>V={n<sub>1</sub>,…n<sub>N</sub>}</em>, <em>E⊆V×V</em>, <em>\(\mathbf{X}_t∈\mathbb{R}^{N×F_n}\)</em>, and <em>\(\mathbf{E}_t∈\mathbb{R}^{|E|×F_e}\)</em>.
</p>
<div class="equation" id="p3_eq-masks">
$$
\begin{aligned}
  \mathcal{M}_x &= \{m_i^x \mid m_i^x = 1\text{ if node feature }i\text{ is observed and }0\text{ otherwise}\},\\
  \mathcal{M}_e &= \{m_j^e \mid m_j^e = 1\text{ if edge feature }j\text{ is observed and }0\text{ otherwise}\}.
\end{aligned}
$$
</div>
<p>
  The UNet architecture for training the diffusion model is insufficient for this task, paving the way for Graph Convolutional Nets (GCNs), introduced in <a href="#ref-GCN">[11]</a>. A GCN has hidden layer defined by:
</p>
<div class="equation" id="p3_eq-gcn">
$$
\mathbf{H} = \sigma\bigl(\tilde{\mathbf{D}}^{-1/2}\tilde{\mathbf{A}}\tilde{\mathbf{D}}^{-1/2}\,\mathbf{X}\,\mathbf{\Theta}\bigr).
$$
</div>
<p>
  GCNs have their own shortcomings, as they do not allow multidimensional edge features, but there are workarounds (for example in <a href="#ref-GCNFix">[15]</a>), and are not the only option. Message-Passing Neural Networks <a href="#ref-MPNN">[12]</a>, Graph Attention Networks <a href="#ref-GAT">[13]</a>, and Graph Transformers <a href="#ref-GT">[14]</a> could also be viable alternatives.
</p>

<h2 id="conclusion">Conclusion</h2>
<p>
  This study successfully integrates diffusion-based generative models with advanced inpainting techniques to address challenges in data assimilation for atmospheric data. By leveraging DDPM and RePaint, the proposed framework effectively reconstructs missing or masked atmospheric data across both 2D and 3D fields, demonstrating resilience to varying degrees of data sparsity. This has potential for practical applications in numerical weather prediction and climate modeling.
</p>
<p>
  While promising, the approach faces challenges related to computational demands; higher-resolution data, which is often found in climate datasets, would be infeasible under this framework. Future work is aimed at extending the framework to 4D hypercubes and graph neural networks, which can offer more nuanced and realistic real-world atmospheric data assimilation. Overall, this research contributes a novel methodology to the intersection of machine learning and meteorological data analysis, paving the way for more accurate and efficient forecasting systems.
</p>



---

<h2 id="references">References</h2>
<ol>
  <li id="ref-SongErmon">
    Song, Y., &amp; Ermon, S. (2019). <strong>Generative Modeling by Estimating Gradients of the Data Distribution</strong>. In H. Wallach et al. (Eds.), <em>Advances in Neural Information Processing Systems</em> (Vol. 32). Curran Associates, Inc.  
    <a href="https://proceedings.neurips.cc/paper_files/paper/2019/file/3001ef257407d5a371a96dcd947c7d93-Paper.pdf">PDF</a>
  </li>
  <li id="ref-Langevin">
    Sohl-Dickstein, J., Weiss, E., Maheswaranathan, N., &amp; Ganguli, S. (2015). <strong>Deep Unsupervised Learning using Nonequilibrium Thermodynamics</strong>. In F. Bach &amp; D. Blei (Eds.), <em>Proceedings of the 32nd International Conference on Machine Learning</em> (Vol. 37, pp. 2256–2265). PMLR.  
    <a href="https://proceedings.mlr.press/v37/sohl-dickstein15.html">Paper</a>
  </li>
  <li id="ref-KalmanWeather">
    Dee, D. P. (1991). <strong>Simplification of the Kalman filter for meteorological data assimilation</strong>. <em>Quarterly Journal of the Royal Meteorological Society</em>, 117(498), 365–384.  
    <a href="https://doi.org/10.1002/qj.49711749806">https://doi.org/10.1002/qj.49711749806</a>
  </li>
  <li id="ref-4DVar">
    Sugiura, N., Awaji, T., Masuda, S., Mochizuki, T., Toyoda, T., Miyama, T., Igarashi, H., &amp; Ishikawa, Y. (2008). <strong>Development of a four-dimensional variational coupled data assimilation system for enhanced analysis and prediction of seasonal to interannual climate variations</strong>. <em>Journal of Geophysical Research: Oceans</em>, 113(C10).  
    <a href="https://doi.org/10.1029/2008JC004741">https://doi.org/10.1029/2008JC004741</a>
  </li>
  <li id="ref-EnKS">
    Milewski, T., &amp; Bourqui, M. S. (2013). <strong>Potential of an ensemble Kalman smoother for stratospheric chemical-dynamical data assimilation</strong>. <em>Tellus A: Dynamic Meteorology and Oceanography</em>, 65(1).  
    <a href="https://doi.org/10.3402/tellusa.v65i0.18541">https://doi.org/10.3402/tellusa.v65i0.18541</a>
  </li>
  <li id="ref-HybridEnKS">
    Tardif, R., Hakim, G. J., &amp; Snyder, C. (2014). <strong>Coupled atmosphere–ocean data assimilation experiments with a low-order climate model</strong>. <em>Climate Dynamics</em>, 43, 1631–1643.  
    <a href="https://doi.org/10.1007/s00382-013-1989-0">https://doi.org/10.1007/s00382-013-1989-0</a>
  </li>
  <li id="ref-DDPM">
    Ho, J., Jain, A., &amp; Abbeel, P. (2020). <strong>Denoising Diffusion Probabilistic Models</strong>. In H. Larochelle et al. (Eds.), <em>Advances in Neural Information Processing Systems</em> (Vol. 33, pp. 6840–6851). Curran Associates, Inc.  
    <a href="https://proceedings.neurips.cc/paper_files/paper/2020/file/4c5bcfec8584af0d967f1ab10179ca4b-Paper.pdf">PDF</a>
  </li>
  <li id="ref-guided-diffusion">
    Dhariwal, P., &amp; Nichol, A. (2021). <strong>Diffusion Models Beat GANs on Image Synthesis</strong>. In M. Ranzato et al. (Eds.), <em>Advances in Neural Information Processing Systems</em> (Vol. 34, pp. 8780–8794). Curran Associates, Inc.  
    <a href="https://proceedings.neurips.cc/paper_files/paper/2021/file/49ad23d1ec9fa4bd8d77d02681df5cfa-Paper.pdf">PDF</a>
  </li>
  <li id="ref-improved-diffusion">
    Nichol, A., &amp; Dhariwal, P. (2021). <strong>Improved Denoising Diffusion Probabilistic Models</strong>. arXiv:2102.09672.  
    <a href="https://arxiv.org/abs/2102.09672">https://arxiv.org/abs/2102.09672</a>
  </li>
  <li id="ref-RePaint">
    Lugmayr, A., Danelljan, M., Romero, A., Yu, F., Timofte, R., &amp; Van Gool, L. (2022). <strong>RePaint: Inpainting Using Denoising Diffusion Probabilistic Models</strong>. In <em>Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)</em> (pp. 11461–11471).  
  </li>
  <li id="ref-GCN">
    Kipf, T. N., &amp; Welling, M. (2017). <strong>Semi-Supervised Classification with Graph Convolutional Networks</strong>. In <em>International Conference on Learning Representations</em>.  
    <a href="https://openreview.net/forum?id=SJU4ayYgl">https://openreview.net/forum?id=SJU4ayYgl</a>
  </li>
  <li id="ref-MPNN">
    Gilmer, J., Schoenholz, S. S., Riley, P. F., Vinyals, O., &amp; Dahl, G. E. (2017). <strong>Neural Message Passing for Quantum Chemistry</strong>. In <em>Proc. 34th Int. Conf. on Machine Learning (ICML ’17)</em> (Vol. 70, pp. 1263–1272).  
  </li>
  <li id="ref-GAT">
    Veličković, P., Cucurull, G., Casanova, A., Romero, A., Liò, P., &amp; Bengio, Y. (2018). <strong>Graph Attention Networks</strong>. In <em>International Conference on Learning Representations</em>.  
    <a href="https://openreview.net/forum?id=rJXMpikCZ">https://openreview.net/forum?id=rJXMpikCZ</a>
  </li>
  <li id="ref-GT">
    Yun, S., Jeong, M., Kim, R., Kang, J., &amp; Kim, H. J. (2019). <strong>Graph Transformer Networks</strong>. In H. Wallach et al. (Eds.), <em>Advances in Neural Information Processing Systems</em> (Vol. 32). Curran Associates, Inc.  
    <a href="https://proceedings.neurips.cc/paper_files/paper/2019/file/9d63484abb477c97640154d40595a3bb-Paper.pdf">PDF</a>
  </li>
  <li id="ref-GCNFix">
    Tan, X., Yang, J., Zhao, Z., Xiao, J., &amp; Li, C. (2024). <strong>Improving Graph Convolutional Network with Learnable Edge Weights and Edge-Node Co-Embedding for Graph Anomaly Detection</strong>. <em>Sensors</em>, 24(8), 2591.  
    <a href="https://doi.org/10.3390/s24082591">https://doi.org/10.3390/s24082591</a>
  </li>
</ol>