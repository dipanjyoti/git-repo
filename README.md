# Swin Transformer

[Link to original Swin Transformer project](https://github.com/microsoft/Swin-Transformer)

## Installation Instructions

1. Set up python packages

```sh
git clone https://github.com/dipanjyoti/git-repo.git
cd git-repo
```


```sh

conda create -n swin python=3.10 -y
conda activate swin
```

CUDA 11.6

```sh
pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 torchaudio==0.12.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116
```

CUDA 11.3

```sh
pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 torchaudio==0.12.1+cu113 --extra-index-url https://download.pytorch.org/whl/cu113
```

Python packages

```sh
pip install matplotlib yacs timm einops black isort flake8 flake8-bugbear termcolor wandb preface opencv-python
```

2. Install Apex

Apex is not needed if you do not want to use fp16.

```sh
git clone https://github.com/NVIDIA/apex.git
cd apex
pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```


3. Download Data

We use the iNat21 dataseta available on [GitHub](https://github.com/visipedia/inat_comp/tree/master/2021)

```
cd /local/scratch/paul.1164/data/inat21/compressed

wget https://ml-inat-competition-datasets.s3.amazonaws.com/2021/train.tar.gz
wget https://ml-inat-competition-datasets.s3.amazonaws.com/2021/val.tar.gz

#unzip the files
tar -xvzf train.tar.gz
tar -xvzf val.tar.gz

# move to raw directory
mv train ../raw/
mv val ../raw/

## store the "inat21_tree.pkl" in the data-path location
```

4. Preprocess iNat 21

Use your root data folder and your size of choice.

```
export DATA_DIR=/mnt/10tb/data/inat21/
python -m data.inat preprocess $DATA_DIR val resize 192
python -m data.inat preprocess $DATA_DIR train resize 192
python -m data.inat preprocess $DATA_DIR val resize 256
python -m data.inat preprocess $DATA_DIR train resize 256
```

5. Login to Wandb

```
wandb login
```

6. Set up multiple runs (use the command to generate .yaml files based on grid/random search):


```
python -m generate_configs.generate --strategy grid path_to_yaml_file_template/template.yaml path_to_yaml_file_template/config_files --no-expand MODEL

# grid path_to_yaml_file_template/template.yaml --> configs/nabird/fuzzy-fig-192/fuzzy-fig-192.yaml
# path_to_yaml_file_template/config_files --> configs/nabird/fuzzy-fig-192/config_files

```

## AWS Helpers

Uninstall v1 of awscli:

```
sudo /usr/local/bin/pip uninstall awscli
```

Install v2:
```
cd ~/pkg
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install --bin-dir ~/.local/bin --install-dir ~/.local/aws-cli
```
