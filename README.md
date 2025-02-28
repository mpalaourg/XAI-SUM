# Explaining Video Summarization Based on the Focus of Attention

## PyTorch Implementation of our Attention-based Method for Explainable Video Summarization [[Paper](https://mpalaourg.me/en/media/files/ism2022_preprint.pdf)] [[DOI](https://doi.org/10.1109/ISM55400.2022.00029)] [[Cite](https://github.com/mpalaourg/XAI-SUM#citation)]
- From **"Explaining Video Summarization Based on the Focus of Attention"**, Proc. of the IEEE Int. Symposium on Multimedia (ISM), Dec. 2022.
- Written by Evlampios Apostolidis, Georgios Balaouras, Vasileios Mezaris and Ioannis Patras
- This software can be used for studying our method for producing attention-based explanations for the outcomes of the [CA-SUM](https://github.com/e-apostolidis/CA-SUM) model for video summarization, and re-producing the reported exprerimental results in our paper. 

## Main dependencies
Developed, checked and verified on an `Ubuntu 20.04.5` PC with an `NVIDIA RTX 2080Ti` GPU and an `i5-11600K` CPU. Main packages required:
|`Python` | `PyTorch` | `CUDA Version` | `cuDNN Version` | `NumPy` | `H5py`
:---:|:---:|:---:|:---:|:---:|:---:|
3.8(.8) | 1.7.1 | 11.0 | 8005 | 1.20.2 | 2.10.0

## Data
<div align="justify">

Structured h5 files with the video features and annotations of the SumMe and TVSum datasets are available within the [data](data) folder. The GoogleNet features of the video frames were extracted by [Ke Zhang](https://github.com/kezhang-cs) and [Wei-Lun Chao](https://github.com/pujols) and the h5 files were obtained from [Kaiyang Zhou](https://github.com/KaiyangZhou/pytorch-vsumm-reinforce). These files have the following structure:
```Text
/key
    /features                 2D-array with shape (n_steps, feature-dimension)
    /gtscore                  1D-array with shape (n_steps), stores ground truth importance score (used for training, e.g. regression loss)
    /user_summary             2D-array with shape (num_users, n_frames), each row is a binary vector (used for test)
    /change_points            2D-array with shape (num_segments, 2), each row stores indices of a segment
    /n_frame_per_seg          1D-array with shape (num_segments), indicates number of frames in each segment
    /n_frames                 number of frames in original video
    /picks                    positions of sub-sampled frames in original video
    /n_steps                  number of sub-sampled frames
    /gtsummary                1D-array with shape (n_steps), ground truth summary provided by user (used for training, e.g. maximum likelihood)
    /video_name (optional)    original video name, only available for SumMe dataset
```
Original videos and annotations for each dataset are also available in the dataset providers' webpages: 
- <a href="https://github.com/yalesong/tvsum" target="_blank"><img align="center" src="https://img.shields.io/badge/Dataset-TVSum-green"/></a> <a href="https://gyglim.github.io/me/vsum/index.html#benchmark" target="_blank"><img align="center" src="https://img.shields.io/badge/Dataset-SumMe-blue"/></a>
</div>

## Running an experiment
<div align="justify">

To run an experiment using one of the aforementioned datasets and considering all of its randomly-generated splits (stored in the JSON file included in the [data/splits](/data/splits) directory), execute the following command:

```
python model/main.py --dataset 'dataset_name' --replacement_method 'repl_function_name' --replaced_fragments 'set_of_repl_fragments' --visual_mask 'mask_name'
```
where, `dataset_name` refers to the name of the used dataset, `repl_function_name` refers to the applied replacement function in fragments of the input data, `set_of_repl_fragments` refers to the amount of replaced fragments of the input data, and `mask_name` refers to the type of the used mask for replacing fragments of the input data.

After executing the above command you get the results for each different data split, as well as the overall results that are computed by averaging the obtained scores across data splits. The overall results correspond to the ones reported in Table 1 of our paper for the different replacement functions. Please note that, the results when fragments' replacement is based on "Randomization" might be slightly different from the reported ones, as we did not use a fixed seed value in our experiments.

## Configurations
<div align="justify">

Setup for the experimental evaluation:
 - In [`main.py`](model/main.py#L63), specify the path to the pretrained models of the CA-SUM network for video summarization. 
 - In [`data_loader.py`](model/data_loader.py#L19:L20), specify the paths to the h5 file of the used dataset, and the JSON file containing data about the used data splits.</div>
   
Arguments in [`configs.py`](model/configs.py): 
|Parameter name | Description | Default Value | Options
| :--- | :--- | :---: | :---:
`dataset` | Used dataset in experiments. | 'SumMe' | 'SumMe', 'TVSum'
`replacement_method` | Applied replacement function. | 'slice-out' | 'slice-out', 'input-mask', 'random', 'attention-mask'
`replaced_fragments` | Amount of replaced fragments. | 'batch' | 'batch', 'single'
`visual_mask` | Visual mask used for replacement. | 'black-frame' | 'black-frame', 'white-frame'

## Citation
<div align="justify">
    
If you find our work, code or pretrained models, useful in your work, please cite the following publication:

E. Apostolidis, G. Balaouras, V. Mezaris, I. Patras, "<b>Explaining Video Summarization Based on the Focus of Attention</b>", Proc. IEEE Int. Symposium on Multimedia (ISM), Dec. 2022.
</div>

BibTeX:

```
@INPROCEEDINGS{9666088,
    author    = {Apostolidis, Evlampios and Balaouras, Georgios and Mezaris, Vasileios and Patras, Ioannis},
    title     = {Explaining Video Summarization Based on the Focus of Attention},
    booktitle = {2022 IEEE International Symposium on Multimedia (ISM)},
    month     = {December},
    year      = {2022}
}
```

## License
<div align="justify">

Copyright (c) 2022, Evlampios Apostolidis, Georgios Balaouras, Vasileios Mezaris, Ioannis Patras / CERTH-ITI. All rights reserved. This code is provided for academic, non-commercial use only. Redistribution and use in source and binary forms, with or without modification, are permitted for academic non-commercial use provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation provided with the distribution.

This software is provided by the authors "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed. In no event shall the authors be liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including, but not limited to, procurement of substitute goods or services; loss of use, data, or profits; or business interruption) however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use of this software, even if advised of the possibility of such damage.
</div>

## Acknowledgement
<div align="justify"> This work was supported by the EU Horizon 2020 programme under grant agreement H2020-951911 AI4Media. </div>
