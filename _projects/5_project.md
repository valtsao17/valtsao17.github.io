---
layout: page
title: optimization of carbon capture and storage deployment under carbon tax uncertainty
description: "[ms&e314] - final project"
img: assets/img/p5_eps_method.png
importance: 3
category: optimization
toc:
  sidebar: left
---
<h2>Introduction & Background</h2>
Over the last several decades, the rapid acceleration of anthropogenic greenhouse gas (GHG) emissions has exacerbated catastrophic changes to climate and resulted in record high global average temperatures (greater than 1.2$$^{\circ}$$C) compared to pre-industrial ages. Since the 1.5$$^{\circ}$$C limit set by the Intergovernmental Panel on Climate Change (IPCC) in 2015, a major shift has been observed within economic, political, and scientific sectors to address this issue--financial institutions have demonstrated interest in investing in net-zero portfolios, renewables such as wind and solar have become vastly cost-competitive due to technological progress, and companies have raised the standard for corporate targets and commitments to emission reduction goals. 

CCUS (Carbon Capture, Storage and Utilization) is a relatively young technology being investigated primarily at the research and development level to reduce global GHG by a process that can be summarized in 4 steps: (1) exhaust from large sources of CO2 such as power plants are captured through a system of ducts which are then passed through a cooling tower, 
(2) the cooling tower brings down the temperature of the exhaust and passes it through an absorber at which point CO2 will be removed from the flue gas through a chosen solvent, 
(3) the carbon-free air is now released back to the atmosphere while the CO2-rich solvent is heated in a regenerator to separate the mixture into CO2 and solvent counterparts, and lastly 
(4) the pure CO2 portion is compressed into a form that can be transported and otherwise dealt with as the solvent is recycled for the next cycle. 
Conventionally, this pressurized carbon, now in its 'supercritical' state, is then pumped and stored deep underground in rock formations or reservoirs. 

One of the policy instruments which address the market failures of CCS and is most relevant to this project is the idea of an economy-wide carbon tax. This effectively sets a price on carbon and follows the “polluter pays principle” in which the burden of responsibility falls back to those who contributed to the emissions. By defining a direct tax rate on CO2 emissions, there is an incentive for firms to install CCS in order to reduce their tax liability. Despite this, the uncertain nature of carbon tax pricing poses quantifiable risk for deploying CCS systems—this context makes up the basis of the problem framework for this project and will be further elaborated on in the following section.

<h2>Problem Statement</h2>
This project focuses on the optimization problem posed by Zhang et. al in the linked paper titled "Risk management optimization framework for the optimal deployment of carbon capture and storage system under uncertainty" published in 2019 <sup>[1](#ref-zhang2019)</sup>. Specifically, the objective is to provide an optimization framework for Carbon Capture Storage (CCS) that would allow government and policy makers to make informed decisions about carbon reduction by quantifying risk metrics associated with an uncertain carbon tax. This objective is achieved through an in-depth analysis of the expected total cost and risk associated with the system, with a final output that informs whether it is more worth it to deploy CCS or pay for the carbon tax.

<h2>Theory</h2>
As stated above, the nature of this problem inherently lends itself to a multi-objective optimization problem (MOOP); we must both minimize the expected total cost and the risk metrics. Please refer to Appendix B for a brief overview of MOOPs.

<h3>Pareto Optimality</h3>
Recall that we denote by $$\mathcal{X}$$ the feasible set, with its image under the objective function mapping $$f$$ denoted by $$\mathcal{Y} := f(\mathcal{X})$$. We call a feasible solution $$\hat{x}\in \mathcal{X}$$ *Pareto optimal* if there is no other $$x\in \mathcal{X}$$ such that $$f(x)\leq f(\hat{x})$$. The set of all efficient solutions we shall denote by $$\mathcal{X}_E$$. Then if $$\hat{x}$$ is efficient, $$f(\hat{x})$$ is called a *nondominated* point. If $$x_1,x_2 \in \mathcal{X}$$ and $$f(x_1)\leq f(x_2)$$, we say $$x_1$$ *dominates* $$x_2$$, and denote this by $$x_1\succ x_2$$. In other words, we achieve dominance when a solution $$x_i$$ is no worse than $$x_j$$ in all objectives and when $$x_i$$ performs better than $$x_j$$ in at least one objective.

When discussing Pareto optimality, due to the trade-off between the conflicting objectives, rather than discussing a single "optimal" solution, we often refer to a set of solutions that represents the optimal trade-off. We call this set of all Pareto efficient solutions the *Pareto front*. Formally, we define this set $$P(\mathcal{Y})$$ as 

$$
\begin{align*}
P(\mathcal{Y})=\{y_1 \in \mathcal{Y} \mid \{ y_2 \in \mathcal{Y} \mid y_2 \succ y_1, \: y_1 \neq y_2\} = \emptyset\}.
\end{align*}
$$

<h3><span>\(\epsilon\)</span>-Constraint Method</h3>
The $$\epsilon-$$constraint method to solving MOOPs and obtaining a Pareto front is perhaps one of the most intuitive. We consider only one of the objectives and minimize, while all others are transformed to constraints. This can be written as

$$
\begin{align} 
    \min_{x\in \mathcal{X}} &\: f_i(x) \nonumber \\ 
    \text{subject to } &f_k(x)\leq \epsilon_k,\quad k=1,\dots p, \: k\neq j, \label{eq:eps}
\end{align}
$$
where $$\epsilon \in \mathbb{R}^p$$. We refer to the following figure as an example:
<div class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p5_eps_method.png" alt="EPS method diagram"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
  </div>
</div>

To justify this approach we must show that optimal solutions to problems of the form Eq. \ref{eq:eps} are at least weakly Pareto optimal.

**Theorem 1.** *Let $$\hat{x}$$ be an optimal solution to Eq. \ref{eq:eps}  for some $$j$$. Then $$\hat{x}$$ is weakly Pareto optimal.*

*Proof.*
Suppose $$\hat{x} \notin \mathcal{X}_{wE}$$. Then there exists some $$x\in \mathcal{X}$$ such that $$f_k(x)<f_k(\hat{x})$$ for all $$k=1,\dots, p$$; in particular, $$f_j(x) < f_j(\hat{x})$$. Then since $$f_k(x)< f_k(\hat{x})\leq \epsilon_k$$ for $$k\neq j$$, we note that $$x$$ is feasible for Eq. \ref{eq:eps}. This contradicts that $$\hat{x}$$ is optimal.

    Now to further build upon this, we need the optimal solution to be unique. 

**Theorem 2.** *Let $$\hat{x}$$ be a unique optimal solution to Eq. \ref{eq:eps} for some $$j$$. Then $$\hat{x} \in \mathcal{X}_{sE}$$, and therefore $$\hat{x} \in \mathcal{X}_E$$.*

*Proof.*
Assume there is some $$x\in \mathcal{X}$$ with $$f_k(x)\leq f_k(\hat{x})\leq \epsilon_k$$ for all $$k\neq j$$. If in addition, $$f_j(x)\leq f_j(\hat{x})$$, then we must have $$f_j(x)=f_j(\hat{x})$$, as $$\hat{x}$$ is an optimal solution. Thus $$x$$ is an optimal solution, and the uniqueness of the optimal solution hence implies $$x=\hat{x}$$ and so $$\hat{x}\in \mathcal{X}_{sE}$$.

In general, however, the efficiency of $$\hat{x}$$ is related to $$\hat{x}$$ being an optimal solution of Eq. \ref{eq:eps} for all $$j=1,\dots, p$$ with the same $$\epsilon$$ used in all of these problems.

**Theorem 3.** *The feasible solution $$\hat{x} \in \mathcal{X}$$ is Pareto optimal if and only if there exists an $$\hat{\epsilon}\in \mathbb{R}^p$$ such that $$\hat{x}$$ is an optimal solution of Eq. \ref{eq:eps} for all $$j=1,\dots, p$$.*

*Proof.*
$$(\Longrightarrow):$$ Let $$\hat{\epsilon}=f(\hat{x})$$. Assume $$\hat{x}$$ is not an optimal solution for some $$j$$. Then there must be some $$x\in \mathcal{X}$$ with $$f_j(x)<f_j(\hat{x})$$ and $$f_k(x)\leq \hat{\epsilon}_k=f_k(\hat{x})$$ for all $$k\neq j$$; that is, $$\hat{x}\notin \mathcal{X}_E$$.

$$(\Longleftarrow):$$ Suppose $$\hat{x}\notin \mathcal{X}_E$$. Then for some index $$j \in \{1,\dots, p\}$$ with corresponding feasible solution $$x\in \mathcal{X}$$ such that $$f_j(x)<f_j(\hat{x})$$ and $$f_k(x)\leq f_k(\hat{x})$$ for $$k\neq j$$. Therefore $$\hat{x}$$ cannot be an optimal solution of Eq. \ref{eq:eps} for any $$\epsilon$$ for which it is feasible. Any such $$\epsilon$$ must have $$f_k(\hat{x})\leq \epsilon_k$$ for $$k\neq j$$.

Thus we have shown that for appropriate choices of $$\epsilon$$, all Pareto optimal solutions can be found. However as the proof shows, these $$\epsilon_j$$ values are equal to the actual objective values of the Pareto optimal solution we would like to find. Let us denote by $$\mathcal{E}_j := \{\epsilon \in \mathbb{R}^p \mid \{x\in \mathcal{X}: f_k(x)\leq \epsilon_k,\: k\neq j\}\neq \emptyset \}$$ for $$\epsilon \in \mathcal{E}_j$$ the set of right-hand sides for which Eq. \ref{eq:eps} is feasible and by $$\mathcal{X}_j(\epsilon) := \{x\in \mathcal{X}\mid x \text{ is an optimal solution of Eq. \ref{eq:eps}}$$} for $$\epsilon \in \mathcal{E}_j$$, the set of optimal solutions of Eq. \ref{eq:eps}. From the above, for each $$\epsilon \in \bigcap_{j=1}^p \mathcal{E}_j$$,

$$
\begin{align*}
\bigcap_{j=1}^p \mathcal{X}_j(\epsilon) \subset \mathcal{X}_E \subset \mathcal{X}_j(\epsilon) \subset \mathcal{X}_{wE}.
\end{align*}
$$

<h3>Numerical Solvers</h3>
The single objective implementation of the proposed problem relies heavily on CVXPY, a Python library that has had wide-ranging applications in signal processing, machine learning, and more. As a domain-specific language (DSL), it solves convex optimization problems by transforming problems into low-level forms that can then be analyzed with generic solvers. CVXPY enforces the disciplined convex programming (DCP) ruleset to guarantee conditions for convexity. Taking an expression made up of variables, parameters, and constants, it will assign one of five curvatures (constant, affine, convex, concave, unknown) while also keeping track that signs of the argument are compliant with curvature rules. Given that CVXPY utilizes solvers based on the class of convex optimization problem, we found we were able to achieve the most accurate results with MOSEK.

To tackle the MOO aspect of this problem and verify the results of CVXPY, we also employ the python library pymoo. For the risk-neutral solution, we use the built-in genetic algorithm (implemented with a stable $$(\mu + \lambda)$$ algorithm) with 100 generations. To incorporate the risk metrics, we implement the $\epsilon-$constraint method with the built-in non-dominated sorting genetic algorithm (NSGA2) with 100 generations. Careful consideration was put into ensuring numerical stability.

<h2>Methodology</h2>
The approach taken to solve this problem is divided into two parts--the first utilizes deterministic parameters to generate a stochastic model and capture ideal CCS deployment under uncertain carbon tax conditions; the second is concerned with quantifying the risk metrics associated with the optimized configuration ascertained from the previous part. Figure [1](#p5_fig-optim-framework) below presents a visualization of the the overall proposed framework. 

<div id="p5_fig-optim-framework" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p5_optim_framework.jpg" alt="Schematic of problem framework"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
  </div>
</div>

<div class="caption">
  Figure 1: Schematic of problem framework, taken from referenced paper.
</div>


Table [1](#tab-params) and [2](#tab-vars) within Appendix [C](#app:a) list all the parameters and variables used to express the two-stage stochastic programming model and financial risk metrics into equations. The mathematical formulation of the MILP model and its constraints will be presented in the following section and discussed in greater detail.

<h3>Objective Function</h3>
To take into account the uncertainty of the carbon tax, a two-part stochastic model is used. For all CC plants, pipelines, and CS facilities, capital and operating costs are required as inputs along with probability distributions of each scenario. These deterministic values are then paired with the uncertain carbon tax parameter and passed into the model to generate the total expected cost of all scenarios, as shown in Eq. \ref{eq:obj}.

$$
\begin{align} 
    \min \: \text{ETC} = \mathbb{E}[\text{TC}_s] &= \text{FixedCost} + \sum_{s\in S}p_s \times \text{OperationCost}_s \label{eq:obj}\\ 
    \text{where } \text{FixedCost} &= \sum_{i \in I} FC_i^c x_{it} + \sum_{j \in J} FC_j^s y_{jt} + \sum_{i \in I} \sum_{j \in J} \sum_{r \in R} FC_r^p z_{ijrt}  \label{eq:fc}\\
    \text{and } \text{OperationCost} &= \sum_{t \in T} \Bigg[ \sum_{i \in I} OC_i^c F_{ijts} + \sum_{j \in J} OC_j^s F_{ijts} +  \nonumber \\
    &\sum_{i \in I} \sum_{j \in J} \sum_{r \in R} OC_r^p F_{ijts} - p_{\text{tax}} \left(\sum_{i \in I} \sum_{j \in J} F_{ijts} + p_{\text{subsidy}} \right) \Bigg] \forall s \in S \label{eq:oc}
\end{align}
$$

By minimizing the expected total cost of carbon removal and storage, the model is able to determine the optimal allocation network for CO$_2$ reduction, effectively enabling a direct comparison to the alternative strategy of paying the carbon tax. 

<h3>Constraints</h3>
Given the objective function above, several constraints are established to deal with pipeline logistics (Eqs. \ref{eq:cap_pipe_max} - \ref{eq:con_pipe}), maximum thresholds for CO$_2$ captured (Eq. \ref{eq:emission_max}) and CO$$_2$$ stored (Eq. \ref{eq:storage_max}), as well as feasible bounds of CO$$_2$$ flow rates (Eq. \ref{eq:flow_min}). To expand briefly, Eqs. \ref{eq:cap_pipe_max} & \ref{eq:cap_pipe_min} limit the amount of CO$$_2$$ moving through each pipeline to the associated maximum and minimum flow rates for a pipe with a given diameter (in our case, this diameter = 16"). Eq. \ref{eq:con_pipe} ensures that there can only be one pipeline connection between a source $$i$$ and a sink $$j$$. Eq. \ref{eq:emission_max} dictates that the captured CO$$_2$$ will always be less than the total emissions for source $i$, and Eq. \ref{eq:storage_max} requires the stored CO$_2$ to be ultimately lower than the injection rate of sink $$j$$. Lastly, Eq. \ref{eq:flow_min} bounds the problem such that only positive CO$$_2$$ flow rates can be evaluated. As a general note, the temporal aspect of varying operation duration for CC and CS facilities was not factored to reduce the complexity of the model (e.g. $$x_{it} = y_{it} = z_{ijrt}$$ = 1 for all scenarios). 

$$
\begin{align}
F_{ijts} &\leq \sum_{r \in R} Q_r^{p,max} z_{ijrt} \quad \forall \, i\in I, j\in J, t\in T, s\in S \label{eq:cap_pipe_max} \\
F_{ijts} &\geq \sum_{r \in R} Q_r^{p,min} z_{ijrt} \quad \forall \, i\in I, j\in J, t\in T, s\in S \label{eq:cap_pipe_min} 
\end{align}
$$

$$
\begin{align}
\sum_{j \in J} \sum_{r \in R} z_{ijrt} \leq 1  \quad i\in I, t\in T \label{eq:con_pipe} 
\end{align}
$$

$$
\begin{align}
\sum_{j \in J} F_{ijts} \leq E_ix_{it} \quad i\in I, t\in T, s\in S \label{eq:emission_max} 
\end{align}
$$

$$
\begin{align}
\sum_{i \in I} F_{ijts} \leq S_jy_{it}  \quad j\in J, t\in T, s\in S  \label{eq:storage_max} 
\end{align}
$$

$$
\begin{align}
F_{ijts} \geq 0  \quad i\in I, j\in J, t\in T, s\in S  \label{eq:flow_min} 
\end{align}
$$

Please refer to Appendix [C](#app:a) for greater specificity on parameter and variable definitions.

<h3>Risk Management Model</h3>
Given the nature of stochastic optimization where we are minimizing the expectation of our total cost for this problem, our current formulation remains risk-neutral. To account for risk management techniques into our model, we introduce two different risk metrics: downside risk and variability index, which will be defined below. 

The *downside risk* focuses on the extremes of the cost distribution. It aims to reduce the risk associated with a scenario whose cost is higher than some pre-defined threshold $$\Omega$$. This is defined as, for all scenarios $$s$$,

$$
\begin{equation}\label{eq:downside}
    \Delta_s = \begin{cases}
        \text{TC}_s - \Omega & \text{if }\text{TC}_s\geq \Omega, \\
        0 &\text{otherwise}. 
    \end{cases},\quad \text{DownSide} = \sum_{s\in S}p_s\Delta_s.
\end{equation}
$$

In contrast, the *variability index* takes into account the positive difference between the cost of scenario $$s$$ and the expectation of all scenarios. That is,

$$
\begin{equation}\mathcal{V}_s = \begin{cases}
    \text{TC}_s - \sum_{s\in S} p_s \text{TC}_s &\text{if }\text{TC}_s \geq \sum_{s\in S} p_s\text{TC}_s, \\ 0&\text{otherwise}.
\end{cases},\quad \text{VarIndex}=\sum_{s\in S}p_s\mathcal{V}_s.\end{equation}
$$

We should note that the variability index is precisely the downside risk when we choose $$\Omega = \sum_{s\in S} p_s\text{TC}_s$$. With these risk metrics defined, we now define our new MOOP:

$$
\begin{equation}\label{eq:MOOPrisk}
\begin{aligned}
    \min_{x\in \mathcal{X}}&\: \mathbb{E}[\text{TC}_s] \\
    \text{subject to }& \text{DownSide/VarIndex} \leq \epsilon.
    \end{aligned}
\end{equation}
$$

<h2>Results</h2>
To generate numerical results, data was taken from the paper's case study, which focused on CCS deployment in Northeast China as well as some parts of Inner Mongolia. In total, 6 different CC facilities and 3 CS plants were considered. To simplify the problem for the scope of this project, information from only 1 pipeline was used as opposed to the 5 options listed in the paper. A study period of 50 years was analyzed. Appendix [D](#app:numerical) summarizes all relevant numerical information for CO$$_2$$ plants, reservoirs, and pipelines. The carbon tax was generated randomly each year from a $$\mathcal{N}(50, 10)$$ random variable, and the government subsidy was generated from a $\text{Pareto}(1)$ random variable to account for long tails in years which a large subsidy may be granted.

The cost distribution of a single run of the risk-neutral scenario is given in Appendix [E](#app:risk-neutral) with Figure [5](#fig-cost). With our implementation, we note that a majority of the runs peak in the highest bin. This is due to the fact that for years with a lower carbon tax, it becomes more optimal to do minimal CCS due to the high operation costs. The following figure illustrates this fact in more detail:

<div id="p5_fig-2" class="row justify-content-center mt-3">
  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_cvxpyout.png" alt="Solved with CVXPY + MOSEK"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-2a" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (a) Solved with CVXPY + MOSEK
    </div>
  </div>

  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_GAoutput.png" alt="Solved with pymoo + GA"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-2b" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (b) Solved with pymoo + GA
    </div>
  </div>
</div>

<div class="caption text-center mt-2">
  Figure 2: Comparison of Different Algorithms
</div>

Figure [2](#p5_fig-2) visualizes the total cost of the optimal scenario versus carbon tax. The left graph (Figure [2a](#fig-2a)) was solved with CVXPY and the right graph (Figure [2b](#fig-2b)) with the $$\epsilon-$$constraint method implemented in pymoo. As we can see, the overall trend remains the same $-$ when the carbon tax is above \$40/Mt, it becomes increasingly optimal to allocate more resources into CCS. We note that due to the stochastic nature of the GA, suboptimal mutations led to outlier results. 

The next two graphs capture the Pareto front for both risk metrics. For downside risk, multiple $$\Omega$$ values were studied, and an $$\Omega=13000000000$$ yielded the best results for a balanced Pareto front. For the variability index, approximately 450 trials were run in the risk-neutral case with the expected value used in $$\Omega$$ being the average of all runs. In both cases, we observe that the risk scales linearly with the cost above the chosen $$\Omega$$ threshold. This aligns with our intuitive understanding of the risk metrics. 

<div id="fig-pareto" class="row justify-content-center mt-4">
  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_pareto_downside.png" alt="Pareto Front of Downside Risk"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-pareto-a" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (a) Pareto Front of Downside Risk
    </div>
  </div>

  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_pareto_varindex.png" alt="Pareto Front of Variability Index"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-pareto-b" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (b) Pareto Front of Variability Index
    </div>
  </div>
</div>

<div class="caption text-center mt-2">
  Figure 3: Pareto Fronts
</div>

<div id="fig-cost-distributions" class="row justify-content-center mt-4">
  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_downside_vs_neutral.png" alt="Cost Distribution of VarIndex Risk Scenarios"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-cost-distributions-a" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (a) Cost Distribution of VarIndex Risk Scenarios
    </div>
  </div>

  <div class="col-sm-6 d-flex flex-column align-items-center">
    <img src="/assets/img/p5_varindex_vs_neutral.png" alt="Cost Distribution of Downside Risk Scenarios"
         style="max-height: 300px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
    <div id="fig-cost-distributions-b" style="font-size: 90%; text-align: center; margin-top: 0.5rem;">
      (b) Cost Distribution of Downside Risk Scenarios
    </div>
  </div>
</div>

<div class="caption text-center mt-2">
  Figure 4: Comparison of Cost Distributions
</div>

Finally, we compare the distributions of the risk-neutral solution against both risk metrics. From the plots, we can see that as a whole the risk metrics create a longer tail on the distribution than the risk-neutral case. As expected, scenarios with lower total costs also yield the least amount of risk. 


<h2>Conclusions</h2>
In this project, we present a comprehensive framework for optimizing the deployment of Carbon Capture Storage (CCS) systems under uncertain carbon tax conditions. By adopting a multi-objective optimization approach, we balance minimizing expected total costs with managing associated risks, providing a necessary tool for policymakers and industries in their quest to reduce carbon footprints. The application of the $\epsilon$-constraint method, supported with numerical results from CVXPY and pymoo, allows for a nuanced exploration of the trade-offs between cost and risk, particularly in the face of stochastic carbon tax scenarios.

Our results underline the importance of strategic CCS deployment, especially when carbon taxes exceed a certain threshold. Through the comparison of risk-neutral and risk-aware approaches, we highlight how incorporating downside risk and variability index metrics can significantly affect decision-making processes, shifting the focus towards solutions that mitigate financial risks associated with carbon emission penalties.

The findings not only support the viability and necessity of CCS technologies in contemporary and future carbon management strategies, but also illuminate the path for refining risk management frameworks within the environmental policy and economic landscape. This study serves as a stepping stone for further research into the optimization of green technologies and their integration into global efforts to combat climate change, advocating for informed, data-driven decisions in the face of uncertainty.

This project was completed with the following person(s): BX

## Appendix

#### A. Supplementary Problem Context {#appendix-context}
In 2015, the Intergovernmental Panel on Climate Change (IPCC) established that warming must be kept under a threshold of 1.5$$^{\circ}$$C in order to prevent irreversible changes to the world as we know it today, which meant reaching net zero global emissions by 2050 and massive implementation of mitigation and adaption strategies now as opposed to later. This also spurred along the creation of the Paris Agreement, an international treaty which pledged to limit emissions through a global framework of transparency, which was consequently signed by 195 nations. 

As of September 2023, 108 countries of the 177 who submitted Nationally Determined Contributions (NDCs) have since shown reduced total emissions compared to their initial baseline. Still, there is much work to be done--even if all commitments are fully realized by 2030, the world is still projected to reach 2$$^{\circ}$$C by 2100, not to mention the limitations set forth by regulations, policy, and current lack of rigorous accounting. To provide a broad overview of the issue, there are four primary GHGs: carbon dioxide (CO$$_2$$), methane (CH$_4$), nitrous oxide (N$$_2$$O), and fluorinated gases such as hydrofluorocarbons. While each has its own implications and associated impact on the environment, our main focus for this problem hinges on atmospheric carbon dioxide.

In 2022, the Global Status Report of CCS cited 83 projects in the US with an additional 66 projects in the planning stage. At the current state, North America boasts the highest number of commercial projects within this space, with developments also underway in Norway, Australia, and the UK. Because CCS can be deployed on new as well as existing fossil fuel facilities, its potential for decarbonization has wide-ranging applications. For one, captured CO2 can be utilized within varying industries as inputs to product manufacturing such as cement and paints. It can also be used in carbon-negative frameworks, effectively removing more CO2 from the atmosphere than emitted, like BECCS (Bio-energy with Carbon capture and Storage) and DAC (Direct Air Capture). However, deploying CCS at large-scale will still require a robust policy architecture that considers the dynamics of new market creation and infrastructure regulation at a high level as well as proper resource management between various industry sectors. Perhaps what remains one of the most salient barriers to CCS is the high upfront cost of deployment and the fact that it generates no revenue in the absence of a defined carbon market. Because current carbon prices are well below the cost of CCS, private entities have no incentive to invest in the technology given its high abatement cost. 

#### B. Multiobjective Optimization {#app:moop}
To introduce the concept of MOOP, let us first take a look at its general form: 
\begin{equation}\label{eq:MOOP}
\begin{aligned}
    \min& \:\:(f_1(x),\dots, f_p(x)) \\
    \text{subject to }& x \in \mathcal{X}.
\end{aligned}
\end{equation}
Here we define $\mathcal{X}$ as the \textit{feasible set}, and $f_i(x)$ for $i=1,\dots, p$ as the \textit{objective functions}. Thus, a multiobjective optimization problem (MOOP) is formulated when one or more objective functions need to be maximized or minimized simultaneously and the desired answer is a set of solutions that represent the tradeoff between various conflicting goals (e.g. cost vs. mass or performance vs. fuel consumption). This tradeoff is commonly referred to as the Pareto optimal solution, which will be further explained in the following section.
% where $f_m(x)$ corresponds to the $m$th objective function, $x$ corresponds to the solution, and  $U$ corresponds to the feasible set. 
% where $f_m(x)$ corresponds to the $m$th objective function, $g_j(x)$ and $h_k(x)$ correspond to the constraints, and  $x$ is a  n-dimension vector of decision variables. 

 Within MOO, there is a multiobjective function space as well as a solution space--for every $x$ within the decision space, there is a corresponding mapping that can be made to the objective space. Because of this attribute, the convexity of the objective space is paramount to informing how a problem can be solved. Unlike a single-objective optimization problem where the solution can be definitively found by comparing objective function values, MOO utilizes the concept of dominance to evaluate the goodness of a solution. There are primarily two solution methods to MOOP which include scalarization and Pareto-optimal front (POF). 

<h4 id="app:a">Appendix C: Parameters & Variables</h4>
<!-- Parameters Table -->
<div class="table-wrapper" id="tab-params">
  <table class="table table-bordered table-sm" style="max-width: 900px; margin: auto;">
    <caption><strong>Table 1:</strong> All parameters used to formulate the objective functions, constraints, and risk models.</caption>
    <thead>
      <tr>
        <th style="width: 25%;">Symbol</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>C<sub>p</sub></td><td>Material price, $/kg</td></tr>
      <tr><td>E<sub>f</sub></td><td>Longitudinal joint factor</td></tr>
      <tr><td>E<sub>i</sub></td><td>Total CO<sub>2</sub> emission at site i, Mt/y</td></tr>
      <tr><td>I</td><td>Capital cost for pipeline, $</td></tr>
      <tr><td>L</td><td>Length of pipeline, km</td></tr>
      <tr><td>t<sub>p</sub></td><td>Wall thickness of pipeline, mm</td></tr>
      <tr><td>S<sub>j</sub></td><td>Injection limit for storage site j, Mt/y</td></tr>
      <tr><td>S<sub>p</sub></td><td>Minimum yield stress for pipeline, MPa</td></tr>
      <tr><td>P<sub>max</sub></td><td>Maximum operating pressure for pipeline, MPa</td></tr>
      <tr><td>p<sub>s</sub></td><td>Occurrence probability of scenario s</td></tr>
      <tr><td>p<sub>tax</sub></td><td>Price of tax/ton of CO<sub>2</sub>, $/t CO<sub>2</sub></td></tr>
      <tr><td>p<sub>subsidy</sub></td><td>Subsidy incentive provided by government, $</td></tr>
      <tr><td>ρ</td><td>Density of pipeline material, kg/Nm<sup>3</sup></td></tr>
      <tr><td>Ω</td><td>Maximum cost threshold defined by decision-maker, $</td></tr>
      <tr><td>FC<sub>i</sub><sup>c</sup></td><td>Capital cost of CC facility for power plant i, $</td></tr>
      <tr><td>FC<sub>r</sub><sup>p</sup></td><td>Capital cost of pipeline with diameter r, $</td></tr>
      <tr><td>FC<sub>j</sub><sup>s</sup></td><td>Capital cost of CC facility for storage site j, $</td></tr>
      <tr><td>OC<sub>i</sub><sup>c</sup></td><td>Operating cost of CC facility for power plant i, $/t CO<sub>2</sub></td></tr>
      <tr><td>OC<sub>r</sub><sup>p</sup></td><td>Operating cost of pipeline with diameter r, $/t CO<sub>2</sub></td></tr>
      <tr><td>OC<sub>j</sub><sup>s</sup></td><td>Operating cost of CC facility for storage site j, $/t CO<sub>2</sub></td></tr>
      <tr><td>Q<sub>r</sub><sup>p,max</sup></td><td>Upper bound of CO<sub>2</sub> flowrate for pipeline with diameter r, Mt/y</td></tr>
      <tr><td>Q<sub>r</sub><sup>p,min</sup></td><td>Lower bound of CO<sub>2</sub> flowrate for pipeline with diameter r, Mt/y</td></tr>
    </tbody>
  </table>
</div>

<!-- Variables Table -->
<div class="table-wrapper mt-5" id="tab-vars">
  <table class="table table-bordered table-sm" style="max-width: 900px; margin: auto;">
    <caption><strong>Table 2:</strong> All variables used to formulate the objective functions, constraints, and risk models.</caption>
    <thead>
      <tr>
        <th style="width: 25%;">Symbol</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>DownRisk</td><td>Downside risk objective, $</td></tr>
      <tr><td>ETC</td><td>Expected total cost of scenarios, $</td></tr>
      <tr><td>F<sub>ijts</sub></td><td>Amount of CO<sub>2</sub> sent from source i to j for scenario s, Mt</td></tr>
      <tr><td>FixedCost</td><td>Fixed capital cost for all facilities in CCS system, $</td></tr>
      <tr><td>OperationCost</td><td>Operating cost for all facilities in CCS system, $</td></tr>
      <tr><td>ProRisk</td><td>Probabilistic financial risk objective for all facilities in CCS system, $</td></tr>
      <tr><td>TC<sub>s</sub></td><td>Total cost of CCS system for scenario s through planning horizon, $</td></tr>
      <tr><td>VarIndex</td><td>Variability index objective for risk management, $</td></tr>
      <tr><td>x<sub>it</sub></td><td>Binary variable for CC facility installation at source i</td></tr>
      <tr><td>y<sub>it</sub></td><td>Binary variable for CC facility installation at storage j</td></tr>
      <tr><td>z<sub>ijrt</sub></td><td>Binary variable for pipeline installation from source i to sink j</td></tr>
    </tbody>
  </table>
</div>

<h4 id="app:numerical">Appendix D: Numerical Data</h4>

<!-- Table 1: CC Plants -->
<div class="table-wrapper" id="tab-cc-plants">
  <table class="table table-bordered table-sm" style="max-width: 900px; margin: auto;">
    <caption><strong>Table 3:</strong> Relevant numerical data for CC plants.</caption>
    <thead>
      <tr>
        <th>Plant</th>
        <th>Emission [Mt/y]</th>
        <th>Capital Cost [$]</th>
        <th>O&amp;M Cost [$/t]</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>9.20</td><td>2.5017×10<sup>8</sup></td><td>30.27</td></tr>
      <tr><td>2</td><td>6.15</td><td>1.7392×10<sup>8</sup></td><td>23.78</td></tr>
      <tr><td>3</td><td>5.90</td><td>1.8282×10<sup>8</sup></td><td>23.19</td></tr>
      <tr><td>4</td><td>5.86</td><td>1.6880×10<sup>8</sup></td><td>23.10</td></tr>
      <tr><td>5</td><td>5.58</td><td>1.6578×10<sup>8</sup></td><td>22.43</td></tr>
      <tr><td>6</td><td>5.00</td><td>1.5181×10<sup>8</sup></td><td>21.12</td></tr>
    </tbody>
  </table>
</div>

<div class="table-wrapper mt-5" id="tab-cs-plants">
  <table class="table table-bordered table-sm" style="max-width: 900px; margin: auto;">
    <caption><strong>Table 4:</strong> Relevant numerical data for CS plants (sinks).</caption>
    <thead>
      <tr>
        <th>Sink</th>
        <th>Injection Limit [Mt/y]</th>
        <th>Capital Cost [$]</th>
        <th>O&amp;M Cost [$/t]</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>MA</td><td>28</td><td>4.0×10<sup>8</sup></td><td>12</td></tr>
      <tr><td>MB</td><td>20</td><td>6.6×10<sup>8</sup></td><td>8</td></tr>
      <tr><td>MC</td><td>30</td><td>5.0×10<sup>8</sup></td><td>5</td></tr>
    </tbody>
  </table>
</div>

<div class="table-wrapper mt-5" id="tab-distances">
  <table class="table table-bordered table-sm" style="max-width: 900px; margin: auto;">
    <caption><strong>Table 5:</strong> Distances [km] between each CC and CS plant.</caption>
    <thead>
      <tr>
        <th>CC Plant (Source)</th>
        <th>MA</th>
        <th>MB</th>
        <th>MC</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>285.0</td><td>483.9</td><td>447.4</td></tr>
      <tr><td>2</td><td>273.6</td><td>648.0</td><td>653.9</td></tr>
      <tr><td>3</td><td>626.4</td><td>363.3</td><td>218.3</td></tr>
      <tr><td>4</td><td>166.3</td><td>616.4</td><td>820.9</td></tr>
      <tr><td>5</td><td>921.8</td><td>701.9</td><td>245.3</td></tr>
      <tr><td>6</td><td>173.1</td><td>412.9</td><td>787.4</td></tr>
    </tbody>
  </table>
</div>

<h4 id="app:risk-neutral">Appendix E: Cost Distribution of Risk-Neutral Scenario</h4>

<div id="fig-cost" class="row justify-content-center mt-3">
  <div class="col-sm-10 d-flex justify-content-center">
    <img src="/assets/img/p5_riskneutraldist.png" alt="Risk-Neutral Scenario Cost Distribution"
         style="max-height: 400px; object-fit: contain;" class="img-fluid rounded z-depth-1" />
  </div>
</div>

<div class="caption text-center mt-2">
  Figure 5: Risk-Neutral Scenario Cost Distribution
</div>


<h2>References</h2>
<ol>
  <li id="ref-zhang2019">
    Zhang, S., Zhuang, Y., Liu, L., Zhang, L., & Du, J. (2019).  
    <strong>Risk management optimization framework for the optimal deployment of carbon capture and storage system under uncertainty.</strong>  
    <em>Renewable and Sustainable Energy Reviews</em>, 113, 109280.  
    <a href="https://doi.org/10.1016/j.rser.2019.109280" target="_blank">https://doi.org/10.1016/j.rser.2019.109280</a>
  </li>
</ol>

