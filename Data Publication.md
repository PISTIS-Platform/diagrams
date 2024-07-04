```mermaid
sequenceDiagram
    autonumber
    participant factoryDataCatalogue as Factory Data Catalogue
    participant PISTISCatalogue as PISTIS Catalogue
    participant adb as Asset Description Bundler
    participant mpd as Monetisation Plan Designer
    participant datainvestmentplanner as Data Investment Planner
    participant NFT as NFT Generator
    participant accesspolicieseditor as Access Policies Editor
    participant fdv as FAIR Data Valuation
    participant ctc as Contract Template Composer
    participant smartcontractengine as Smart Contract Execution Engine
    participant contractchecker as Smart Contract Checker
    participant onoffinspector as On/Off Platform Inspector



    adb ->> factoryDataCatalogue: Select Dataset
    factoryDataCatalogue ->> adb: Return Assets Metadata
    adb ->> mpd: Select Monetsation Method	
  
opt Data Investment Plan
datainvestmentplanner --> mpd: Return Investment Plan
end

opt NFT Generation
    mpd ->> NFT: Initiate NFT Generation Process
    NFT ->> smartcontractengine: Check existing transactions of asset
    smartcontractengine ->> NFT: Return existing transactions
    NFT ->> mpd: Return NFT
end
    mpd ->> fdv: Request Data Valuation
    fdv ->> mpd: Suggest Data Valuation
    mpd ->> adb: Return Monetisation Plan
    accesspolicieseditor ->> adb: set access policies
    adb ->> ctc: Request Contract Text generation
    ctc ->> adb: Return Contract Text
    adb ->> smartcontractengine: Execute Contract for Publishing Data Asset
    smartcontractengine ->> onoffinspector: Check if Publishing is permitted according also to license
    onoffinspector ->> smartcontractengine: Return decision
    smartcontractengine ->> contractchecker: Check "Reshare" Counter (if exists)
    contractchecker->> smartcontractengine: Return if counter is exceeded or not
    smartcontractengine ->> adb: Return contract results
    adb ->> PISTISCatalogue: Publish Data Asset in Catalogue and link to contract
    adb ->> factoryDataCatalogue: Notify that Asset is published as listin X (could be multiple)



    

    


    
