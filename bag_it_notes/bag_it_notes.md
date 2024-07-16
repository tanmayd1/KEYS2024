# BagIt Notes

Note: These notes are from Day 17 (6/26/24) of the [logbook](logbook.md).

## BagIt Overview

* A hierarchical file packaging format designed for the reliable transfer and storage of digital content
* Provides a standardized way to package files and metadata into bags for transfer and storage
* Each file in a BagIt package is accompanied by a checksum. 
    * Ensures that files can be validated for integrity upon receipt or retrieval, detecting any corruption or alteration.
* BagIt packages are designed to be both human-readable and machine-readable, making them accessible and easy to use
* Bags can include metadata files that describe the contents and context of the data
    * useful for data management and preservation

## BagIt Structure

1. Bagit.txt
    * Contains information about the bag itself, such as the version of the BagIt specification used and character encoding]
2. Manifest files
    * Lists of files in the bag and their corresponding checksums
    * Used to verify the integrity of the files in the bag
3. Data Directory
    * Contains the actual files being packaged
4. Tag files
    * Additional metadata files that provide information about the bag and its contents
    * Can include information such as provenance, rights, and preservation metadata

Example Directory Structure:

```
my_bag/
├── bagit.txt
├── bag-info.txt
├── manifest-md5.txt
└── data/
    ├── file1.txt
    ├── file2.jpg
    └── subdirectory/
        └── file3.pdf
```

