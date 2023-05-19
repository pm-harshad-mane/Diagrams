# Inventory Packaging
We have to collect the ad-slot viewability and dwell time information and use it on the server side to put the inventory in different buckets.

## Current Approach: Client-Side Data Storage

```mermaid
sequenceDiagram
    autonumber
    box Publisher Page
    participant PubCode as Publisher Code on page
    participant LS as Browser Local Storage
    participant GPT as Google Publisher Tag
        participant PBJS_Core as Prebid JS Core
        participant PBJS_Module as Prebid JS Data Collection Module
        participant SSP_Bidder as PubMatic Bid Adapter
    end
    participant SSP_Server as SSP Server
    participant GAM as Google Ad Manager
    
    PubCode->>PBJS_Core: Initiate Auction For AdSlot1
    PBJS_Core->>PBJS_Module: Inform Auction Init
    PBJS_Module->>LS: Pull the data (Viewability and DwellTime) from Local Storage for AdSlot1
    PBJS_Module->>PBJS_Core: Pass the available data for AdSlot to Prebid Core
    PBJS_Core->>SSP_Bidder: Pass AdSlot details with data (Viewability and DwellTime) and request bids
    SSP_Bidder->>SSP_Server: SSP Requests bids and passes Viewability and DwellTime data
    SSP_Server->>SSP_Bidder: Return a bid
    SSP_Bidder->>PBJS_Core: Bidder submits the bid
    PBJS_Core->>PBJS_Core: Conduct auction of SSP bids and find Auction winner
    PBJS_Core->>GPT: Submit Key-Value pairs for the AdSlot
    GPT->>GAM: Send request to GAM
    GAM->>GAM: Conduct Line Item Selection
    GAM->>GPT: Return Prebid template creative to render
    GPT->>PBJS_Core: Render specific bid
    PBJS_Core->>PubCode: Render the winning bid creative
    GPT->>PBJS_Module: Inform as creative is rendered
    PBJS_Module->>LS: Store stat to note that creatve was rendered
    GPT->>PBJS_Module: Inform as creative is viewed by user
    PBJS_Module->>LS: Store stat to note that creatve was rendered
```

## New Approach 1: Stream Events from Page to Analytics

```mermaid
sequenceDiagram
    autonumber
    box Publisher Page
    participant PubCode as Publisher Code on page
    participant GPT as Google Publisher Tag
        participant PBJS_Core as Prebid JS Core
        participant PBJS_Module as Prebid JS Data Collection Module
        participant SSP_Bidder as PubMatic Bid Adapter
    end
    participant SSP_Server as SSP Server
    participant SSP_Event_Logger as SSP Event Logger Server
    participant SSP_DB as SSP DB with Aggregated Data
    participant GAM as Google Ad Manager
    
    PubCode->>PBJS_Core: Initiate Auction For AdSlot1
    PBJS_Core->>PBJS_Module: Inform Auction Init
    PBJS_Core->>SSP_Bidder: Pass AdSlot details and request bids
    SSP_Bidder->>SSP_Server: SSP Requests bids 
    SSP_Bidder->>SSP_DB: Pull Viewability and Dwelltime data for AdSlot
    SSP_DB->>SSP_Bidder: Return Viewability and Dwelltime data
    SSP_Server->>SSP_Bidder: Return a bid    
    SSP_Bidder->>PBJS_Core: Bidder submits the bid
    PBJS_Core->>PBJS_Core: Conduct auction of SSP bids and find Auction winner
    PBJS_Core->>GPT: Submit Key-Value pairs for the AdSlot
    GPT->>GAM: Send request to GAM
    GAM->>GAM: Conduct Line Item Selection
    GAM->>GPT: Return Prebid template creative to render
    GPT->>PBJS_Core: Render specific bid
    PBJS_Core->>PubCode: Render the winning bid creative
    GPT->>PBJS_Module: Inform as creative is rendered
    PBJS_Module->>SSP_Event_Logger: Store stat to note that creative was rendered
    SSP_Event_Logger->>SSP_DB: (Delayed) Store Aggregated Data
    GPT->>PBJS_Module: Inform as creative is viewed by user
    PBJS_Module->>SSP_Event_Logger: Store stat to note that creatve was rendered
    SSP_Event_Logger->>SSP_DB: (Delayed) Store Aggregated Data
```
