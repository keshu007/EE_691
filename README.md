## Installation

- Environment: Linux / CUDA 11.3 / Python 3.9
- Create a virtual environment

  ```bash
  conda create -n scmil python=3.9
  source activate scmil
  ```
- Install pytorch

  ```
  pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 --extra-index-url https://download.pytorch.org/whl/cu113
  ```
- Install cuML

  ```bash
  spip install --extra-index-url=https://pypi.nvidia.com cudf-cu11==23.10.0 cuml-cu11==23.10.0
  ```
- Other requirements

  ```bash
  pip install tqdm
  pip install lifelines
  pip install munch
  pip install tensorboardX
  pip install einops
  pip install h5py
  pip install seaborn
  ```
  ## Download dataset and Pretrain weights

1. Download diagnostic WSIs and clinical information from [TCGA](https://portal.gdc.cancer.gov/)
2. Use the WSI processing tool provided by [CLAM](https://github.com/mahmoodlab/CLAM) to crop WSIs into 256 × 256 patches at 20x magnification.
3. Use [vit](https://github.com/lunit-io/benchmark-ssl-pathology#pre-trained-weights) that pre-trained on a large-scale collection of WSIs using self-supervised learning to extract features of patches.

## Train

1. Prepare dataset annotation file
   To facilate data loading, you should prepare your own 'wsi_annos_vit-s-dino-p16.txt'

   ```bash
   data
    ├── kirc
    │   ├── 5fold_wsi-rnaseq
    │   │   ├── fold1
    │   │   │   ├── train.txt
    │   │   │   └── val.txt
    │   │   ├── ...
    │   │   └── fold5
    │   ├── clinical.csv
    │   └── wsi_annos_vit-s-dino-p16.txt
    └── luad
        ├── 5fold_wsi-rnaseq
        │   ├── fold1
        │   │   ├── train.txt
        │   │   └── val.txt
        │   ├── ...
        │   └── fold5
        ├── clinical.csv
        └── wsi_annos_vit-s-dino-p16.txt
   ```

  wsi_annos_vit-s-dino-p16.txt

```bash
patient_id,wsi_id,label,feature_h5_path
 ...
```

2. Train
   ```bash
   # train kirc
   python train.py --config configs/kirc_scmil.py
   python train.py --config configs/kirc_amil.py

   # train luad
   python train.py --config configs/luad_scmil.py
   python train.py --config configs/luad_amil.py
   ```

## Visual results

Plot survival probability distribution
   ```
   python plot_survival_probability.py --config configs/kirc_scmil.yaml
   ```
