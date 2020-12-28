# Extract, Transform, and Load Processes

### Contents
1. [The ETL Process](#The_ETL_Process)
2. [Extracting](#Extracting)
3. [Transforming](#Transforming)
    - [Data Cleaning and Optimization](#Data_Cleaning_and_Transforming)
4. [Loading](#Loading)
5. [Data Warehouse or Database](#Data_Warehouse_or_Database)
    - [ELT](#ELT)
  
## The ETL Process

ETL is the common abbriveation of the process which **extracts** raw information from various sources, then cleans or **transforms** this information into data which is able to be analyzed and **loads** it into a single location (or at least single cluster of locations) so it can be easily shared. Though this seems a simple definition with only a few steps, the reality must deal with the diversity and variety of data that needs to be transformed and loaded, the difficulty in transporting data when it comes in the massive quantities (Tb or Pb) it can, and how new technology can keep this process in flux as it must constantly adapt.

This section will provide some notes on how each step in this process works and how Big Data and new technology have changed it in recent years.


## Extracting

Data in the real-world rarely comes diectly in a nice CSV or JSON file, but from many heterogeneous data sources each from various places
which must be processed at the same time and changed into a usable form. This is what we mean by ***Extract***: we must pull 
this data from these sources. These sources can include:
  1. PDFs
    - Includng such things as PDFs of forms which include images of handwritten notes within them
  2. Human Readable Excel Spreadsheets (or Google Sheets or etc)
    - These are different then Well-Formatted Spreadsheets because they are not organized in a way that computers can easily read
  3. Proper (well formatted) or Improper XML (XML that doesn’t follow standard)
    - Or its cousin: HTML
  4. Images
  5. Sound Recordings
  6. JSON or CSVs or text files
  7. Full Databases
    - Its actually really common to have a number of individual (local) SQLite DBs that you merge into a single DB
  8. A host of other sources
  
So a relitively simple extraction task may be to pull all the data from networked registers in the form of transcation 
"journals" (DBs or tables), user comments about their experiences (text files), and online sales (json) files. Then stage 
them for the "Transformation" process where we will clean up and transform them to make it possible to load them into 
a single database.

## Transforming

ADD TRANSFORMING

#### Data Cleaning and Optimization

## Loading

ADD LOADING

## Data Warehouse or Database

ADD MODERN ELT PROCESS with DBW

#### ELT
