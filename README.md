# ENet-keras

[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/PavlosMelissinos/enet-keras/blob/master/LICENSE)
![](https://reposs.herokuapp.com/?path=PavlosMelissinos/enet-keras&style=flat&color=red)

This is an implementation of [ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation](https://arxiv.org/abs/1606.02147), ported from [ENet-training](https://github.com/e-lab/ENet-training) ([lua-torch](https://github.com/torch/torch7)) to [keras](https://github.com/fchollet/keras).


## Requirements

[pycocotools](https://github.com/pdollar/coco)

Pillow

hdf5

[tensorflow](https://www.tensorflow.org/) 

[keras](https://keras.io)


## Installation


### Dependencies


On Anaconda/miniconda: ```conda env create -f environment.yml```


On pip: ```pip install -r requirements.txt```


### pycocotools

```
cd src/data/pycocotools/
make
```

## Preparation

```git clone https://github.com/PavlosMelissinos/enet-keras.git```

### Set up pycocotools
Either set PYTHONPATH accordingly:

```export PYTHONPATH=path/to/MS-COCO/API/PythonAPI``` or

add a symbolic link to the pycocotools directory in the root of the project:

```ln -s path/to/MS-COCO/API/PythonAPI/pycocotools .```

### Prepare data

[Download ms-coco data](http://mscoco.org/dataset/#download):

```
cd data
./download_mscoco.sh
```

### Prepare pretrained ENet model

TODO

## Usage

### Predict

```python src/predict.py path/to/txt/file/containing/image/paths /path/to/h5/model /path/where/predictions/will/be/saved```

### Train on MS-COCO

```./train.sh```



## Remaining tasks

- [ ] Clean up code
  - [ ] Remove hardcoded paths
  - [ ] Add documentation everywhere
- [ ] Test code
  - [ ] Add tests
  - [x] Debug train.py
- [ ] Fix performance (mostly preprocessing bottleneck)
  - [ ] Remove unnecessary computations in data preprocessing
  - [ ] Index dataset category internals. Dataset categories have fields with one-to-one correspondence like id, category_id, palette, categories. This seems like perfect table structure. Might be too much though.
  - [ ] (Optionally) Make data loader multithreaded (no idea how to approach this one, multithreadedness is handled by keras though)
- [ ] Enhance reproducibility/usability
  - [ ] complete dataset preparation
    - [ ] download_mscoco.sh should extract the archives to their appropriate locations
      - [ ] Include symlink creation in Dataset.download()
  - [ ] Upload pretrained model
  - [ ] Finalize predict.py
    - [x] Test whether it works after latest changes
    - [x] Retrain new version of ENet for rgb values
    - [ ] Modify predict.py to load a single image or from a file. There's no point in loading images from the validation set.
  - [x] Add enet version with unpooling instead of naive upsampling
- [ ] Fix bugs
  - [x] steps_per_epoch doesn't correspond to the actual number per epoch when instance crops are used: Annotations that cover a tiny area (less than 50x50) are skipped. This should somehow be computed when the dataset is loaded. Counting it in the dataset constructor and load method might suffice.
  - [ ] Investigate reason for bad results, see [#11](https://github.com/PavlosMelissinos/enet-keras/issues/11)
  - [ ] Fix MSCOCOReduced, [also see #9](https://github.com/PavlosMelissinos/enet-keras/issues/9)
  - [ ] ?????
