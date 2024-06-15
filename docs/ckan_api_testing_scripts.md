# CKAN API Testing Scripts

Note: This code is from Day 7 (6/11/24) of the [logbook](logbook.md).

## Initial Setup

```python
# Import the necessary libraries
import requests
import json

# Set the URL for the CKAN API
ckan_url = 'https://ckan.cyverse.rocks/'

# Set the API key for the CKAN API
api_key = 'API_KEY'
```

---

## Create a Dataset

```python
def create_dataset(name, title, description, owner_org, author, 
                   author_email=None, tags=None, is_private=False, extras=None):
    """
    Create a new dataset in CKAN.

    Parameters:
    name (str): The name of the dataset (must be unique).
    title (str): The title of the dataset.
    description (str): A brief description of the dataset.
    owner_org (str): The ID or name of the organization to which the dataset belongs.
    author (str): The name of the author of the dataset.
    author_email (str): The email address of the author of the dataset.
    tags (list): A list of tags to associate with the dataset.
    is_private (bool): Whether the dataset should be private or public.
    extras (dict): A dictionary of additional metadata to associate with the dataset.

    Returns:
    dict: The response from the CKAN API, typically containing the dataset metadata.
    """
    
    # Set the URL for the CKAN API
    url = f'{ckan_url}/api/3/action/package_create'
    
    # Set the headers for the request
    headers = {
        'Authorization': api_key,
        'Content-Type': 'application/json'
    }
    
    # Prepare the data to be sent in the request
    data = {
        'name': name,
        'title': title,
        'notes': description,
        'owner_org': owner_org,
        'private': is_private,
        'author': author,
        'author_email': author_email,
        'tags': tags,
        'extras': extras
    }

    # Make the request to create the dataset
    response = requests.post(url, headers=headers, data=json.dumps(data))
    
    return response.json()


# Example usage
extras = [{'key': 'DOI', 'value': '10.12345/abcde'}, 
          {'key': 'Publication Year', 'value': '2024'}, 
          {'key': 'Publisher', 'value': 'CyVerse Data Commons'}, 
          {'key': 'resourceType', 'value': 'Example Dataset'}]
dataset_response = create_dataset(name='my-dataset3',
                                  title='My Dataset',
                                  description='This is a test dataset',
                                  owner_org='tanmay-s-playground',
                                  is_private=False,
                                  author='John Doe',
                                  author_email='john.doe@example.com',
                                  tags=[{'name': 'example'}, {'name': 'dataset'}],
                                  extras=extras)
print(f'Dataset creation response: {dataset_response}')
```

## Delete a Dataset

```python
# Function to delete an existing dataset
def delete_dataset(dataset_id):
    """
    Delete an existing dataset in CKAN.

    Parameters:
    dataset_id (str): The ID of the dataset to delete.

    Returns:
    dict: The response from the CKAN API, indicating success or failure.
    """
    url = f'{ckan_url}/api/3/action/package_delete'
    headers = {
        'Authorization': api_key,
        'Content-Type': 'application/json'
    }
    data = {
        'id': dataset_id
    }
    response = requests.post(url, headers=headers, data=json.dumps(data))
    return response.json()


# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
delete_response = delete_dataset(dataset_id)
print(f'Delete response: {delete_response}')
```


## Get the ID of a Dataset

```python
def get_dataset_id(dataset_name):
    """
    Get the dataset ID for a given dataset name in CKAN.

    Parameters:
    dataset_name (str): The name of the dataset.

    Returns:
    str: The dataset ID if found, or None if the dataset does not exist.
    """

    # Convert the dataset name to lowercase and replace spaces with hyphens
    dataset_name = dataset_name.lower().replace(' ', '-')

    # Set the URL and headers for the request
    url = f'{ckan_url}/api/3/action/package_show'
    headers = {'Authorization': api_key}
    params = {'id': dataset_name}

    # Make the request to get the dataset metadata
    response = requests.get(url, headers=headers, params=params)

    # Check the response status code
    if response.status_code == 200:
        # The dataset exists and the response contains the dataset metadata
        dataset_metadata = response.json()
        if dataset_metadata['success']:
            return dataset_metadata['result']['id']
        else:
            print(f"Error: {dataset_metadata['error']['message']}")
            return None
        
    # Check if the dataset was not found
    elif response.status_code == 404:
        print(f"Dataset '{dataset_name}' not found.")
        return None
    else:
        print(f"An error occurred: {response.status_code} - {response.text}")
        return None

# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
```

## Upload a Resource to a Dataset

```python
def upload_resource(dataset_id, file_path, resource_name, description):
    """
    Upload a resource (file) to an existing dataset in CKAN.

    Parameters:
    dataset_id (str): The ID of the dataset to upload the resource to.
    file_path (str): The local path to the file to upload.
    resource_name (str): The name of the resource.
    description (str): A brief description of the resource.

    Returns:
    dict: The response from the CKAN API, typically containing the resource metadata.
    """
    
    url = f'{ckan_url}/api/3/action/resource_create'
    headers = {
        'Authorization': api_key
    }
    data = {
        'package_id': dataset_id,
        'name': resource_name,
        'description': description
    }
    files = {
        'upload': open(file_path, 'rb')
    }
    response = requests.post(url, headers=headers, data=data, files=files)
    return response.json()

# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
resource_response = upload_resource(dataset_id, file_path='example.csv', resource_name='Example Resource')
```

## Add a resource using a link

```python
def add_resource_link(dataset_id, url, name, format, date_created_de, date_modified_de, description=None):
    """
    Add a link to a resource in a CKAN dataset.

    Parameters:
    dataset_id (str): The ID of the dataset to add the resource to.
    url (str): The URL of the resource.
    name (str): The name of the resource.
    description (str, optional): A description of the resource.

    Returns:
    dict: The response from the CKAN API.
    """
    resource_url = f'{ckan_url}/api/3/action/resource_create'
    headers = {
        'Authorization': api_key
    }
    data = {
        'package_id': dataset_id,
        'name': name,
        'description': description,
        'url': url,
        'format': format,  # or any other format that your link represents
        'Date Created in Discovery Environment': date_created_de,
        'Date Last Modified in Discovery Environment': date_modified_de
    }
    response = requests.post(resource_url, headers=headers, json=data)
    return response.json()

# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
resource_response = upload_resource(dataset_id, url="www.example.com", name='example', format='HTML', 
                                    date_created_de='2024-06-11', date_modified_de='2024-06-11', description='Example resource')
```

## Get dataset information

```python
def get_dataset_info(dataset_id):
    """
    Get detailed information about a specific dataset in CKAN.

    Parameters:
    dataset_id (str): The ID of the dataset to retrieve information about.

    Returns:
    dict: The response from the CKAN API, containing the dataset metadata.
    """
    url = f'{ckan_url}/api/3/action/package_show'
    headers = {'Authorization': api_key}
    params = {'id': dataset_id}
    response = requests.get(url, headers=headers, params=params)
    return response.json()


# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
dataset_info = get_dataset_info(dataset_id)
print(f'Dataset information: {dataset_info}')
```
 
## List all Datasets in the CKAN instance

```python 
def list_datasets():
    """
    List all datasets in the CKAN instance.

    Returns:
    dict: The response from the CKAN API, containing a list of dataset IDs.
    """
    url = f'{ckan_url}/api/3/action/package_list'
    headers = {'Authorization': api_key}
    response = requests.get(url, headers=headers)
    return response.json()


# Example usage
datasets = list_datasets()
print(f'List of datasets: {datasets}')
```