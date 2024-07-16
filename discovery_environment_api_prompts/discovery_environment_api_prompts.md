# Discovery Environment API Prompts

Note: These prompts are from Day 8 (6/12/24) of the [logbook](logbook.md).

---

**System Instructions:**

*Same as the instructions for Example 1 in [practice](prompt_engineering_examples.md).*

---

## Finding API Endpoints to Access Curated Datasets

**Prompt:**
> Now I need to access the API for the current discovery environment which has the data commons right now. 
> Again, the goal is to write a script to transfer the datasets from this current discovery environment to the new CKAN environment. 
> I am going to give you a list of endpoints for the current discovery environment and I need you to tell me which of the possible endpoints will let me access the datasets in the discovery environment.

> *(List of endpoints not shown here but can be found [Discovery Environment API Documentation](https://de.cyverse.org/terrain/docs/index.html#!))*


**Response:**

> To identify which endpoints you can use to access the datasets in the discovery environment, you should look for endpoints related to data, files, and file systems. Based on the provided list, the relevant endpoints are under the "data" and "filesystem" categories. Here are the potential endpoints that you can use:

> Data Endpoints

> * GET /terrain/secured/saved-searches: Get Saved Searches
> * GET /terrain/saved-searches: Get Saved Searches

> Filesystem Endpoints

> * GET /terrain/secured/filesystem/root: Root Listing
> * GET /terrain/secured/filesystem/directory: Directory List (Non-Recursive)
> * GET /terrain/secured/filesystem/display-download: Display and Download File
> * GET /terrain/secured/filesystem/paged-directory: List Folder Contents
> * GET /terrain/secured/filesystem/file/manifest: Get File Manifest
> * GET /terrain/secured/filesystem/search-documentation: Get additional documentation for the search endpoint
> * GET /terrain/secured/filesystem/entry/{entry-id}/comments: Get File or Folder Comments

> File I/O Endpoints

> * GET /terrain/secured/fileio/download: Retrieve File Contents


## Accessing Metadata for Specific Datasets

**Prompt:**
> I need to access the metadata for specific datasets in the discovery environment but I'm not sure which endpoint to use.
> Can you tell me which endpoint I should use to get the metadata for a specific dataset like the DOI, creator, description, etc?

**Response:**
> To get the metadata of a dataset, including information such as the DOI, creator, description, and other relevant metadata, you should look for an endpoint that provides detailed information about the dataset or a specific file's metadata. Based on the provided endpoints, the following could be used:

> Relevant Endpoints

> - GET /terrain/secured/filesystem/{data-id}/metadata: This endpoint retrieves the metadata of a specific data item (file or folder) by its ID. This should include various attributes such as the creator, description, and potentially the DOI if it is stored as part of the metadata.

