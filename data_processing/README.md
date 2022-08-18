#### Data prepare to train

### Processing
Run the processing:

    python processing.py -dataset_path  DATASET_PATH 

So, you processed the dataset and have folder with manifests and precalculated features in DATASET_PATH

Aggregates data from the data folder and creates manifests with features in jsonl format

If you want to use tsv format
Run the processing: 

    python processing.py -dataset_path  DATASET_PATH -tsv

You can also change the threshold for aggregation.
    
    python processing.py  -dataset_path  DATASET_PATH -threshold THRESHOLD