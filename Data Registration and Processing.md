```mermaid
sequenceDiagram
    autonumber
    participant dataStorage as Data Storage
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
    participant lineageTracker as Lineaga Tracker

    dataStorage ->> dataCheckIn: Upload Data and Metadata
    dataCheckIn ->> factoryDataStorage: Store Data
    factoryDataStorage ->> dataCheckIn: Return ID
    dataCheckIn ->> factoryDataCatalogue: Store Metadata
    factoryDataCatalogue ->> dataCheckIn: Acknowledge

    opt Data Processing 
        dataCheckIn ->> jobConfigurator: Configure Pipeline
        jobConfigurator ->> dataCheckIn: Acknowledge
        dataCheckIn ->> jobConfigurator: Trigger Pipeline
        jobConfigurator ->> factoryDataStorage: Get Data
        factoryDataStorage ->> jobConfigurator: Return Data
        opt Transformation
            jobConfigurator ->> dataTransformation: Transform Data
            dataTransformation ->> jobConfigurator: Return Data
        end
        opt Insights
            jobConfigurator ->> dataInsightsGenerator: Create Insights
            dataInsightsGenerator ->> jobConfigurator: Return Insights
        end
        opt Enrichment
            jobConfigurator ->> dataEnrichment: Enrich Data
            dataEnrichment ->> jobConfigurator: Return Data
        end
        opt Searchable Encryption
            jobConfigurator ->> searchableEncryption: Enable Searchable Encryption
            searchableEncryption ->> factoryDataStorage: Store Encrypted Keyword
            factoryDataStorage ->> searchableEncryption: Acknowledge
            searchableEncryption ->> jobConfigurator: Return Data
        end
        jobConfigurator ->> dataQualityAssess: Assess Data
        dataQualityAssess ->> jobConfigurator: Return Assessment
        jobConfigurator ->> factoryDataStorage: Store Data
        factoryDataStorage ->> lineageTracker: Store Lineage Data
        lineageTracker ->> factoryDataStorage: Acknowledge
        jobConfigurator ->> factoryDataCatalogue: Store Metadata
        factoryDataCatalogue ->> jobConfigurator: Acknowledge
    end

    opt Data Analytics
        analyticsEngine ->> factoryDataStorage: Get Data
        analyticsEngine ->> factoryMLRepo: Get ML Models
        factoryMLRepo ->> analyticsEngine: Return ML Models
        analyticsEngine ->> analyticsEngine: Create Analytics
    end
    
    opt Data Anonymization 
        anonymizer ->> factoryDataCatalogue: Get Metadata
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
    end


    

    


    






