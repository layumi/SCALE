# SCALE: Modeling Clothed Humans with a Surface Codec of Articulated Local Elements (CVPR 2021)

[![Paper](https://img.shields.io/badge/arXiv-Paper-b31b1b.svg)](https://arxiv.org/abs/2104.07660)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1lp6r-A-s1kBorIvg6rLD4Ja3o6JOvu3G?usp=sharing)


This repository contains the official PyTorch implementation of the CVPR 2021 paper:

**SCALE: Modeling Clothed Humans with a Surface Codec of Articulated Local Elements** <br>
Qianli Ma, Shunsuke Saito, Jinlong Yang, Siyu Tang, and Michael J. Black <br>
[Full paper](https://arxiv.org/pdf/2104.07660) | [Video](https://youtu.be/-EvWqFCUb7U) | [Project website](https://qianlim.github.io/SCALE.html) | [Poster](https://ps.is.tuebingen.mpg.de/uploads_file/attachment/attachment/650/SCALE_poster_CVPR_final_compressed.pdf)

![](teasers/teaser.gif)


## Installation
- The code has been tested with python 3.6 on both (Ubuntu 18.04 + CUDA 10.0) and (Ubuntu 20.04 + CUDA 11.1).

- First, in the folder of this SCALE repository, run the following commands to create a new virtual environment and install dependencies:

  ```bash
  conda create --name SCALE python=3.6
  conda activate SCALE
  pip install -U pip setuptools
  pip install -r requirements.txt
  mkdir checkpoints
  ```

- Install the Chamfer Distance package (MIT license, taken from [this implementation](https://github.com/krrish94/chamferdist/tree/97051583f6fe72d5d4a855696dbfda0ea9b73a6a)). Note: the compilation is verified to be successful under CUDA 10.0, but may not be compatible with later CUDA versions. 

  ```bash
  cd chamferdist
  python setup.py install
  cd ..
  ```

- You are now good to go with the next steps! All the commands below are assumed to be run from the `SCALE` repository folder, within the virtual environment created above. 



## Run SCALE

- Download our [pre-trained model weights](https://owncloud.tuebingen.mpg.de/index.php/s/pMYCtcpMDjk34Zw), unzip it under the `checkpoints` folder, such that the checkpoints' path is `<SCALE repo folder>/checkpoints/SCALE_demo_00000_simuskirt/<checkpoint files>`.

- Download the [packed data for demo](https://owncloud.tuebingen.mpg.de/index.php/s/B33dqE5dcwbTbnQ), unzip it under the `data/` folder, such that the data file paths are `<SCALE repo folder>/data/packed/00000_simuskirt/<train,test,val split>/<data npz files>`.

- With the data and pre-trained model ready, the following code will generate a sequence of `.ply` files of the teaser dancing animation in `results/saved_samples/SCALE_demo_00000_simuskirt`:

  ```bash
  python main.py --config configs/config_demo.yaml
  ```

- To render images of the generated point sets, run the following command: 

  ```bash
  python render/o3d_render_pcl.py --model_name SCALE_demo_00000_simuskirt
  ```

  The images (with both the point normal coloring and patch coloring) will be saved under `results/rendered_imgs/SCALE_demo_00000_simuskirt`. 
  
  

## Train SCALE

### Training demo with our data examples

- Assume the demo training data is downloaded from the previous step under `data/packed/`. Now run:

  ```bash
  python main.py --config configs/config_train_demo.yaml
  ```

  The training will start! 

- The code will also save the loss curves in the TensorBoard logs under `tb_logs/<date>/SCALE_train_demo_00000_simuskirt`.
- Examples from the validation set at every 10 (can be set) epoch will be saved at `results/saved_samples/SCALE_train_demo_00000_simuskirt/val`.

- Note: the training data provided above are only for demonstration purposes. Due to their very limited number of frames, they will not likely yield a satisfying model. Please refer to the README files in the `data/` and `lib_data/` folders for more information on how to process your customized data.

### Training with your own data

We provide example codes in `lib_data/` to assist you in adapting your own data to the format required by SCALE. Please refer to [`lib_data/README`](./lib_data/README.md) for more details.



## News

- [2021/10/29] **We now provide the packed, SCALE-compatible CAPE data on the [CAPE dataset website](https://cape.is.tue.mpg.de/download.php).** Simply register as a user there to access the download links (at the bottom of the Download page).
- [2021/06/24] Code online!



## License

Software Copyright License for non-commercial scientific research purposes. Please read carefully the [terms and conditions](./LICENSE) and any accompanying documentation before you download and/or use the SCALE code, including the scripts, animation demos and pre-trained models. By downloading and/or using the Model & Software (including downloading, cloning, installing, and any other use of this GitHub repository), you acknowledge that you have read these terms and conditions, understand them, and agree to be bound by them. If you do not agree with these terms and conditions, you must not download and/or use the Model & Software. Any infringement of the terms of this agreement will automatically terminate your rights under this [License](./LICENSE).

The SMPL body related files (including `assets/{smpl_faces.npy, template_mesh_uv.obj}` and the UV masks under `assets/uv_masks/`) are subject to the license of the [SMPL model](https://smpl.is.tue.mpg.de/). The provided demo data (including the body pose and the meshes of clothed human bodies) are subject to the license of the [CAPE Dataset](https://cape.is.tue.mpg.de/). The Chamfer Distance implementation is subject to its [original license](./chamferdist/LICENSE).



## Related Research
[SCANimate: Weakly Supervised Learning of Skinned Clothed Avatar Networks (CVPR 2021)](https://scanimate.is.tue.mpg.de/)<br>
*Shunsuke Saito, Jinlong Yang, Qianli Ma, Michael J. Black*

Our *implicit* solution to pose-dependent shape modeling: cycle-consistent implicit skinning fields + locally pose-aware implicit function = a fully animatable avatar with implicit surface from raw scans without surface registration!

[Learning to Dress 3D People in Generative Clothing (CVPR 2020)](https://cape.is.tue.mpg.de/)<br>
*Qianli Ma, Jinlong Yang, Anurag Ranjan, Sergi Pujades, Gerard Pons-Moll, Siyu Tang, Michael J. Black*

CAPE --- a generative model and a large-scale dataset for 3D clothed human meshes in varied poses and garment types. 
We trained SCALE using the [CAPE dataset](https://cape.is.tue.mpg.de/dataset), check it out!




## Citations

```bibtex
@inproceedings{Ma:CVPR:2021,
  title = {{SCALE}: Modeling Clothed Humans with a Surface Codec of Articulated Local Elements},
  author = {Ma, Qianli and Saito, Shunsuke and Yang, Jinlong and Tang, Siyu and Black, Michael J.},
  booktitle = {Proceedings IEEE/CVF Conf.~on Computer Vision and Pattern Recognition (CVPR)},
  month = jun,
  year = {2021},
  month_numeric = {6}
}
```

