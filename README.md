# PISTIS Data Flow and Data States

## Questions
- It is not clear which components are integrated in the Job Configurator?
- When and how is the Data Insights Generator triggered?
  - #Yury: the Data Insights Generator is supposed to deliver the insights about the data to partially automate and simplify the data registration process in the Factory's Data Catalogue.  It may use the Analytics Engine for this purpose (or not). The insights may include the information about the data type, its structure and other relevant metadata. It should be triggered at the moment of registering the data in catalogue and prefill some metadata in the data registration form, which have to be approved and filled by the user uploading(checking-in) the data to the Factory.    
- What is the connection of Analytics Engine to Data Enrichment?
  - #Yury: Data Enrichment can use the Analytics Engine to run the data analysis/processing scripts. The Analytics Engine is a generic data analysis tool with some predefined data analysis/processing models 
- Where and how is the GDPR Checker integrated? 
- Can we simply the parallel calls to Factory Data Storage and Data Catalogue?
- Should the Searchable Encryption access the Data Storage?
- Is there a need that components call the Lineage Service directly?
