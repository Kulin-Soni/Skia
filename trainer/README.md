# Skia (Model)
This directory contains all the code that was used to build the data, model and other back-end stuff for Skia.

## Folder Structure
- `/labeled_data` : Contains all the data that was used to train the model.
- `/src` : Contains The files which serve
different purpose in the making of the model.

## Train
If you want to train the weights on other comments, please follow the guide.

### Installing Dependencies
Use [`uv`](https://docs.astral.sh/uv) or `pip` to install dependencies
```sh
pip install -r requirements.txt # using pip
uv sync # using uv
```

### Collecting Data
Comments can be easily extracted using Google Cloud API using the `src/extract.py` script.<br />
If you are collecting data manually, only two columns of data - Comment and Quality are required. Quality depends on the context you are training it on.

Store these data files inside the `labeled_data` directory.

|Comment               |Quality|
|----------------------|-------|
|The comment comes here|0.5    |
|Second comment here   |0.9    |

**Comment** - Can be of any length.<br />
**Quality** - `≥0` and `<1`

Additional columns like **Video Id** and **Likes** can also be added, but not really needed for training.

### Training
1. Merge all the comments csv files inside the `labeled_data` directory using the `src/merge.py` script.
2. Use the `src/train.py` script to train a linear regression model using the merged csv file.
3. `regressor.json` file is generated when trained, you can use this in the extension by copying the file to `extension\src\workers\model` as `weights.json`.
4. [Bundle the extension](../extension/README.md) and reload the extension.