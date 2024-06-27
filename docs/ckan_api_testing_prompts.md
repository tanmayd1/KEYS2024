# CKAN API Testing Prompts

Note: These prompts are from Day 7 (6/11/24) of the [logbook](logbook.md).

**System Instructions:**

*Same as the instructions for Example 1 in [practice](prompt_engineering_examples.md).*

## Helping set up the CKAN API and Creating a Dataset

**Prompt:**

Create a python script that sets up the CKAN API and creates a dataset in CKAN in a specific organization. 
Make sure to include comments in the script to explain what each part of the code does.

**Response:**
> ```python
> # Import the necessary libraries
> import requests
> import json
> 
> # Set the URL for the CKAN API
> ckan_url = 'https://data.yourorganization.org'
> 
> # Set the API key for the CKAN API
> api_key = 'YOUR_API_KEY'
> 
> # Function to create a new dataset in a specific organization
> def create_dataset_in_org(name, title, description, owner_org):
>     # Set the URL for the CKAN API endpoint to create a new dataset
>     url = f'{ckan_url}/api/3/action/package_create'
> 
>     # Set the headers for the request
>     headers = {
>         'Authorization': api_key,
>         'Content-Type': 'application/json'
>     }
>     # Prepare the data to be sent in the request
>     data = {
>         'name': name,
>         'title': title,
>         'notes': description,
>         'owner_org': owner_org
>     }
>     # Make the request to create the dataset
>     response = requests.post(url, headers=headers, data=json.dumps(data))
>     return response.json()
> ```
## Creating a Function to Get the Dataset ID

**Prompt:**

> Create a Python function that takes a dataset name as input and returns the dataset ID from CKAN. 
> The function should handle cases where the dataset is not found and return `None` in such cases.

**Response:**
> ```python
> def get_dataset_id(dataset_name):
>     url = f'{ckan_url}/api/3/action/package_show'
>     headers = {'Authorization': api_key}
>     params = {'id': dataset_name}
> 
>     response = requests.get(url, headers=headers, params=params)
> 
>     if response.status_code == 200:
>         # The dataset exists and the response contains the dataset metadata
>         dataset_metadata = response.json()
>         if dataset_metadata['success']:
>             return dataset_metadata['result']['id']
>         else:
>             print(f"Error: {dataset_metadata['error']['message']}")
>             return None
>     elif response.status_code == 404:
>         print(f"Dataset '{dataset_name}' not found.")
>         return None
>     else:
>         print(f"An error occurred: {response.status_code} - {response.text}")
>         return None
> ```

## Debugging the get_dataset_id Function

**Prompt:**

> Can you tell me why this function call is saying that the dataset is not found even though I know that the dataset name is correct and the dataset exists in the organization?
> 
> *(Same function as above)*

**Response:**
> If the function call is saying that the dataset is not found even though the dataset name is correct and it exists, there might be several potential issues to investigate:
>
> 1. Dataset Name: Ensure that the dataset_name provided is exactly as it appears in CKAN, including case sensitivity and any special characters.
> 2. Authorization: Verify that the API key has the necessary permissions to access the dataset.
> 3. CKAN Instance URL: Ensure that the ckan_url is correct and points to the right CKAN instance.
> 4. API Endpoint: Ensure that the CKAN instance's API endpoint is working correctly.

## Adding Comments to the Functions

**Prompt:**

> Add comments to the below functions to explain what each function does, what parameters it takes, and what it returns. 
> Make sure to include a brief description of the purpose of the function and any important details that the user should be aware of when using it.
> 
> *Functions (Not Shown Here)*

**Response:**
> *(Not Shown Here)*

Can be found in the [CKAN API Testing Scripts](ckan_api_testing_scripts.md).