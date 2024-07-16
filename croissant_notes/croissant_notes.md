# Croissant Notes

Note: These notes are from Day 16 (6/25/24) and Day 17 (6/26/24) of the [logbook](logbook.md).

## Croissant Overview

* Croissant is a new metadata format for standardizing machine learning datasets.
* The puropse of Croissant is to make datasets easily discoverable and useable across many different tools and platforms.
* Features
    * Adds metadata to describe datasets content in a standardized way.
    * Enables loading datasets into different ML platforms without reformatting.
    * Supported by major repositories like Kaggle, HuggingFace, and OpenML as well as Google Dataset Search
* Benefits
    * Simplifies dataset discovery and usage.
    * Reduces the need for manual data preprocessing.
    * Facilitates collaboration and reproducibility in ML research.
* Croissant Layers
    * Dataset Metadata Layer: General info (name, description, license)
    * Resources Layer: Describes files (FileObject) and sets of files (FileSet)
    * Structure Layer: Describes and organizes structure of resources (RecordSets)
    * Semantic Layer: adAdds ML-specific interpretations (e.g., data types, train/test splits)
* Future Directions
    * Community involvement to enhance ML specific aspects
    * Adoption beyond ML for broader data interoperability


