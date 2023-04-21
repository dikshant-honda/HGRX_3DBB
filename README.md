
## Installation

* Clone the repository and cd to it.

```bash
git clone https://github.com/AKITOSHI8186/HGRX_3DBB.git HGRX_3DBB
cd HGRX_3DBB
```
* (Optional, but recommended) Create virtual environment for this project - you can use **virtualenvwrapper** or following commands. **IMPORTANT NOTE:** this project is using **Python3**.

```bash
virtuenv -p /usr/bin/python3 HGRX_3DBB_venv
source HGRX_3DBB_venv/bin/activate
```

* Install required packages:

```bash
pip3 install -r requirements.txt 
```

* Manually download dataset https://drive.google.com/file/d/19LHLOmmVyUS1R4ypwByfrV8KQWnz2GDT/view and unzip it.
* Modify `scripts/config.py` and change `BOXCARS_DATASET_ROOT` to directory where is the unzipped dataset.


## Usage
### Fine-tuning of the Models
To fine-tune a model use `scripts/train_eval.py` (use -h for help). Example for ResNet50:
```bash
python3 scripts/train_eval.py --train-net ResNet50 
```
It is also possible to resume training using `--resume` argument for `train_eval.py`.

### Evaluation
The model is evaluated when the training is finished, however it is possible to evaluate saved model by running:
```bash
python3 scripts/train_eval.py --eval path-to-model.h5
```

## dataset
The dataset was created for the paper and it is possible to download it from our (https://drive.google.com/file/d/19LHLOmmVyUS1R4ypwByfrV8KQWnz2GDT/view)
The dataset contains 116k of images of vehicles with fine-grained labels taken from surveillance cameras under various viewpoints. 
The dataset contains tracked vehicles with the same label and multiple images per track. The track is uniquely identified by its id `vehicle_id`, while each image is uniquely identified by `vehicle_id` and `instance_id`. It is possible to use class `BoxCarsDataset` from `lib/boxcars_dataset.py` for working with the dataset; however, for convenience, we describe the structure of the dataset also here. 
The dataset contains several files and folders:
* **images** - dataset images and masks 
* **atlas.pkl** - *BIG* structure with jpeg encoded images, which can be convenient as the whole structure fits the memory and it is possible to get the images on the fly. To load the atlas (or any other pkl file), you can use function `load_cache` from `lib/utils.py`. To decode the image (in RGB channel order), use the following statement.
```python
atlas = load_cache(path_to_atlas_file)
image = cv2.cvtColor(cv2.imdecode(atlas[vehicle_id][instance_id], 1), cv2.COLOR_BGR2RGB)
```

* **dataset.pkl** - contains dictionary with following fields:
```
cameras: information about used cameras (vanishing points, principal point)
samples: list of vehicles (index correspons to vehicle id). 
		 The structure contains several fields which should understandable. 
		 It also contains field instances with list of of dictionaries 
		 with information about images of the vehicle track. 
		 The flag to_camera defines whether the vehicle is going towards camera or not. 
```

* **classification_splits.pkl** - different splits (*hard*, *medium* from paper and additional *body* and *make* split). Each split contains structure `types_mapping` definig mapping from textual labels to integer labels. It also contains fields `train`, `test`, and `validation` which are lists and each element contains tuple `(vehicle_id, class_id)`.

* **verification_splits.pkl** - similar to classification splits; however, the elements in `train`, `test` are triplets `(vehicle_id1, vehicle_id2, class_id)`.

* **json_data** and **matlab_data** - converted pkl file


----------------------------------------------------------------------------------------------------------------------------------------------------------------
## ## Installation

* Clone the repository and cd to it.

```bash
git clone https://github.com/dikshant-honda/HGRX_3DBB.git
cd HGRX_3DBB
```
* (RECOMMENDED) Create virtual environment for this project using anaconda (make sure anaconda is already installed)
```bash
conda create -n venv python=3.9
conda activate venv
```

* Install required packages:

```bash
pip install tensorflow-gpu==2.10.0
pip install opencv-python
pip install pyparsing pyyaml theano
```

* Manually download dataset https://drive.google.com/file/d/19LHLOmmVyUS1R4ypwByfrV8KQWnz2GDT/view and unzip it.
* Modify `scripts/config.py` and change `BOXCARS_DATASET_ROOT` to directory where is the unzipped dataset.

* `pip list` should look like this:
```bash
Package                      Version
---------------------------- ---------
absl-py                      1.4.0
astunparse                   1.6.3
cachetools                   5.3.0
certifi                      2022.12.7
charset-normalizer           3.1.0
flatbuffers                  23.3.3
gast                         0.4.0
google-auth                  2.17.3
google-auth-oauthlib         0.4.6
google-pasta                 0.2.0
grpcio                       1.54.0
h5py                         3.8.0
idna                         3.4
importlib-metadata           6.5.0
keras                        2.10.0
Keras-Preprocessing          1.1.2
libclang                     16.0.0
Markdown                     3.4.3
MarkupSafe                   2.1.2
numpy                        1.24.2
oauthlib                     3.2.2
opencv-python                4.7.0.72
opt-einsum                   3.3.0
packaging                    23.1
pip                          23.0.1
protobuf                     3.19.6
pyasn1                       0.5.0
pyasn1-modules               0.3.0
requests                     2.28.2
requests-oauthlib            1.3.1
rsa                          4.9
scipy                        1.10.1
setuptools                   66.0.0
six                          1.16.0
tensorboard                  2.10.1
tensorboard-data-server      0.6.1
tensorboard-plugin-wit       1.8.1
tensorflow-estimator         2.10.0
tensorflow-gpu               2.10.0
tensorflow-io-gcs-filesystem 0.31.0
termcolor                    2.2.0
Theano                       1.0.5
typing_extensions            4.5.0
urllib3                      1.26.15
Werkzeug                     2.2.3
wheel                        0.38.4
wrapt                        1.15.0
zipp                         3.15.0
```

* CUDA env variables: CUDA_PATH_V11_2 OR CUDA_PATH_V8_0
