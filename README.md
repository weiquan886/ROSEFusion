# ROSEFusion :rose:

This project is based on our SIGGRAPH 2021 paper, [ROSEFusion: **R**andom **O**ptimization for Online Den**SE** Reconstruction under Fast Camera Motion
](https://arxiv.org/abs/2105.05600).



## Introduction

ROSEFsuion is proposed to tackle the difficulties in fast-motion camera tracking using random optimization with depth information only. Our method attains good quality pose tracking under fast camera motion in a realtime framerate without including loop closure or global pose optimization.

 <p id="demo1" align="center"> 
  <img src="assets/intro.gif" />
 </p>

## Installation
The code is based on C++ and CUDA with the support of:
- [Pangolin](https://github.com/stevenlovegrove/Pangolin)
- OpenCV with CUDA (v.4.5 is required, for instance you can follow the [link](https://gist.github.com/raulqf/f42c718a658cddc16f9df07ecc627be7))  
- Eigen
- CUDA (v.11 and above is required)

Before building, please make sure the architecture ```(sm_xx and compute_xx)``` in the [L22 of CMakeLists.txt](CMakeLists.txt#L22) is compatible with your own graphics card.


Our code has been tested with Nvidia GeForce RTX 2080 SUPER on Ubuntu 16.04. 

## [Option] Test with Docker

We have already upload a docker image with all the lib, code and data. Please download the image from the [google drive](https://drive.google.com/file/d/1sNvm8vSJM5MxxDpgkqDo-FhpwXBdraMb/view?usp=sharing).

### **Prepare**
Make sure you have successfully installed the [docker](https://www.docker.com/) and [nvidia docker](github.com/NVIDIA/nvidia-docker). Once the environment is ready, you can using following commands to boot the docker image:
```
sudo docker load -i rosefusion_docker.tar 
sudo docker run -it  --gpus all jiazhao/rosefusion:v7 /bin/bash
```


And please check the architecture in the L22 of ``` /home/code/ROSEFusion-main/CMakeList.txt``` is compatible with your own graphics card. If not, change the sm_xx and compute_xx, then rebuild the ROSEFusion.


### **QuickStart**
All the data and configuration files are ready for using. You can find "run_example.sh" and "run_stairwell.sh" in ```/home/code/ROSEFusion-main/build```. After running the scripts, the trajectory and reconstuciton results would be generated in ```/home/code/rosefusion_xxx_data```. 



## Configuration File
We use the following configuration files to make the parameters setting easier. There are four types of configuration files.

- **seq_generation_config.yaml:** data information 
- **camera_config.yaml:** camera and image information.
- **data_config.yaml:** output path, sequence file path and parameters of the volume.
- **controller_config.yaml:** visualization, saving and parameters of tacking.

The **seq_generation_config.yaml** is only used in data preparation, and the other three types of configuration files are necessary to run the fusion part. The configuration files of many common datasets are given in `[type]_config/` directory, you can change the settings to fit your own dataset.

## Data Preparation
The details of data prepartiation can be found in [src/seq_gen.cpp](src/seq_gen.cpp). By using the *seq_generation_config.yaml* introduced above, you can run the program:
```
./seq_gen  sequence_information.yaml
```
Once finished, there will be a `.seq` file containing all the information of the sequence.


## Particle Swarm Template
We share the same pre-sampled PST as we used in our paper. Each PST is saved as an N×6 image and the N represents the number of particles. You can find the ``.tiff`` images in [PST dicrectory](/PST), and please prelace the PST path in ``controller_config.yaml `` with your own path.

## Running
To run the fusion code, you need to provide the `camera_config.yaml`, `data_config.yaml` and `controller_config.yaml`. We already share configuration files of many common datasets in `./camera_config`, `./data_config`, `/controller_config`. All the parameters of configuration can be modified as you want. With all the preparation done, you can run the code below:
```
./ROSEFsuion  your_camera_config.yaml your_data_config.yaml your_controller_config.yaml
```
For a quick start, you can download and use a small size synthesis [seq file and related configuration files](https://drive.google.com/drive/folders/1vW5GV2xsJN1kIrl-5JZX1fUrpjCtp5AS?usp=sharing). Here is a preview.


 <p id="demo1" align="center"> 
  <img src="assets/example.gif" />
 </p>

## FastCaMo Dataset
We present the **Fast** **Ca**mera **Mo**tion dataset, which contains both synthesis and real captured sequences. You are welcome to download the sequences and take a try.

### FastCaMo-Synth
With 10 diverse room-scale scenes from [Replica Dataset](https://github.com/facebookresearch/Replica-Dataset), we render the color images and depth maps along the synthesis trajectories. The raw sequences are provided in [FastCaMo-synth-data(raw).zip](https://drive.google.com/file/d/15PG7jd1wFdf26zaPp-pq04RAoCRQA1qt/view?usp=sharing), and we also provide the [FastCaMo-synth-data(noise).zip](https://drive.google.com/file/d/1a8QLimLFvteac6OfGPxSsTqITrJSC2ox/view?usp=sharing) with synthesis noise. We use the same noise model as [simkinect](https://github.com/ankurhanda/simkinect). For evaluation, you can download the ground truth [trajectories](https://drive.google.com/file/d/106p9N99K-X3_jbt8PRcKthySbpxhIwxB/view?usp=sharing).

### FastCaMo-Real
There are 12 [real captured RGB-D sequences](https://drive.google.com/drive/folders/1kDUz_Vxjy5zi5LO8G5HwjkZ0WbetsBy1?usp=sharing) with fast camera motions are released. Each sequence is recorded in a challenging scene like gym or stairwell by using [Azure Kinect DK](https://azure.microsoft.com/en-us/services/kinect-dk/). We offer a full and dense reconstruction  scanned using the high-end laser scanner, serving as ground truth. However, The original file is extremely large, we will share the dense reconstruction in another platform or release the sub-sampled version only.

 <p id="demo1" align="center"> 
  <img src="assets/fastcamo-real.gif" />
 </p>

## Citation
If you find our work useful in your research, please consider citing:
```
@article {zhang_sig21,
    title = {ROSEFusion: Random Optimization for Online Dense Reconstruction under Fast Camera Motion},
    author = {Jiazhao Zhang and Chenyang Zhu and Lintao Zheng and Kai Xu},
    journal = {ACM Transactions on Graphics (SIGGRAPH 2021)},
    volume = {40},
    number = {4},
    year = {2021}
}
```

## Acknowledgments
Our code is inspired by [KinectFusionLib](https://github.com/chrdiller/KinectFusionLib).

This is an open-source version of ROSEFusion, some functions have been rewritten to avoid certain license. It would not be expected to reproduce the result exactly, but the result is almost the same.
## License
The source code is released under [GPLv3](https://www.gnu.org/licenses/gpl-3.0.html) license.

## Contact
If you have any questions, feel free to email Jiazhao Zhang at zhngjizh@gmail.com.

