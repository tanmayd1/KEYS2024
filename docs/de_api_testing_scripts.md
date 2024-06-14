# Discovery Environment API Testing Scripts

Note: This code is from Day 9 (6/13/24) of the [logbook](logbook.md).

## Initial Setup

```python
import requests
import json
from datetime import datetime

# Base URL for the Discovery Environment API
base_url = 'https://de.cyverse.org/terrain'

# API Key (replace with your actual API key)
api_key = 'Bearer " + 'YOUR_API_KEY'

# Headers for the requests
headers = {
    'Authorization': api_key
}
```

## Python Functions

```python
def pretty_print(json_data):
    """
    Format and print JSON data in a readable way.

    Parameters:
    json_data (dict): JSON data to be pretty-printed.
    """
    print(json.dumps(json_data, indent=4, sort_keys=True))

def convert_to_date(milliseconds):
    """
    Convert milliseconds since epoch to a human-readable date and time.

    Parameters:
    milliseconds (int): Milliseconds since epoch.

    Returns:
    str: Human-readable date and time.
    """
    seconds = milliseconds / 1000
    date_obj = datetime.fromtimestamp(seconds)
    date_str = date_obj.strftime('%Y-%m-%d %H:%M:%S')
    return date_str

def get_metadata(data_id):
    """
    Get metadata for a specific data ID.

    Parameters:
    data_id (str): The ID of the data item.

    Returns:
    dict: The metadata for the specified data ID.
    """
    url = f'{base_url}/filesystem/{data_id}/metadata'
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        metadata = response.json()
        return metadata
    else:
        print(f"Error getting metadata: {response.status_code} - {response.text}")
        return None

def get_all_metadata_dataset(dataset):
    """
    Get all metadata for a dataset.

    Parameters:
    dataset (dict): The dataset dictionary.

    Returns:
    dict: A dictionary containing all metadata for the dataset.
    """
    metadata_dict = {}

    # Convert and store creation and modification dates
    date_created = convert_to_date(int(dataset['date-created']))
    metadata_dict['date_created'] = date_created

    date_modified = convert_to_date(int(dataset['date-modified']))
    metadata_dict['date_modified'] = date_modified

    metadata_dict['de_path'] = dataset['path']

    dataset_id = dataset['id']

    # Get detailed metadata from the API
    metadata_return = get_metadata(dataset_id)
    avus = metadata_return['avus']

    for avu in avus:
        key = avu['attr']
        value = avu['value']
        if key in metadata_dict:
            try:
                metadata_dict[key].append(value)
            except AttributeError:
                metadata_dict[key] = [metadata_dict[key], value]
        else:
            metadata_dict[key] = value

    return metadata_dict

def get_all_metadata_file(file):
    """
    Get metadata for a specific file.

    Parameters:
    file (dict): The file dictionary.

    Returns:
    dict: A dictionary containing all metadata for the file.
    """
    metadata_dict = {}

    # Convert and store creation and modification dates
    date_created = convert_to_date(int(file['date-created']))
    metadata_dict['date_created'] = date_created

    date_modified = convert_to_date(int(file['date-modified']))
    metadata_dict['date_modified'] = date_modified

    metadata_dict['de_path'] = file['path']

    file_name = file['label']
    metadata_dict['file_name'] = file_name

    # Get the file type from the label
    file_type = file_name.split('.')[-1]
    metadata_dict['file_type'] = file_type

    # Construct the WebDAV location URL
    web_dav_location = "https://data.cyverse.org/dav-anon/iplant/commons/cyverse_curated/" + file['path'].replace('/iplant/home/shared/commons_repo/curated/', '')
    metadata_dict['web_dav_location'] = web_dav_location

    return metadata_dict

def get_list_of_files(path, limit=10):
    """
    Get the list of files in a specified directory.

    Parameters:
    path (str): The path to the directory.
    limit (int): The maximum number of files to retrieve.

    Returns:
    dict: A dictionary containing the list of files.
    """
    url = f'{base_url}/secured/filesystem/paged-directory'
    params = {'limit': limit, 'path': path}
    response = requests.get(url, headers=headers, params=params)
    if response.status_code == 200:
        files = response.json()
        print("List of files:")
        pretty_print(files)
        return files
    else:
        print(f"Error getting files: {response.status_code} - {response.text}")
        return None

def get_datasets_metadata():
    """
    Get a list of all datasets with their metadata.

    Returns:
    list: A list of dictionaries, each representing a dataset with its metadata.
    """
    path = '/iplant/home/shared/commons_repo/curated/'
    url = f'{base_url}/secured/filesystem/directory'
    params = {'path': path}
    response = requests.get(url, headers=headers, params=params)
    if response.status_code == 200:
        directories = response.json()

        # This variable is now a list of dictionaries where each dictionary represents a single dataset
        datasets = directories['folders']

        datasets_metadata = []

        # Get metadata for each dataset
        for dataset in datasets:
            metadata = get_all_metadata_dataset(dataset)
            datasets_metadata.append(metadata)
            pretty_print(metadata)
            print("\n")

        return datasets_metadata
    else:
        print(f"Error getting directories: {response.status_code} - {response.text}")
        return None
```