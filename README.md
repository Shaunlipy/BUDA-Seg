# BUDA-Seg: Bayesian Unsupervised Domain Adaptation for Cerebellum Segmentation


## Data

[AB annotation](https://drive.google.com/file/d/1erZ_DVrG7JSRFbWvJrfoXnmN9DNvOVBz/view?usp=sharing)
files are in PNG format with integers from 0 to 5
- 0: tissue border
- 1: white matter
- 2: purkinje layer
- 3: granular layer
- 4: molecular layer
- 5: background

[AB annotation (RGB version for visualization)](https://drive.google.com/file/d/1G5L2QPWM9OtPt0EfNSnnv7YueRuvW4w-/view?usp=sharing)
- black: tissue border
- white: white matter
- red: purkinje layer
- green: granular layer
- blue: molecular layer
- grey: background

[AB<sub>trans</sub> input](https://drive.google.com/file/d/1-2wmDGPscTrnzg3Yj2lfwIXdAVWkV7B0/view?usp=sharing)
transformed AB input patches

[BB input](https://drive.google.com/file/d/1-1gXOGcCS6uYgV9_O_hFPBj_5sqKMLWn/view?usp=sharing)
BB data input patches


## Dependencies
Clone the repo and install [requirements.txt](requirements.txt) in a Python >= 3.8 environment. [Anaconda](https://www.anaconda.com) is recommended.

```angular2html
pip install -r requirements.txt
```


## Training

To train the model, please refer to [Pytorch-CycleGAN](https://github.com/aitorzip/PyTorch-CycleGAN) for stage 1 _Distribution Transfer_. Here we provide transformed AB input for stage 2 _Bayesian uncertainty learning_:  

<ol>
    <li> download and unzip all datasets (AB<sub>trans</sub> input, AB annotation, and BB input) in to one root folder.
    <li> train a segmentation model Seg<sub>A</sub> on the AB<sub>trans</sub> dataset.
    <li> generate pseudo labels for the BB.
    <li> train Seg<sub>B</sub> on AB<sub>trans</sub> and BB.
    <li> generate predictions for BB and replace pseudo labels with the new predictions. 
    <li> repeat 3-4 for iteratively self-training.
</ol>

Specifically:

for step 2, run:
```
python train.py --exp 0 --dataroot PathToYourDataroot
```

** exp=0 for AB<sub>trans</sub> training, exp=1,2,3 for iterative self-training with Bayesian uncertainty learning **

for step 3, run:
```angular2html
python inference.py --dataroot PathToYourDataroot --ckpt_dir output/0/model.pt
```

** replace 0/model.pth with corresponding training iteration (exp) when generating new pseudo labels after each iterative joint training **

for step 4, run:
```angular2html
python train.py --exp 1 --dataroot PathToYourDataroot
```

** replace --exp 1 with corresponding iteration **
