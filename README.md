# Modeling and Control of Off-road Autonomous Vehicles with Situationally Aware Data-Driven Framework (Results Documentation)
This repository serves as a supplemental documentation for capturing the framework, 
experimental setup and results for 
Static and Adaptive Multi-Model Parameterized Koopman framework.

## Table of Contents
- [Abstract](#abstract)
- [Repo Organization](#repository-organization)
- [MMPK Framework](#mmpk-framework)
- [Experimental Setup](#experiment-setup)
- [Results](#results)
- [Contact](#contact)


### Abstract:
In recent years, the scope of deployments of
Uncrewed Ground Vehicles (UGVs) has significantly expanded
from traditional onroad- and industrial- environments to in-
creasingly challenging off-road terrain. These challenging do-
mains feature non-linear wheel-terrain interactions and sig-
nificant environmental perturbations. This complicates use of
traditional model-based control paradigms - accurate models
can be both challenging to derive and may not fully capture
real-world complexities. In this regard, learning-based tech-
niques leveraging neural-networks are often used to capture
complex dynamics but often at the cost of explainability and
generalizability. This study presents an alternate viewpoint
to capture the complex vehicle dynamics in unstructured
environments through a data-driven learning-based approach
but without the mentioned sacrifices. This is achieved by a novel
End-2-End framework utilizing the Koopman operator theory.
Our framework captures the dynamics in a multi-model setting
with an auxiliary control matrix to factor in environmental
perturbations. Additionally, we propose an outer control loop
incorporating a novel model-driven kinodynamic motion plan-
ner and path-tracking controller. Our approach is computation-
ally efficient, robust and generalizable for effectively realizing
offroad path tracking problems. This work advances the data-
driven modeling- planning- and controls approach for offroad
operations, laying the groundwork enabling next-generation
UGVs to expand their operating domains even further.

**Paper (Preprint)**: </br>
[Modeling and Control of Off-road Autonomous Vehicles with Situationally Aware Data-Driven Framework](https://www.researchgate.net/publication/383427789_Modeling_and_Control_of_Off-road_Autonomous_Vehicles_with_Situationally_Aware_Data-Driven_Framework)



### Repository Organization
- **Figures:** This folder contains all the plots from the experiment. 
- **Videos:** Folder for storing video results from MMPK hardware deployment.

### MMPK Framework

![Framework](https://github.com/ajinkya-joglekar/Koopman_Offroad_Autonomy_Deployment/blob/main/Figures/MMPK_offroad_workflow.png)

The framework can be broken down into following process flows.</br>

- **Data-gathering/Model-training (Offline):** 
  - First, we collect data of a vehicle undergoing maneuvers resulting in roll/yaw plane excitation.
  - The data is then converted into body-frame representation.
  - Finally, the data is parameterized into discrete bins according to curvature.
  
- **Parameterized family of Koopman models (Offline):** 
  - In a nutshell, the MMPK approach generates model for each of the discrete curvature bin to capture dynamics across the entire operating domain.
  - We present two types of modeling techniques based on MMPK approach (more info on fundamental foundation can be found [here](https://www.researchgate.net/publication/380152547_Expanding_Autonomous_Ground_Vehicle_Navigation_Capabilities_through_a_Multi-Model_Parameterized_Koopman_Framework)).
     - Static MMPK: Modeling without consideration of effects of offroad terrain leading to load-transfers.
     - Adaptive MMPK: Considers and rejects terrain disturbances with the help of an augmented control matrix.
  
- **Outer control loop (Online):** 
  - For the online real-time deployment, the reference trajectory and the pose estimate of the vehicle are sent to the local motion planner.
  - The local planner samples all the feasible trajectories across models and chooses the one closest to the reference trajectory.
  - Similar to modeling techniques, there are two novel kinodynamic motion planners presented here.
    - Curvature-based Reachability Planner: For each of the model, this planner generates a trajectory satisfying the respective curvature constraint.
    - Load-transfer based Reachability Planner: For each of the model, this planner not only generates a trajectory to satisfy the curvature constraint, but also considers the dynamic load transfers due to terrain perturbations to predict pose evolution across the prediction horizon.
  - This reference trajectory along with the selected curvature is passed forward to the linear MPC.
  - The linear MPC produces high-level commands (velocity steering) for the vehicle.



### Experiment Setup
The following diagram presents pictorial representation for host of experimental stipulations for thorough
validation of MMPK framework in simulation and hardware deployment settings.

![Expsetup](./Figures/Exp_setup_canva.png)

The experiments are divided into two main sections: Experiment I for simulation-based deployment and Experiment II for hardware deployment, as illustrated above. Each experiment considers a modeling approach (Static or Adaptive MMPK), planning strategy (Curvature-based or Load-Transfer based Reachability Planner), along with a linear MPC considering the chosen model set. The experimental setup includes the following stipulations:


**Experiment I-A/II-A:**<br>
Static MMPK with Curvature-based Reachability Planner. 
Here the modeling, planning and control do not account for adapting to environmental perturbations.




**Experiment I-B/II-B:** <br> 
Adaptive MMPK with Curvature-based Reachability Planner. In this case, the model and subsequent controller adapt to changing terrain. However, similar to Experiment (I-A, I-B), the motion planner simply generates trajectories to satisfy curvature requirements.



**Experiment I-C/II-C:** <br>
Adaptive MMPK with Load-transfer based Reachability Planner. Finally, all the modalities consider the effect of terrain perturbations. The modeling and control is adaptive as well as the motion planner, which generates trajectories based on dynamically evolving operating conditions.


### Results
**Simulation deployment (Experiment I-A to I-C) Box plot:**
![ExpIA-IB-IC](./Figures/Exp%20I-A_vs_Exp%20I-B%20_vs_Exp%20I-C.png)


**Simulation deployment (Experiment I-A to I-C) CG distribution:**
![ExpIA-IB-IC](./Figures/CG_distribution_1ABC_N_3.png)

**Hardware deployment (Experiment II-A to II-D) Box plot:**
![ExpIIA-IIB-IIC](./Figures/Exp%20II-A_vs_Exp%20II-B%20_vs_Exp%20II-C.png)


**Hardware deployment (Experiment II-A to II-D) Tracking Performance:**
![ExpIIA-IIB-IIC](./Figures/Exp_IIA_B_C_tracking_annotated_v3.png)

**Hardware deployment videos** <br>

[//]: # (***Experiment II-A:*** <br>)

[//]: # (## Watch the Video)

[//]: # ()
[//]: # ([![Watch the video]&#40;https://img.youtube.com/vi/wTq5Eq4f6iM/0.jpg&#41;]&#40;https://youtu.be/wTq5Eq4f6iM?si=7IWAElwKmhtsJxPH&#41;)
## Experiment II-A Runs

| [![Exp II-A Run 1](https://img.youtube.com/vi/wTq5Eq4f6iM/hqdefault.jpg)](https://youtu.be/wTq5Eq4f6iM?si=KaZJqXWO3yNcG3bs) | [![Exp II-A Run 2](https://img.youtube.com/vi/yYlnjDdXbZ0/hqdefault.jpg)](https://youtu.be/yYlnjDdXbZ0?si=RT0cZwxBZtdYBEWp)|
|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Exp II-A Run 1**                                                                                                   | **Exp II-A Run 2**                                                                                                   |
| [![Exp II-A Run 3](https://img.youtube.com/vi/1rpZeyKL9wg/hqdefault.jpg)](https://youtu.be/1rpZeyKL9wg?si=Fg32JKmwhF9l8aEC)  | [![Exp II-A Run 4](https://img.youtube.com/vi/Xx7AT6N8b_8/hqdefault.jpg)](https://youtu.be/Xx7AT6N8b_8?si=wE52PS7vd0l0etwc)  |
| **Exp II-A Run 3**                                                                                                   | **Exp II-A Run 4**                                                                                                   |


## Experiment II-B Runs

| [![Exp II-B Run 1](https://img.youtube.com/vi/TAX-1FG5c20/hqdefault.jpg)](https://youtu.be/TAX-1FG5c20?si=k2F2nqD_uWDOa6G2) | [![Exp II-B Run 2](https://img.youtube.com/vi/bFwbd5NEEMc/hqdefault.jpg)](https://youtu.be/bFwbd5NEEMc?si=asEJQ9FE0A4iSTTV) |
|---------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Exp II-B Run 1**                                                                                                  | **Exp II-B Run 2**                                                                                                  |
| [![Exp II-B Run 3](https://img.youtube.com/vi/dn9nijezPf0/hqdefault.jpg)](https://youtu.be/dn9nijezPf0?si=N8dszoNMLUDlLH76)| [![Exp II-B Run 4](https://img.youtube.com/vi/hBYvE3paiuU/hqdefault.jpg)](https://youtu.be/hBYvE3paiuU?si=K55GkxS34h9-PK2X) |
| **Exp II-B Run 3**                                                                                                  | **Exp II-B Run 4**                                                                                                  |



## Experiment II-C Runs

| [![Exp II-C Run 1](https://img.youtube.com/vi/mrv5cq-9kf4/hqdefault.jpg)](https://youtu.be/mrv5cq-9kf4?si=-2TDJbhn7hlCP6wP)| [![Exp II-C Run 2](https://img.youtube.com/vi/4v4wX91epK0/hqdefault.jpg)](https://youtu.be/4v4wX91epK0?si=BWlgiAVFiNe45ZuA) |
|---------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| **Exp II-C Run 1**                                                                                                  | **Exp II-C Run 2**                                                                                                  |
| [![Exp II-C Run 3](https://img.youtube.com/vi/t-_JJ8hOccQ/hqdefault.jpg)](https://youtu.be/t-_JJ8hOccQ?si=xVr7z4VSWAAj98WH) | [![Exp II-C Run 4](https://img.youtube.com/vi/USJCNfV1iCU/hqdefault.jpg)](https://youtu.be/USJCNfV1iCU?si=EgOmzQTyW6vSehC0) |
| **Exp II-C Run 3**                                                                                                  | **Exp II-C Run 4**                                                                                                  |


[//]: # (## Experiment II-B Runs/Experiment II-C Runs &#40;coming soon...&#41;)

## Contact
Ajinkya Joglekar
- [Email](mailto:ajoglek@clemson.edu) 📧
- [LinkedIn](https://www.linkedin.com/in/ajinkyajoglekar/) 🔗
- [ResearchGate](https://www.researchgate.net/profile/Ajinkya-Joglekar/research)

## Citations:
This work represents the culmination of two years of research on Koopman Theory applied to Uncrewed Ground Vehicles (UGVs). If you find this research valuable, I encourage you to explore my other papers. If you utilize any part of this dataset in your own research, please consider citing the following papers:


 [Data-Driven Modeling and Experimental Validation of Autonomous Vehicles Using Koopman Operator](https://ieeexplore.ieee.org/abstract/document/10341797)
```bibtex
@INPROCEEDINGS{10341797,
  author={Joglekar, Ajinkya and Sutavani, Sarang and Samak, Chinmay and Samak, Tanmay and Kosaraju, Krishna Chaitanya and Smereka, Jonathon and Gorsich, David and Vaidya, Umesh and Krovi, Venkat},
  booktitle={2023 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)}, 
  title={Data-Driven Modeling and Experimental Validation of Autonomous Vehicles Using Koopman Operator}, 
  year={2023},
  volume={},
  number={},
  pages={9442-9447},
  keywords={Location awareness;Analytical models;Trajectory tracking;Computational modeling;Predictive models;Vehicle dynamics;Standards},
  doi={10.1109/IROS55552.2023.10341797}
}
```




[Expanding Autonomous Ground Vehicle Navigation Capabilities through a Multi-Model Parameterized Koopman Framework](https://www.researchgate.net/publication/380152547_Expanding_Autonomous_Ground_Vehicle_Navigation_Capabilities_through_a_Multi-Model_Parameterized_Koopman_Framework)
```bibtex
@article{joglekar2024expanding,
  title={Expanding Autonomous Ground Vehicle Navigation Capabilities through a Multi-Model Parameterized Koopman Framework},
  author={Joglekar, Ajinkya and Samak, Chinmay and Samak, Tanmay and Krovi, Venkat and Vaidya, Umesh},
  year={2024}
}
```

[Analytical Construction of Koopman EDMD Candidate Functions for Optimal Control of Ackermann-Steered Vehicles](https://www.sciencedirect.com/science/article/pii/S2405896323024266)
```bibtex
@article{joglekar2023analytical,
  title={Analytical Construction of Koopman EDMD Candidate Functions for Optimal Control of Ackermann-Steered Vehicles},
  author={Joglekar, Ajinkya and Samak, Chinmay and Samak, Tanmay and Kosaraju, Krishna Chaitanya and Smereka, Jonathon and Brudnak, Mark and Gorsich, David and Krovi, Venkat and Vaidya, Umesh},
  journal={IFAC-PapersOnLine},
  volume={56},
  number={3},
  pages={619--624},
  year={2023},
  publisher={Elsevier}
}
