![mike](M.I.K.E.png)

# Chinese Cars
### Overview
This python script dissects a dataset, translates the column headers, removes duplicate rows, splits the dataset into manageable sizes for cleaning, validates the data, removes inconsistencies in the dataset and reconstructs the data into two readable files (one with consistent data and one with inconsistent data).

# Requirements
* [Google Colab](https://colab.research.google.com/)
* Dataset
* Pandas
* Regular expression library
* os library
* Google Translate

# Import and Install Libraries
```
import pandas as pd
import os
import re
!pip install googletrans==4.0.0-rc1
from googletrans import Translator
```

* pandas: For data manipulation and analysis
* os: For manipulating paths
* re: Specifies a set of strings that matches it
* Translator: Translates headers from Chinese to English

# Functioning Process

### Translate Headers to English
```
translator = Translator()
new_headers = []
for header in nationwide.columns:
  try:
    translation = translator.translate(header, dest='en')
    new_headers.append(translation.text)
  except:
    new_headers.append(header)
```

* This function creates a new headers list, stores the translated headers using the google translate library and appends the new list to the dataset

### Normalize headers
```
def normalize_header(header):
  header = header.lower()
  header = header.replace(' ', '_')
  header = re.sub(r'[^a-zA-Z0-9_]', '', header)
  return header
```

* Transform headers to lowercase and replace spaces

### Drop duplicates
```
nationwide.drop_duplicates(subset=[
    'frame_number',
    'id_card',
    'mail',
    'motor_number'
], keep='first', inplace=True)
```

* Drop duplicate records and keep the first occurrence

### Drop columns
```
columns_to_drop = ['gender', 'industry', 'monthly_salary',
                   'marriage', 'educate', 'brand',
                   'car', 'model', 'configuration',
                   'color', 'unnamed_21']
```

* Drop unnecessary columns

### Split Dataset
```
def split_dataframe(df, num_chunks):
  """Splits a DataFrame into a specified number of chunks."""
  chunk_size = len(df) // num_chunks
```

* Split the dataset based on the dataset size into 5 parts
* num_chunks = 5

### Validate Email Addresses
```
def validate_email(email):
    """Validates an email address using a regular expression."""
    if pd.isnull(email):
        return True  # Treat missing values as valid for now
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return bool(re.fullmatch(pattern, email))
```

* Validate email address and keep rows with missing values as those rows could still have valuable information

### Combine Chunks
```
def combine_chunks(input_dir, output_file):
    """Combines all CSV files in a directory into a single CSV file."""
    all_chunks = []
    for filename in os.listdir(input_dir):
        if filename.endswith('.csv'):
            file_path = os.path.join(input_dir, filename)
            chunk = pd.read_csv(file_path)
            all_chunks.append(chunk)
```

* Combine clean chunks into one csv
* Combine invalid chunks into one csv

# Execution
To use this script, download the .py file and change the parameters to suit your data and file management workflow.
