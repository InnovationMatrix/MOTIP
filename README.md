# Multiple Object Tracking as ID Prediction

This is the official PyTorch implementation of our paper:

> ***[Multiple Object Tracking as ID Prediction](https://arxiv.org/abs/2403.16848)*** <br>
> :mortar_board: [Ruopeng Gao](https://ruopenggao.com/), Yijun Zhang, [Limin Wang](https://wanglimin.github.io/) <br>
> :e-mail: Primary contact: ruopenggao@gmail.com

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/multiple-object-tracking-as-id-prediction/multi-object-tracking-on-dancetrack)](https://paperswithcode.com/sota/multi-object-tracking-on-dancetrack?p=multiple-object-tracking-as-id-prediction)

## Overview :mag:

**TL; DR.** MOTIP proposes a new perspective to regard the multi-object tracking task as an ID prediction problem. 
It directly predicts the ID labels for each object in the tracking process, which is more straightforward and effective.

![Overview](./assets/overview.png)

**Abstract.** In Multiple Object Tracking (MOT), tracking-by-detection methods have stood the test for a long time, which split the process into two parts according to the definition: object detection and association. They leverage robust single-frame detectors and treat object association as a post-processing step through hand-crafted heuristic algorithms and surrogate tasks. However, the nature of heuristic techniques prevents end-to-end exploitation of training data, leading to increasingly cumbersome and challenging manual modification while facing complicated or novel scenarios. In this paper, we regard this object association task as an End-to-End in-context ID prediction problem and propose a streamlined baseline called MOTIP. Specifically, we form the target embeddings into historical trajectory information while considering the corresponding IDs as in-context prompts, then directly predict the ID labels for the objects in the current frame. Thanks to this end-to-end process, MOTIP can learn tracking capabilities straight from training data, freeing itself from burdensome hand-crafted algorithms. Without bells and whistles, our method achieves impressive state-of-the-art performance in complex scenarios like DanceTrack and SportsMOT, and it performs competitively with other transformer-based methods on MOT17. We believe that MOTIP demonstrates remarkable potential and can serve as a starting point for future research.


## News :fire:

- <span style="font-variant-numeric: tabular-nums;">*2024.03.28*</span>: We release the inference code, you can evaluate the model following the [instructions](#evaluation) :tada:. Our model weights and logs are available in the [Google Drive](https://drive.google.com/drive/folders/1LTBWHLHhrF0Ro7fgCdAkgu9sJUV_y-vw?usp=drive_link) :cloud:.

- <span style="font-variant-numeric: tabular-nums;">*2024.03.26*</span>: The paper is released on [arXiv](https://arxiv.org/abs/2403.16848), the code will be available in several days :soon:.


## Main Results :chart_with_upwards_trend:

### DanceTrack :dancer:

| Method              | Training Data       | HOTA | DetA | AssA | MOTA | IDF1 | URLs                                                         |
| ------------------- | ------------------- | ---- | ---- | ---- | ---- | ---- | ------------------------------------------------------------ |
| MOTIP               | DT                  | 67.5 | 79.4 | 57.6 | 90.3 | 72.2 | [model](https://drive.google.com/file/d/1qNGN7RsDf6a3i5lwjb0V8v6mKzxaMh0G/view?usp=drive_link), [config](./configs/r50_deformable_detr_motip_dancetrack.yaml), [log](https://drive.google.com/file/d/1XRRBjw92bQk7FUGxmZSsrTf5BjXbL2pp/view?usp=drive_link) |
| MOTIP<sub>DAB</sub> | DT                  | 70.0 | 80.8 | 60.8 | 91.0 | 75.1 | *TBD*                                                        |
| MOTIP               | DT + CH             | 71.4 | 81.3 | 62.8 | 91.6 | 76.3 | *TBD*                                                        |
| MOTIP               | DT<sup>*</sup> + CH | 73.7 | 82.6 | 65.9 | 92.7 | 78.4 | *TBD*                                                        |

<details>
  <summary><i>NOTE</i></summary>
  <ol>
    <li>MOTIP is built upon original Deformable DETR, while MOTIP<sub>DAB</sub> is based on DAB-Deformable DETR.</li>
    <li>DT and CH are the abbreviations of DanceTrack and CrowdHuman respectively.</li>
    <li>DT<sup>*</sup> denotes we utilize both the training and validation set of DanceTrack for training.</li>
  </ol>
</details>


### SportsMOT :basketball:

| Method | Training Data | HOTA | DetA | AssA | MOTA | IDF1 | URLs                                                         |
| ------ | ------------- | ---- | ---- | ---- | ---- | ---- | ------------------------------------------------------------ |
| MOTIP  | Sports        | 71.9 | 83.4 | 62.0 | 92.9 | 75.0 | [model](https://drive.google.com/file/d/1NIw77CBt8xEoZxHrUg14vrPYBCXUUgq-/view?usp=drive_link), [config](./configs/r50_deformable_detr_motip_sportsmot.yaml), [log](https://drive.google.com/file/d/1SNZ60uxVCdU5Poza0fXztWSGaZifVdaD/view?usp=drive_link) |

<details>
  <summary><i>NOTE</i></summary>
  <ol>
    <li>Sports is the abbreviation of SportsMOT.</li>
  </ol>
</details>

### MOT17 :walking:

| Method | Training Data | HOTA | DetA | AssA | MOTA | IDF1 | URLs                                                         |
| ------ | ------------- | ---- | ---- | ---- | ---- | ---- | ------------------------------------------------------------ |
| MOTIP  | MOT17 + CH    | 59.2 | 62.0 | 56.9 | 75.5 | 71.2 | [model](https://drive.google.com/file/d/1ZsojRYBCbH9u9m1C5leb1MwmBB42sox8/view?usp=drive_link), [config](./configs/r50_deformable_detr_motip_mot17.yaml), [log](https://drive.google.com/file/d/1RB0XasyMMJFziB5wuyT208jMBLW37CPM/view?usp=drive_link) |

<details>
  <summary><i>NOTE</i></summary>
  <ol>
    <li>CH is the abbreviation of CrowdHuman.</li>
  </ol>
</details>


## Quick Start :dash:

<details>
<summary><strong>Dependencies Install</strong></summary>

```bash
# Suggest python version >= 3.10
conda create -n MOTIP python=3.11
conda activate MOTIP
# Now we only support pytorch version >= 2.0, we will support pytorch version <= 1.13 in the future
conda install pytorch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 pytorch-cuda=11.8 -c pytorch -c nvidia
# Other dependencies
conda install matplotlib pyyaml scipy tqdm tensorboard seaborn scikit-learn pandas
pip install opencv-python einops wandb pycocotools timm
# Compile the Deformable Attention
cd models/ops/
sh make.sh
```

</details>




<details>
<summary><strong>Data Preparation</strong></summary>

You can download the datasets from the following links:
- [DanceTrack](https://github.com/DanceTrack/DanceTrack)
- [SportsMOT](https://github.com/MCG-NJU/SportsMOT)
- [MOT17](https://motchallenge.net/data/MOT17/)
- [CrowdHuman](https://www.crowdhuman.org/)

Then, you need to unzip and organize the data as follows:

```
DATADIR/
  ├── DanceTrack/
  │ ├── train/
  │ ├── val/
  │ ├── test/
  │ ├── train_seqmap.txt
  │ ├── val_seqmap.txt
  │ └── test_seqmap.txt
  ├── SportsMOT/
  │ ├── train/
  │ ├── val/
  │ ├── test/
  │ ├── train_seqmap.txt
  │ ├── val_seqmap.txt
  │ └── test_seqmap.txt
  ├── MOT17/
  │ ├── images/
  │ │ ├── train/     # unzip from MOT17
  │ │ └── test/      # unzip from MOT17
  │ └── gts/
  │   └── train/     # generate by ./data/gen_mot17_gts.py
  └── CrowdHuman/
    ├── images/
    │ ├── train/     # unzip from CrowdHuman
    │ └── val/       # unzip from CrowdHuman
    └── gts/
      ├── train/     # generate by ./data/gen_crowdhuman_gts.py
      └── val/       # generate by ./data/gen_crowdhuman_gts.py
```

For MOT17 and CrowdHuman, you can generate the ground-truth files by running the corresponding scripts [gen_mot17_gts.py](./data/gen_mot17_gts.py) and [gen_crowdhuman_gts.py](./data/gen_crowdhuman_gts.py).

</details>


<details id="evaluation">
<summary><strong>Evaluate the model</strong></summary>

- **Get tracking results for submitting**:
  ```bash
  python -m torch.distributed.run --nproc_per_node=<gpu num> main.py --mode submit --use-distributed True --use-wandb False --config-path <config file path> --inference-model <checkpoint path> --outputs-dir <outputs dir> --inference-dataset <dataset name> --inference-split <dataset split>
  ```
  For example, you can submit the model on DanceTrack test set as follows:
  ```bash
  python -m torch.distributed.run --nproc_per_node=8 main.py --mode submit --use-distributed True --use-wandb False --config-path ./configs/r50_deformable_detr_motip_dancetrack.yaml --inference-model ./outputs/r50_deformable_detr_motip_dancetrack.pth --outputs-dir ./outputs/dancetrack_trackers/ --inference-dataset DanceTrack --inference-split test
  ```

</details>