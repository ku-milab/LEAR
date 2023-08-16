# Learn-Explain-Reinforce
This repository provides a TensorFlow implementation of the following paper:
> **Learn-Explain-Reinforce: Counterfactual Reasoning and Its Guidance to Reinforce an Alzheimer's Disease Diagnosis Model**<br>
> [Kwanseok Oh](https://scholar.google.co.kr/citations?user=EMYHaHUAAAAJ&hl=ko)<sup>1</sup>, [Jee Seok Yoon](https://scholar.google.co.kr/citations?user=YjaJ5qQAAAAJ&hl=ko)<sup>2</sup>, and [Heung-Il Suk](https://scholar.google.co.kr/citations?user=dl_oZLwAAAAJ&hl=ko)<sup>1, 2</sup><br/>
> (<sup>1</sup>Department of Artificial Intelligence, Korea University) <br/>
> (<sup>2</sup>Department of Brain and Cognitive Engineering, Korea University) <br/>
> (* indicates equal contribution) <br/> 
> Official Version: https://ieeexplore.ieee.org/document/9854196 <br/>
> Published in IEEE Transactions on Pattern Analysis and Machine Intelligence (2021 JCR-IF: 24.314, COMPUTER SCIENCE & ARTIFICIAL INTELLIGENCE: 2/144)
> 
> **Abstract:** *Existing studies on disease diagnostic models focus either on diagnostic model learning for performance improvement or on the visual explanation of a trained diagnostic model. We propose a novel learn-explain-reinforce (LEAR) framework that unifies diagnostic model learning, visual explanation generation (explanation unit), and trained diagnostic model reinforcement (reinforcement unit) guided by the visual explanation. For the visual explanation, we generate a counterfactual map that transforms an input sample to be identified as an intended target label. For example, a counterfactual map can localize hypothetical abnormalities within a normal brain image that may cause it to be diagnosed with Alzheimer's disease (AD). We believe that the generated counterfactual maps represent data-driven knowledge about a target task, i.e., AD diagnosis using structural MRI, which can be a vital source of information to reinforce the generalization of the trained diagnostic model. To this end, we devise an attention-based feature refinement module with the guidance of the counterfactual maps. The explanation and reinforcement units are reciprocal and can be operated iteratively. Our proposed approach was validated via qualitative and quantitative analysis on the ADNI dataset. Its comprehensibility and fidelity were demonstrated through ablation studies and comparisons with existing methods.*


## Overall framework
- We propose a novel learn-explain-reinforce framework that integrates the following tasks: (1) training a diagnostic model, (2) explaining a diagnostic model's output, and (3) reinforcing the diagnostic model based on the explanation systematically.
- To the best of our knowledge, this work is the first that exploits an explanation output to improve the generalization of a diagnostic model reciprocally.
- In regard to explanation, we propose a GAN-based method to produce multi-way counterfactual maps that can provide a more precise explanation, accounting for severity and/or progression of AD.

![Group 2896 (3)](https://user-images.githubusercontent.com/57162425/141603646-f714edb2-cc01-4b22-80df-056da791947c.png)

![Group 2897 (1)](https://user-images.githubusercontent.com/57162425/141603979-e9f58b9b-6424-4392-8158-1dbb2353b5d3.png)

## Results
###  Example of counterfactual map conditioned on interpolated target labels
![Group 2584](https://user-images.githubusercontent.com/57162425/141603337-4951d4d6-8237-4fc1-80dd-8c87f7dd9d18.png)
### Visual explanation comparison between XAI methods
![Group 2777](https://user-images.githubusercontent.com/57162425/141603345-abdf11e0-f7bf-4ecf-979e-f1604cd27c2c.jpg)

### Iterative explanation-reinforcement learning
![Group 2896](https://user-images.githubusercontent.com/57162425/141603346-ec6afc03-9aa7-4f73-815a-d79969fd0f09.png)


## Requirements
tensorflow (2.2.0)\
tensorboard (2.2.2)\
tensorflow-addons (0.11.0)\
tqdm (4.48.0)\
matplotlib (3.3.0)\
numpy (1.19.0)\
scikit-learn (0.23.2)\
nibabel (3.0.1)


## Datasets
Place them into "data_path" on each Config.py
1. [HandWritten digits data (MNIST)](http://yann.lecun.com/exdb/mnist/)
2. [Alzheimer’s Disease Neuroimaging Initiative (ADNI)](http://adni.loni.usc.edu/)


## How to run
Mode:\
#0 Learn / #1 Explain / #2 Reinforce / #3 Iter_Explanation / #4 Iter_Reinforcement

1. Learn: pre-training a diagnostic model
>- Diagnosic models are a CNN-based architecure (ResNet18, VoxCNN, and SonoNet16)
  >- `LEAR_training.py --mode=0`

2. Explain: Counterfactual map generation using a pre-trained diagnostic model
>- Set the classifier and encoder weight for training (freeze)
>- Change the mode from 0 to 1 on Config.py
  >- `LEAR_training.py --mode=1`

3. Reinforce: Explanation-guided attention to improve its generalizabiliy and performance
>- Set the counterfactual map generator (CMG) weight for training an explanation-guided attention (XGA) module injected into a diagnostic model
>- Change the mode to 2 on Config.py
  >- `LEAR_training.py --mode=2`

4. Iterative explanation-reinforcement learning
>- Enhances the quality of visual explanation as well as the performance of the diagnostic model
>- Change the mode to 3 or 4 on Config.py
  >- `LEAR_iterative_training.py --mode=3 and --mode=4`

## Config.py of each dataset with saved weight path
data_path = Raw dataset path\
save_path = Storage path to save results such as tensorboard event files, model weights, etc.\
cls_weight_path = Pre-trained diagnostic model weight path trained in mode#0 setup\
enc_weight_path = Pre-trained encoder weight path trained in mode#0 setup\
gen_weight_path = Pre-trained counterfactual map generator weight path trained in mode#1 setup\
xga_cls_weight_path = Pre-trained XGA-injected diagnostic model weight path trained in mode#2 or mode#4 setup\
xga_enc_weight_path = Pre-trained XGA-injected encoder weight path trained in mode#2 or mode#4 setup\
xga_gen_weight_path = Pre-trained XGA-injected counterfactual map generator weight path trained in mode#3 setup

## Citation
If you find this work useful for your research, please cite our [journal publication](https://ieeexplore.ieee.org/document/9854196):

```
@ARTICLE{oh2022learn,
  author={Oh, Kwanseok and Yoon, Jee Seok and Suk, Heung-Il},
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence}, 
  title={Learn-Explain-Reinforce: Counterfactual Reasoning and Its Guidance to Reinforce an Alzheimer's Disease Diagnosis Model}, 
  year={2022},
  publisher={IEEE},
  volume={45},
  number={4},
  pages={4843--4857},
  doi={10.1109/TPAMI.2022.3197845}}
```

## Acknowledgement
This work was supported by Institute of Information \& communications Technology Planning & Evaluation (IITP) grant funded by the Korea government (MSIT) No. 2022-0-00959 ((Part 2) Few-Shot Learning of Causal Inference in Vision and Language for Decision Making) and No. 2019-0-00079 (Department of Artificial Intelligence (Korea University)).
