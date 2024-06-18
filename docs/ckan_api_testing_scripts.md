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
def create_dataset(data):
    """
    Create a new dataset in CKAN.

    This function sends a POST request to the CKAN API to create a new dataset with the provided metadata.
    The dataset metadata should include information such as name, title, description, owner organization,
    and any additional metadata fields.

    Args:
        data (dict): The dataset metadata dictionary, including keys like 'name', 'title', 'description',
                     'owner_org', and any additional metadata.

    Returns:
        dict: The response from the CKAN API, typically containing the dataset metadata.
    """
    url = f'{ckan_url}/api/3/action/package_create'  # API endpoint for creating a dataset
    headers = {
        'Authorization': api_key,  # API key for authorization
        'Content-Type': 'application/json'  # Content type for the request
    }
    response = requests.post(url, headers=headers, data=json.dumps(data))  # Send POST request to create dataset
    return response.json()  # Return the JSON response from the API


# Example usage
dataset_metadata = {
    'name': 'my_dataset',
    'title': 'My Dataset',
    'notes': 'This is a test dataset created using the CKAN API.',
    'owner_org': 'my_organization',
    'tags': [{'name': 'example'}, {'name': 'dataset'}],
    'author': 'John Doe',
    'author_email': '
}
response = create_dataset(dataset_metadata)
```

## Delete a Dataset

```python
def delete_dataset(dataset_id):
    """
    Delete a dataset in CKAN.

    This function deletes a specified dataset by sending a POST request to the CKAN API.
    This is used in the migration script when a dataset needs to be updated or replaced.

    Args:
        dataset_id (str): The ID of the dataset to delete.

    Returns:
        dict: The response from the CKAN API.
    """
    url = f'{ckan_url}/api/3/action/package_delete'  # API endpoint for deleting a dataset
    headers = {
        'Authorization': api_key,  # API key for authorization
        'Content-Type': 'application/json'  # Content type for the request
    }
    data = {
        'id': dataset_id  # ID of the dataset to delete
    }
    response = requests.post(url, headers=headers, data=json.dumps(data))  # Send POST request to delete dataset
    return response.json()  # Return the JSON response from the API


# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
delete_response = delete_dataset(dataset_id)
print(f'Delete response: {delete_response}')
```

## Delete all Datasets in an Organization

```python
def delete_all_datasets_in_organization(organization):
    """
    Delete all datasets in a specific organization in CKAN.

    This function deletes all datasets belonging to a specified organization by first listing all datasets
    in the organization and then sending a delete request for each dataset.
    This can be used to clean up existing datasets before migrating new data.

    Args:
        organization (str): The name or ID of the organization.

    Returns:
        None
    """
    datasets = list_datasets(organization=organization)  # List all datasets in the organization
    for dataset in datasets:
        dataset_id = dataset['result']['id']  # Get the dataset ID
        delete_response = delete_dataset(dataset_id)  # Delete the dataset
        print(f'Deleted dataset with ID: {dataset_id}. Response: {delete_response}')  # Print confirmation message

# Example usage
organization_name = 'My Organization'
delete_all_datasets_in_organization(organization_name)
```


## Get the ID of a Dataset

```python
def get_dataset_id(dataset_name):
    """
    Get the dataset ID for a given dataset name in CKAN.

    This function retrieves the ID of a dataset by its name. It sends a GET request to the CKAN API
    to fetch the dataset metadata and extract the dataset ID.

    Args:
        dataset_name (str): The name of the dataset.

    Returns:
        str: The dataset ID if found, or None if the dataset does not exist.
    """
    dataset_name = dataset_name.lower().replace(' ', '-').replace('.', '-')  # Format dataset name to match CKAN conventions
    url = f'{ckan_url}/api/3/action/package_show'  # API endpoint for showing dataset details
    headers = {'Authorization': api_key}  # API key for authorization
    params = {'id': dataset_name}  # Parameters for the GET request
    response = requests.get(url, headers=headers, params=params)  # Send GET request to retrieve dataset details
    if response.status_code == 200:
        dataset_metadata = response.json()  # Parse the JSON response
        if dataset_metadata['success']:
            return dataset_metadata['result']['id']  # Return dataset ID if found
        else:
            print(f"Error: {dataset_metadata['error']['message']}")  # Print error message if not successful
            return None
    elif response.status_code == 404:
        print(f"Dataset '{dataset_name}' not found.")  # Print message if dataset not found
        return None
    else:
        print(f"An error occurred: {response.status_code} - {response.text}")  # Print error message for other errors
        return None

# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
```

## Upload a Resource to a Dataset

```python
def upload_resource(dataset_id, file_path, name, date_created, date_updated, description=None):
    """
    Upload a resource (file) to a CKAN dataset.

    This function uploads a file to a specified CKAN dataset by sending a POST request to the CKAN API.
    The function attaches the file and metadata such as the resource name and description.

    Args:
        dataset_id (str): The ID of the dataset to add the resource to.
        file_path (str): The local path to the file to upload.
        name (str): The name of the resource.
        description (str, optional): A brief description of the resource.
        date_created (str): The date the resource was created.
        date_updated (str): The date the resource was last updated.

    Returns:
        dict: The response from the CKAN API, typically containing the resource metadata.
    """
    url = f'{ckan_url}/api/3/action/resource_create'  # API endpoint for creating a resource
    headers = {
        'Authorization': api_key  # API key for authorization
    }
    data = {
        'package_id': dataset_id,  # ID of the dataset to add the resource to
        'name': name,  # Name of the resource
        'description': description,  # Description of the resource
        'date_created_de': date_created,  # Date the resource was created
        'date_updated_de': date_updated  # Date the resource was last updated
    }
    files = {
        'upload': open(file_path, 'rb')  # File to upload
    }
    response = requests.post(url, headers=headers, data=data, files=files)  # Send POST request to upload resource
    return response.json()  # Return the JSON response from the API

# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
resource_response = upload_resource(dataset_id, file_path='example.csv', resource_name='Example Resource')
```

## Add a resource using a link

```python
def add_resource_link(data):
    """
    Add a link to a resource in a CKAN dataset.

    This function adds a URL link to a specified CKAN dataset by sending a POST request to the CKAN API.
    This is the primary way that resources are added to CKAN datasets from the discovery environment.
    The resource metadata should include the dataset ID, URL, name, description, format, and relevant dates.

    Args:
        data (dict): The resource metadata dictionary, including keys like 'package_id', 'name', 'url',
                     'description', 'format', and relevant dates.

    Returns:
        dict: The response from the CKAN API, typically containing the resource metadata.
    """
    resource_url = f'{ckan_url}/api/3/action/resource_create'  # API endpoint for creating a resource
    headers = {
        'Authorization': api_key  # API key for authorization
    }
    response = requests.post(resource_url, headers=headers, json=data)  # Send POST request to add resource link
    return response.json()  # Return the JSON response from the API

# Example usage
resource_data = {
    'package_id': 'my_dataset_id',
    'name': 'Example Resource',
    'url': 'https://example.com/data.csv',
    'description': 'This is an example resource link.',
    'format': 'CSV',
    'Date Created in Discovery Environment': '2024-06-11',
    'Date Last Modified in Discovery Environment': '2024-06-11'
}
response = add_resource_link(resource_data)
```

## Get dataset information

```python
def get_dataset_info(dataset_id):
    """
    Get detailed information about a specific dataset in CKAN.

    This function retrieves detailed metadata for a specified dataset by sending a GET request to the CKAN API.
    It is used to fetch information such as the dataset title, description, tags, resources, and other metadata.

    Args:
        dataset_id (str): The ID of the dataset to retrieve information about.

    Returns:
        dict: The response from the CKAN API, containing the dataset metadata.
    """
    url = f'{ckan_url}/api/3/action/package_show'  # API endpoint for showing dataset details
    headers = {'Authorization': api_key}  # API key for authorization
    params = {'id': dataset_id}  # Parameters for the GET request
    response = requests.get(url, headers=headers, params=params)  # Send GET request to retrieve dataset details
    return response.json()  # Return the JSON response from the API


# Example usage
dataset_name = 'My Dataset'
dataset_id = get_dataset_id(dataset_name)
dataset_info = get_dataset_info(dataset_id)
print(f'Dataset information: {dataset_info}')
```
 

## List all Datasets in the CKAN instance

```python 
def list_datasets(organization=None, group=None):
    """
    List all datasets in the CKAN instance for a specific organization and/or group.

    This function retrieves a list of all datasets in the CKAN instance, filtered by organization
    or group if specified. It sends a GET request to the CKAN API and returns the dataset metadata.
    This is used in the migration process to check whether a dataset from the discovery environment already exists in CKAN.

    Args:
        organization (str, optional): The name or ID of the organization.
        group (str, optional): The name or ID of the group.

    Returns:
        dict: The response from the CKAN API, containing a list of dataset IDs.
    """
    url = f'{ckan_url}/api/3/action/package_list'  # API endpoint for listing datasets
    headers = {'Authorization': api_key}  # API key for authorization
    output = []

    if organization is not None:
        # If an organization is specified, get the list of datasets for that organization
        url = f'{ckan_url}/api/3/action/organization_show'
        response = requests.get(url, headers=headers, params={'id': organization, 'include_datasets': True})
        response = response.json()
        response = response['result']['packages']
        for dataset in response:
            output.append(get_dataset_info(dataset['id']))
        return output

    elif group is not None:
        # If a group is specified, get the list of datasets for that group
        url = f'{ckan_url}/api/3/action/group_show'
        response = requests.get(url, headers=headers, params={'id': group, 'include_datasets': True})
        response = response.json()
        response = response['result']['packages']
        for dataset in response:
            output.append(get_dataset_info(dataset['id']))
        return output

    else:
        # If neither an organization nor a group is specified, get the list of all datasets
        response = requests.get(url, headers=headers)
        return response.json()


# Example usage
datasets = list_datasets()
print(f'List of datasets: {datasets}')

# Example usage with organization
organization_name = 'My Organization'
organization_datasets = list_datasets(organization=organization_name)

# Example usage with group
group_name = 'My Group'
group_datasets = list_datasets(group=group_name)
```

## Print json data in a readable format

```python
def pretty_print(json_data):
    """
    Format and print JSON data in a readable way.

    This function formats JSON data with indentation and sorted keys to make it more readable
    when printed to the console.

    Args:
        json_data (dict): JSON data to be pretty-printed.
    """
    print(json.dumps(json_data, indent=4, sort_keys=True))

# Example usage
pretty_print(dataset_info)
```