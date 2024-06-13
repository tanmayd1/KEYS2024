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
These functions can be found [here](ckan_api_testing.md).
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




