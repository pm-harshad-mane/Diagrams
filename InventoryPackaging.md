# Inventory Packaging
We have to collect the ad-slot viewability and dwell time information and use it on the server side to put the inventory in different buckets.

## Current Approach: Client-Side Data Storage

```mermaid
sequenceDiagram
    autonumber
    box Publisher Page
    participant PubCode as Publisher Code on page
    participant LS as Browser Local Storage
    participant PBJS_Core as Prebid JS Core
    participant PBJS_Module as Prebid JS Data Collection Module
    participant SSP_Bidder as PubMatic Bid Adapter
    end
    participant SSP_Server as SSP Server
    
    PubCode->>PBJS_Core: Initiate Auction For AdSlot1
    PBJS_Core->>PBJS_Module: Inform Auction Init
    PBJS_Module->>LS: Pull the data (Viewability and DwellTime) from Local Storage for AdSlot1
    PBJS_Module->>PBJS_Core: Pass the available data for AdSlot to Prebid Core
    PBJS_Core->>SSP_Bidder: Pass AdSlot details with data (Viewability and DwellTime) and request bids
    SSP_Bidder->>SSP_Server: SSP Requests bids and passes Viewability and DwellTime data
```
