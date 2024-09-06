```mermaid
sequenceDiagram
    autonumber
    participant dataSource as Data Source
    actor User;
    participant dataCheckIn as Data Check-In
    participant dataTransformation as Data Transformation
    participant dataEnrichment as Data Enrichment
    participant dataQualityAssess as Data Quality<br>Assessment
    participant searchableEncryption as Searchable Encryption
    participant jobConfigurator as Job Configurator
    participant analyticsEngine as Analytics Engine
    participant dataInsightsGenerator as Data Insights<br>Generator
    participant factoryMLRepo as Data Factory ML<br>Model Repository
    participant factoryDataStorage as Factory Data<br>Storage
    participant factoryDataCatalogue as Factory Data<br>Catalogue
    participant gdprChecker as GDPR Checker
    participant anonymizer as Anonymizer
    participant lineageTracker as Lineage Tracker
    participant iam as Identity Access<br>Manager
    participant policyEditor as Access Policy<br>Editor
    participant dqlsh as Distributed Query LSH
    participant contractInspectorOffPlatform as Off Platform Contract Inspector


    

    User ->> jobConfigurator: Configure Pipeline
    jobConfigurator ->> User: Acknowledge
    User ->> jobConfigurator: Trigger Pipeline
    jobConfigurator ->> dataCheckIn: Get Data File
    dataCheckIn ->> dataSource: Get File
    dataSource ->> dataCheckIn: File
    dataCheckIn ->> jobConfigurator: Return Data File

    opt Transformation
        jobConfigurator ->> dataTransformation: Transform Data in File
        dataTransformation ->> jobConfigurator: Return File
    end
    opt Insights
        jobConfigurator ->> dataInsightsGenerator: Create Insights
        dataInsightsGenerator ->> jobConfigurator: Return Insights
    end


    opt Searchable Encryption
        jobConfigurator ->> searchableEncryption: Enable Searchable Encryption
        searchableEncryption ->> jobConfigurator: Return Data (Keywords?)
    end

    jobConfigurator ->> factoryDataStorage: Save Data
    factoryDataStorage ->> dqlsh: Trigger LSH indexing
    dqlsh ->> dqlsh: Generate LSH index
    factoryDataStorage ->> lineageTracker: Save Lineage Data
    lineageTracker ->> factoryDataStorage: Acknowledge
    factoryDataStorage ->> jobConfigurator: Acknowledge
    jobConfigurator ->> factoryDataCatalogue: Save Metadata (basic metadata and insights)
    factoryDataCatalogue ->> jobConfigurator: Acknowledge


    opt Access Policies Definition for Ingestion
        User ->> policyEditor: Start custom policies creation
        policyEditor ->> iam: Create custom policies for ingestion phase
        iam ->> policyEditor: Acknowledge
        policyEditor ->> User: Acknowledge
    end
    
    opt Check similarity of new dataset with existing datasets
            jobConfigurator ->> contractInspectorOffPlatform: Start inspection
            contractInspectorOffPlatform ->> jobConfigurator: Acknowledge
            contractInspectorOffPlatform ->> pistisDataCatalogue: Ask for the fingerprints of the exisitng datasets
            pistisDataCatalogue ->> contractInspectorOffPlatform: Return the fingerprints
            contractInspectorOffPlatform ->> factoryDataCatalogue: Store similarity with existing datasets
        end

    opt Enrichment
        User ->> factoryDataCatalogue: Enrich Selected Data Asset
        factoryDataCatalogue ->> dataEnrichment: Enrich Selected Data File
        dataEnrichment ->> factoryDataCatalogue: Get the structure information
        factoryDataCatalogue ->> dataEnrichment: Return the structure information
        dataEnrichment ->> User: Show Data and the Structure Information
        User ->> dataEnrichment: Refine data Semantics/structure
        dataEnrichment ->> factoryDataStorage: Update Data
        factoryDataStorage ->> lineageTracker: Store Lineage Data
        lineageTracker ->> factoryDataStorage: Acknowledge
        factoryDataStorage ->> dataEnrichment: Acknowledge
        dataEnrichment ->> factoryDataCatalogue: Update Metadata
        factoryDataCatalogue ->> dataEnrichment: Acknowledge
        dataEnrichment ->> User: Acknowledge
    end

    opt Data Quality Assessment
        User ->> dataQualityAssess: Assess Data
        dataQualityAssess ->> User: Return Assessment    
    end

    opt Data Anonymization 
        User ->> factoryDataCatalogue: Anonymize Data Asset
        factoryDataCatalogue ->> anonymizer: Anonymize Selected Data File
        anonymizer->> factoryDataCatalogue: Get Metadata
        factoryDataCatalogue ->> anonymizer: Return Metadata
        anonymizer ->> factoryDataStorage: Get Data
        factoryDataStorage ->> anonymizer: Return Data
        anonymizer ->> anonymizer: Anonymize Data
        anonymizer ->> factoryDataCatalogue: Update Metadata
        factoryDataCatalogue ->> anonymizer: Acknowledge
        anonymizer ->> factoryDataStorage: Update Data
        factoryDataStorage ->> lineageTracker: Store Lineage Data
        lineageTracker ->> factoryDataStorage: Acknowledge
        factoryDataStorage ->> anonymizer: Acknowledge
        anonymizer ->> User: Acknowledge
    end


    opt Data Analytics
        User ->> factoryDataCatalogue: Analyse Data Asset
        factoryDataCatalogue ->> analyticsEngine: Analyse Selected Data File        
        analyticsEngine ->> factoryDataStorage: Get Data
        factoryDataStorage ->> analyticsEngine: Return Data
        analyticsEngine ->> factoryMLRepo: Get ML Models
        factoryMLRepo ->> analyticsEngine: Return ML Models
        analyticsEngine ->> User: Show list and Request to select an ML Model
        User ->> analyticsEngine: Selected ML Model
        analyticsEngine ->> analyticsEngine: Create Analytics
        analyticsEngine ->> User: Analytics Results
    end  
