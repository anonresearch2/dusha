Once you processed dataset Dusha or downloaded precalculated features, you have a folder with manifests and features in `DATASET_PATH`
(by default `DUSHA_REPOSITORY_PATH/data/paper_setups`)

## Preparing
You can reproduce our experiments using prepared docker image or your python environment.

You should specify `DATASET_PATH` in the variable `base_path` in `./configs/data.config`:
- If you want to use own python environment - define an **absolute** path to processed_dataset folder
- If you want to use proposed docker image - define `DATASET_PATH` relative to `DUSHA_REPOSITORY_PATH` (it will be mounted to the docker as `/workspace`).
  It is already done for default `DATASET_PATH` - `'/workspace/data/paper_setups'`

## Docker
Build the docker image:

```
docker build -t dusha_image .
```

Then run it:

```
export CURRENT_DIR=$PWD
docker run --gpus device=0 -it -v /$CURRENT_DIR/..:/workspace --name dusha_docker dusha_image
```

"Inside" the docker activate python environment:
```
source /venv/bin/activate && export PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python
```

So, we are ready to start!

## Train
We set up experiments in 7 settings, for each of them there is a corresponding config in _configs_ folder.
Run the train:

```
python train.py -config configs/{EXP_NAME}.config -exp_path exps/{EXP_NAME}
```

`podcast_tune` experiment uses pretrained `crowd_large` model to initializate ,
so you have to train this model first and specify a path to the trained model as `pt_model_path` in `podcast_tune.config`.

We have tried to make the experiments as reproducible as possible, so the training script can run longer.
To speed up the training you can change `train.py` by deleting `# fix seeds for reproducibility` part of the code.


## Inference
You have a few trained model in `./exps` folder.
To calculate predicts and metrics for them you should run the command:

```
python inf.py -exps_path exps -vm {PATH_TO_TESTS_FOLDER}
```

Where `{PATH_TO_TESTS_FOLDER}` is a path to test manifests folder (`base_path / 'test'` from `./configs/data.config`).

The script will also calculate pivot tables with metrics grouped by dataset and dump it in `exps/metrics/exps_{dataset_name}.csv`.

See `--help` for more information.
