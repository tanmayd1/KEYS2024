# Logbook

[//]: # ([Daily Logs] -- short &#40;5-6 sentence&#41; summary of your daily activities that includes: &#40;1&#41; what was the plan?, &#40;2&#41; what actually happend?, &#40;3&#41; what could you have done better?, &#40;4&#41; how you will approach this differently next time? )

---

## Training Week

### Day 1 (6/3/24)

Started off the day with learning about science literacy, research posters, and scientific wriitng. Then, I learned the basics of command line and scripting with Dr. Hilgert.

### Day 2 (6/4/24)

Learned presentation skills and then how to create a poster in PowerPoint. After that, I learned about remote computing and then connected to the University of Arizona's HPC.

### Day 3 (6/5/24)

Learned the importance of responsible science and research communication and learned about machine learning from Dr. Greg Chism.

I also met with Dr. Tyson Swetnam and Dr. Tony Edgin to discuss the details of the internship. They introduced me to the CyVerse Data Commons, CKAN, and iRODS which are the main tools that I will be using for the internship. They explained that the goal of the project is to allow a researcher to upload their data onto the CyVerse Data Commons and then use the CKAN API to access the data. We also discussed the importance of prompt engineering in order to be able to generate helpful code. 

### Day 4 (6/6/24)

Learned how to use excel and visualize data with the help of Mollie Wiegand. Learned about statistics in a lecture given by Dr. Greg Chism. Then, I learned the basis of using R and RStudio as well as image analysis using ImageJ.

### Day 5 (6/7/24)

Discussed a research paper on science literacy with other interns. Attended a lecture on bioinformatics given by Dr. Uwe Hilgert. Attended a lecture on professionalism given by Kelle Hyland and completed the training week debrief and evaluation.

---

## KEYS Internship

### Day 6 (6/10/24)

Today, I set up my OpenAI account so that I could practice prompt engineering. I watched some YouTube videos on prompt engineering and read through the [website](https://ua-data7.github.io/introllms/) provided by Dr. Swetnam. 
[Here](prompt_engineering_examples.md) are some examples that I created.

I also explored the CKAN deployment for CyVerse within an example organization. I learned how to create a dataset and how to add metadata to it. Additionally, I worked on my project description and the layout of my poster.

### Day 7 (6/11/24)

I manually transferred existing datasets from the curated data commons to the CKAN deployment to get a feel of how the datasets are structured. 

I also learned how to use the CKAN API to automate the process of transferring datasets. I created several different functions in Python that interact with the CKAN API to create/delete datasets, add resources, create organizations, and get dataset information. 
These functions were created with the help of [ChatGPT Prompt Engineering](ckan_api_testing_prompts.md) and using the [CKAN API documentation](https://docs.ckan.org/en/2.10/api/index.html).
These functions can be found [here](ckan_api_testing_scripts.md).
One obstacle that I encountered was that I didn't exactly know how to create some functions like the get_dataset_id function. To solve this, I used prompt engineering to ask ChatGPT to help me create the function. 
Another obstacle was that the get_dataset_id function that was provided to me by ChatGPT wasn't working at first and it was saying that the dataset was not found. 
After asking ChatGPT for help, I realized that the dataset name has to be in all lowercase and has to have hyphens instead of spaces.

In the next few days, I plan to get familiar with the API for the existing data commons so that in the future I can automate the process of transferring datasets from the existing data commons to the CKAN deployment.

### Day 8 (6/12/24)

In the morning, I attended the weekly science seminar and learned more about science literacy and attended lectures by Dr. Josh Nagao and Dr. Aisha Ahmed.

Afterwords, Tony Edgin introduced me to the [Discovery Environment API](https://de.cyverse.org/terrain/docs/index.html#/) that I could use to access the curated datasets in the datacommons. 
I was told to find the API endpoints that would allow me to access the datasets as well as the metadata for those datasets.
Because of the vast quantity of API endpoints, I asked ChatGPT to help me find the endpoints that I needed.
The prompts that I used can be found [here](discovery_environment_api_prompts.md).

Below are some of the endpoints that will be important for me to use:

* GET /terrain/secured/filesystem/directory: Get the list of directories in a specified path 
* GET /terrain/secured/filesystem/paged-directory: Get the list of files in a specified directory 
* GET /terrain/secured/filesystem/{data-id}/metadata: Get the metadata for a specific directory using its id


### Day 9 (6/13/24)

I started creating python functions that interact with the Discovery Environment API. 
I used the endpoints that I found yesterday to create these functions.
During the standup meeting, I was told to determine if any of the datasets have ARK permanent identifiers so using the functions that I created, I was able to find that there were 17 of the datasets that had ARK permanent identifiers.
The process of migrating these datasets to the CKAN deployment will be slightly different so it is important to know which datasets have ARK permanent identifiers.
Lastly, I was working on getting the metadata for the datasets and files so that I could store it in an organized manner.
This will make it easier when I am creating the datasets in the CKAN deployment.

The Python code that I created can be found [here](de_api_testing_scripts.md).

### Day 10 (6/14/24)

To start off the day, I revised the functions that I created in the past few days and mentally mapped out the process of transferring datasets from the existing data commons to the CKAN deployment.
When I was doing this, I realized that there was no way to edit the modified date and the created date of CKAN folders so upon asking my lab mentors, it was determined that I would not map the modified date and the created date of the datasets to the CKAN deployment.
Furthermore, I also realized that there wasn't a field for the modified and created dates for the resources in the CKAN deployment. 
However, I realized that I could add custom fields to the resources which is what I will be doing.

I then started creating the script that will automate the process of transferring datasets from the existing data commons to the CKAN deployment.
I was first having difficulty figuring out how to map the license metadata because the exact name of the license is different even though they refer to the same license.
To solve this, I had to create sample datasets in the CKAN deployment and then use the functions I created to see the metadata that was returned. 
The metadata showed that I had to create three new fields depending on the type of license so that it would properly show up in the CKAN deployment.

Another issue I had was that the subject metadata in the existing data commons was either a single string or a list of strings there were errors when I was trying to map them to the tags in the CKAN deployment.
Therefore, I had to first check if the subject metadata was a list and then iterate through the list to add each subject as a tag.

Another issue I was encountering was that some of the metadata keys were under different names but referred to the same thing such as the title, author name, etc.
Some of the datasets had "datacite." in front of the keys so I was working on adjusting the script to handle this. 
However, I am not done with this and will need to finish it on Monday.

In addition, I still need to start working on the part of the script that will map the resources of the datasets.


### Day 11 (6/17/24)

I finished the migration script that will automate the process of transferring datasets from the existing data commons to the CKAN deployment that I was working on last Friday.
Something that took a considerable amount of time was creating the citations for the datasets because that metadata wasn't available in the existing data commons.
I had to manually create a function that would create the citation for the datasets based on the metadata.
However, I realized that some of the fields for the citation like the creator, title, publication year, etc. were stored slightly differently in the existing data commons so I had to adjust the function to handle this.

I was also having some trouble with deciding how I would make the script idempotent meaning that the script won't add anything to CKAN that is already present, but it will update information that has changed.
Specifically, I wasn't sure how I would update any changes to the CKAN dataset that were made to the datasets in the existing data commons.
I then realized that I could use the date modified field in the existing data commons to check if the dataset has been modified and then update the dataset in the CKAN deployment by deleting the dataset and then creating it again with the new metadata.
This would make it easier to update the datasets in the CKAN deployment rather than having to manually check each metadata field and resource for changes.

The migration script that I created can be found [here](migration_script.md).
The entire code so far can also be found on GitHub [here](https://github.com/cyverse/data-commons).

### Day 12 (6/18/24)

I started running the script that I created to transfer the datasets from the existing data commons to the CKAN deployment.
While running the script, there were several problems that came up:

1. My function to get the details of all the datasets in CKAN was only returning the first 10 datasets. 
I couldn't exactly figure out the problem by myself so I asked ChatGPT for help. I told it what the problem was and it gave me a solution that I could use to fix the problem.
However, the solution that ChatGPT gave me didn't work so I had to ask ChatGPT for help again. This happened a few times until it finally gave me a solution that worked.
2. There were two datasets that had the same title despite it having different metadata so my script was throwing an error. 
I asked Tony Edgin and Michele Cosi for help, and they fixed the title in the existing data commons so that the script would run without any errors.
3. Some of the titles were longer than 100 characters and the CKAN API wasn't letting me create a dataset whose name was longer than 100 characters.
I asked Tony Edgin for help and he told me to just truncate the name to 100 characters. The title of the dataset is different from the name of the dataset so it was okay to truncate the name.
4. There is a dataset with no metadata in the existing data commons. I asked my lab mentors for help but I am still waiting on a reply from Dr. Merchant.
5. There are several datasets with many files and the script was taking a long time to run for these datasets.
Therefore, I skipped these datasets for now and I will ask my lab mentors for help on how to handle these datasets.

There were other minor errors that I encountered, mostly dealing with characters that weren't allowed in the name of the dataset or the tags.
These were quite easy to fix and I just had to add a bit of code to replace those characters with something else.
