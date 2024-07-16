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

### Day 13 (6/20/24)

In the morning, I attended the weekly science seminar and learned more about science literacy and attended lectures by Dr. Chris Frost.
After the seminar, I attended the standup meeting with Tony Edgin and Dr. Swetnam. 
I told them about how it was taking a long time to run the script for datasets with many files and they showed me how to run the script in a Ubuntu VM that was hosted on CyVerse.
This would allow me to easily run the script for datasets with many files because the VM had more resources than my local machine.
This script is still running and I will check on it tomorrow.
I also looked into flake8 which is a tool that checks for style errors in Python code, as suggested by Tony.
Lastly, I looked into some documentation and resources for Gradio provided by Dr. Merchant.

### Day 14 (6/21/24)

I checked on the script that was running in the Ubuntu VM and it is still running. 
I finished the script that would take in command line arguments and run the migration script for a specific dataset.
The arguments that the script would take in are the dataset name, the organization name, and the username and password for CyVerse in order to generate the API key.

I also created a cron job on the CKAN host so that the script would run every day at 5:00 PM.

Lastly, I finished the Materials and Methods section which can be found [here](assignment3.md).

### Day 15 (6/24/24)

I checked on the script that was running in the Ubuntu VM again and there were errors that were thrown.
I wasn't exactly sure what the error meant but when I reran the script I realized that the API key that I was using had expired.
In order to solve this, I created a test account on CyVerse and created a function that would generate the API key using the username and password of the test account.
Every time that the script runs, it will generate a new API key so that it hopefully doesn't expire while the script is running.

Additionally, when I reran the script for the second time, I realized that something was wrong with the part of the script that was checking the last modified dates in the Discovery Environment API and CKAN because the CKAN dates were exactly 7 hours ahead of the Discovery Environment dates.
However, when the datasets were originally created, the dates were the same so I am not exactly sure why the dates are different now.
Until I figure out why the dates are different, I temporarily check if the last modified date in the Discovery Environment API is exactly 7 hours behind the last modified date in CKAN and if it is, I don't update the dataset in CKAN.

Furthermore, I realized that some of the datasets hadn't transferred all of their files to the CKAN deployment so I wrote a function that would check if all of the files in the dataset were transferred and if they weren't, it would transfer the files that weren't transferred.

I started working on figuring out how to disable the ability of regular users to create CKAN organizations but I couldn't figure out how to access the configuration file in the CKAN deployment to do this.
I will ask Tony for help in the standup meeting tomorrow.

While I was working on that, Tony got back to me about things that I should change in my migration script. 
I spent the rest of the day working on that but I have still not finished it.

### Day 16 (6/25/24)

At the beginning of the day, I continued working on the changes that Tony suggested for the migration script.
In the middle of the day, I had a meeting with Dr. Merchant and Tony to discuss the progress of the project as well as future directions.
We discussed the involvement of gradio in the project and how it could be used to create a user interface for the project which would ensure that the metadata that a user inputs is correct.
We also discussed how something called croissant could be integrated with gradio. 
Croissant is a metadata format for standardizing machine learning datasets.
Apart from this, we talked about Amazon buckets which would allow to store vast amounts of data in an efficient way in CKAN.
I was told to research on these topics and see how they could be integrated into the project.
Today, I did research on croissant and my notes can be found [here](croissant_notes.md).
I also created a sample gradio app that would work with the script to transfer a single dataset from the existing data commons to the CKAN deployment.

### Day 17 (6/26/24)

In the morning we had the weekly science seminar where an alumni panel was held.
We also learned about the KEYS slam which would be held the day before the research showcase.

After the seminar, I found a research paper on croissant and read through it to get a better understanding of what croissant is.
I added the notes from the research paper to my notes on croissant which can be found [here](croissant_notes.md).

After this, I created an example workflow for how the gradio app would work with the script and croissant to transfer a dataset from the existing data commons to the CKAN deployment.

Then, I researched BagIt which is a file packaging format that is used to store and transfer large amounts of data. 
The notes from my research can be found [here](bag_it_notes.md).

### Day 18 (6/27/24)
For the majority of today, I was working on integrating the gradio app with the script that I already had. 
I added validation to the app that would make sure the user's credentials, organization, and dataset name were correct.
I did some more research on the exact structure of croissant metadata by looking at [this google colab notebook](https://colab.research.google.com/github/mlcommons/croissant/blob/main/python/mlcroissant/recipes/introduction.ipynb#scrollTo=MHdVY4TBEqZ8) that gave an example of how to create a croissant metadata file.

I also met with Dr. Merchant, Tony, and Larry Head who is a professor at the University of Arizona working on the DRIVEArizona project so that I could use his datasets to test my project.

### Day 19 (6/28/24)

Today, I was working on integrating croissant with the gradio interface. 
I wanted the interface to be able to take in a DE path and then create a croissant metadata file for that path.
If there was metadata that wasn't available in the DE path, the user would be able to input that metadata in the interface.
However, I couldn't figure out how to add additional fields depending on what was missing.
I asked ChatGPT for help but it wasn't able to give me a solution.

### Day 20 (7/1/24)

Over the weekend, Dr. Merchant gave me a set of instructions to be working on which I will work on for the next few days.
Today, I worked on the first step which was to create a parquet file using a file path in the Discovery Environment in a Jupyter notebook.
While working on this, I encountered several errors and problems. 
Firstly, I was having a hard time figuring out the API endpoint to get a file from the Discovery Environment.
I asked ChatGPT for help and it was still not able to give me a solution.
Eventually, I was able to figure out that I was using the wrong API endpoint and I was able to get the file from the Discovery Environment.
Next, I was having trouble converting the csv file to a parquet file.
The function that I was using was not storing the parquet file anywhere even though it was saying that it was.
I asked ChatGPT for help and it provided a different function that stored the parquet file differently which worked.

### Day 21 (7/2/24) 

During the standup meeting today, I was told to try and create a VICE app that would have the croissant editor in it.
I first ran the docker container from the croissant github repository locally which did work. 
When I tried to create the VICE app, however, the app was not able to start.
After asking Dr. Swetnam for help, he told me to hold off on the VICE app for now because we would need to create our own GitHub repository for it.

Other than that, I worked on the Jupyter notebook from yesterday to add a functon which would combine all the csv files in a directory into a single parquet file by year.

### Day 22 (7/3/24)

Today, I was working on creating a python script to be able to generate a DCAT JSON LD file from a some sample metadata.
After I finished this, I was working on another python script which would be able take in a DCAT JSON LD file and create a Croissant metadata file from it.
However, I kept encountering errors when trying to do this. 
One of the main errors is that the properties "md5" or "sha256" must be defined in the json file. 
I asked ChatGPT for help but it wasn't able to give me a solution.
I will continue working on this tomorrow.

### Day 23 (7/5/24)

I figured out the error that I was encountering yesterday with the "md5" and "sha256" properties.
I was formatting the JSON file incorrectly so I fixed that and the error went away.
I created another script to generate a Croissant metadata file from metadata.
I started creating the gradio app from scratch because of all the errors I was getting previously. 
I created 5 tabs in the app: 

1. one to transfer a dataset from the DE to CKAN
2. one to create a Croissant metadata file from a DE link
3. one to create a DCAT JSON LD file from a DE link
4. one to create a dataset in CKAN from a Croissant metadata file
5. one to create a dataset in CKAN from a DCAT JSON LD file.

I plan to add more tabs such as creating a parquet file from a csv file and creating a parquet file from a directory of csv files.
The issue that I was running into was that I wanted the app to be dynamic so that when the user enters a DE link and presses submit (for the first three tabs), there should be more fields that appear depending on what metadata is missing.

### Day 24 (7/8/24)

I asked Tony Edgin for help on the gradio app and he told me that I can hide and show fields depending on what the user inputs.
I was able to implement this for my tabs. 
I also implemented the functionality where the app gives an error if the user inputs a DE link that doesn't exist, if the username/password is incorrect, or if the input fields were left blank.
Other than that, I simultaneously worked on the first three tabs of the app because they require very similar functions.

### Day 25 (7/9/24)

CyVerse was down for most of the day so I wasn't able to work that much on the first three tabs because I wasn't able to test them.
I mainly worked on the last two tabs of the app where I created functions to extract the metadata from the Croissant metadata file and the DCAT JSON LD file.
For the 2nd and 3rd tabs, I was trying to figure out how the user could download the Croissant metadata file and the DCAT JSON LD file respectively.
After looking at the documentation and some github posts, I was able to figure out how to do this.
After CyVerse was back up, I worked on the first three tabs and I was able to complete the second one. 
The third tab should be similar to the second one so I will be able to complete that tomorrow.
I'm encountering an error for the first tab though and there is a problem with migrating the datasets to CKAN.

### Day 26 (7/10/24)

Today was the weekly science seminar where the guests gave lectures about undergraduate research as well as the University of Arizona's Honors College.
We also discussed the KEYS slam which would be held the day before the research showcase as well as the research showcase itself.
I had to submit my poster to my KEYS crew mentor today so I mainly worked on that for the rest of the day. 
For the error from yesterday regarding the first tab, I realized that the error was because there was no name key when uploading data to CKAN.
However, my python script should be creating a name key so I'm not exactly sure why it isn't.

### Day 27 (7/11/24)

Today, I mainly worked on my poster for the Research Showcase as well as the KEYS slam slides because I have to submit them tomorrow.
Dr. Merchant, Dr. Swetnam, and Tony Edgin gave a lot of feedback for my rough draft which I incorporated into my final draft.
I also met with Dr. Merchant and Tony Edgin to discuss what things to include in the poster and the slam slides.

The poster can be found [here](poster.md).

### Day 28 (7/12/24)

I finished basically all of my Gradio app today. I wasn't able to test it as much and I plan to do that on Monday and fix any errors that I encounter.
I know that there could be some issues with DCAT JSON LD files and Parquet files especially because I didn't really get to test that code that much.
I also did some finishing touches on my poster and slam slides and submitted them to Jordan Pilch.

### Day 29 (7/15/24)

CyVerse and CKAN were down for most of the day because of a power outage so I wasn't able to test my Gradio app that much.
I mainly worked on writing comments and restructuring my code so that it is easier to read and understand.
I encountered some errors while I was doing this but I fixed those.
I also pushed my code to the [data commons GitHub repository](https://github.com/cyverse/data-commons).
If CKAN is back up tomorrow, I will test my Gradio app and fix any errors that I encounter.

I also added Devan Patel's (a fellow KEYS intern) chatbot to this website.