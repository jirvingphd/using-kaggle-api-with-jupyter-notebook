# Using Kaggle API Python Client with Jupyter

- Kaggle API Package: https://github.com/Kaggle/kaggle-api 


- Official API Credential Instructions
    - https://github.com/Kaggle/kaggle-api#api-credentials

## Creating a Kaggle API Token

>- Please follow the steps below to download and use kaggle data within Python. (Original Source: https://github.com/Kaggle/kaggle-api#api-credentials)
1. Log into www.kaggle.com 


2. Go to your Account page <br>(Click on your profile pic in top right corner of website and select Account)


3. Scroll down to API section and:
     - Click Expire API Token to remove previous tokens.
     - Click on **Create New API Token** 
         - It will download `kaggle.json` file on your machine that needs to be moved to a special "~/.kaggle" folder.


4. Make the `~/.kaggle/` folder.
    - Short Relative Filepath version:
        - `mkdir ~/.kaggle/`
    - Full Absolute Filepath Version:
        - will be something like:   
        `mkdir "/Users/YOUR-USERNAME/.kaggle/`
        - or: `mkdir "/c/Users/YOUR-USERNAME/.kaggle"`


        
5. Move the kaggle.json file to the new `.kaggle` folder.
    - See the ["API Credentials" section of the kaggle api README for details](https://github.com/Kaggle/kaggle-api#api-credentials)
    - Example shell command:
    `cp ~/Downloads/kaggle.json ~/.kaggle/`
    
    
    
6. Change access permissions to just your user account
    `chmod 600 ~/.kaggle/kaggle.json`
    
    
7. Confirm the API credentials work.
    - Run the command to list datasets:
    `kaggle datasets list`
    
    
8. Remove the kaggle.json from the downloads folder:
    `rm ~/kaggle.json`
    


# Downloading Data with the Kaggle API

## From the Shell

>- On any dataset listing on Kaggle, click on the `...` on the menu and select "Copy API Command"
  - paste the command in a cell and add a `!` to the beginning.
    - e.g. `!kaggle datasets download -d jillanisofttech/fake-or-real-news`
  - Then, run `!unzip` on the name of the dataset source (the last part of the api command).
    - e.g. `!unzip fake-or-real-news.zip` 

### Example Shell Command for Downloading a Dataset
```python
!kaggle datasets download -d jillanisofttech/fake-or-real-news
!unzip fake-or-real-news
```

## Using Kaggle Api - From Jupyter


```python
## Using the kaggle.api          
import kaggle.api as kaggle     
kaggle.authenticate()
```


```python
## Folder for downloaded dataset
import os, glob
path = "Data/"
os.makedirs(path, exist_ok=True)
```

- On any dataset listing on Kaggle, click on the ... on the menu and select "Copy API Command"


```python
## Paste in API command from kaggle
API_COMMAND = "kaggle datasets download -d catherinerasgaitis/mxmh-survey-results"

## Get just the dataset name
dataset_name = API_COMMAND.split(' -d ')[1]
print(f"Kaggle Dataset name = '{dataset_name}'")

data_fpath = f"{path}{dataset_name.split('/')[-1]}/"
os.makedirs(data_fpath, exist_ok=True)
data_fpath
```

    Kaggle Dataset name = 'catherinerasgaitis/mxmh-survey-results'





    'Data/mxmh-survey-results/'



### Using `kaggle.dataset_download_files`

```
Signature:
kaggle.dataset_download_files(
    dataset,
    path=None,
    force=False,
    quiet=True,
    unzip=False,
)
Docstring:
download all files for a dataset

Parameters
==========
dataset: the string identified of the dataset
         should be in format [owner]/[dataset-name]
path: the path to download the dataset to
force: force the download if the file already exists (default False)
quiet: suppress verbose output (default is True)
unzip: if True, unzip files upon download (default is False)
```


```python
 ## Download dataset
kaggle.dataset_download_files(dataset_name, 
                              path=data_fpath,
                              force=True,
                              unzip=True)
```


```python
## Get list of downloaded files
print(f"[i] Files from {dataset_name}:")

dl_files = glob.glob(data_fpath+"*")
[print(f"  - {file}") for file in dl_files];
```

    [i] Files from catherinerasgaitis/mxmh-survey-results:
      - Data/mxmh-survey-results/mxmh_survey_results.csv



```python
import pandas as pd
df = pd.read_csv(dl_files[0])
df.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>Age</th>
      <th>Primary streaming service</th>
      <th>Hours per day</th>
      <th>While working</th>
      <th>Instrumentalist</th>
      <th>Composer</th>
      <th>Fav genre</th>
      <th>Exploratory</th>
      <th>Foreign languages</th>
      <th>...</th>
      <th>Frequency [R&amp;B]</th>
      <th>Frequency [Rap]</th>
      <th>Frequency [Rock]</th>
      <th>Frequency [Video game music]</th>
      <th>Anxiety</th>
      <th>Depression</th>
      <th>Insomnia</th>
      <th>OCD</th>
      <th>Music effects</th>
      <th>Permissions</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8/27/2022 19:29:02</td>
      <td>18.0</td>
      <td>Spotify</td>
      <td>3.0</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Latin</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>...</td>
      <td>Sometimes</td>
      <td>Very frequently</td>
      <td>Never</td>
      <td>Sometimes</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>I understand.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8/27/2022 19:57:31</td>
      <td>63.0</td>
      <td>Pandora</td>
      <td>1.5</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>Rock</td>
      <td>Yes</td>
      <td>No</td>
      <td>...</td>
      <td>Sometimes</td>
      <td>Rarely</td>
      <td>Very frequently</td>
      <td>Rarely</td>
      <td>7.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>I understand.</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8/27/2022 21:28:18</td>
      <td>18.0</td>
      <td>Spotify</td>
      <td>4.0</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Video game music</td>
      <td>No</td>
      <td>Yes</td>
      <td>...</td>
      <td>Never</td>
      <td>Rarely</td>
      <td>Rarely</td>
      <td>Very frequently</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>10.0</td>
      <td>2.0</td>
      <td>No effect</td>
      <td>I understand.</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 33 columns</p>
</div>



___

# Using Kaggle API on Google Colab

[Using the Kaggle API on Colab](https://www.kaggle.com/general/74235)

>- Please follow the steps below to download and use kaggle data within Google Colab: <br> (Instructions adapted from [Using the Kaggle API on Colab](https://www.kaggle.com/general/74235))
    1. Go to your account, Scroll to API section and **Click Expire API Token** to remove previous tokens.
    2. Click on **Create New API Token** - It will download `kaggle.json` file on your machine.
    3. Go to your Google Colab project file and run the following commands:
        1. `!pip install -q kaggle`
        2. `from google.colab import files`<br>`files.upload();`
            - Choose the kaggle.json file that you downloaded
        3. `!mkdir ~/.kaggle`<br>`!cp kaggle.json ~/.kaggle/`
            - Make directory named kaggle and copy kaggle.json file there.
        4. `!chmod 600 ~/.kaggle/kaggle.json`
            - Change the permissions of the file.
        5. Test the installation using: `! kaggle datasets list`
        6. Remove the uploaded kaggle.json: 
            -  `!rm kaggle.json`

> 

#### Uncomment the cells below for Colab


```python
# ## Install kaggle api and upkoad kaggle.json
# ! pip install kaggle
# from google.colab import files
# files.upload();
```


```python
# ## Make .kaggle directory and copy json fil
# ! mkdir ~/.kaggle
# ! cp kaggle.json ~/.kaggle/

#  ## Change the permissions on the kaggle.json file
# !chmod 600 ~/.kaggle/kaggle.json

# ## remove original uploaded copy of kaggle.json
# !rm kaggle.json
```


```python
# ## Check list of available datasetss
# !kaggle datasets list
```
