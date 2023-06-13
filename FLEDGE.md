## FLEDGE / PAAPI

```mermaid
sequenceDiagram
    autonumber
    participant FLEDGE as Protected Audience API on Chrome
    participant Pub as Publisher
    participant GPT as Google Publisher Tag
    box PrebidJS
    participant PBJS as PrebidJS Core
    participant PBJS_FLEDGE as PrebidJS Fledge Module
    participant SSP_PBJS as PrebidJS SSP BidAdapter
    end
    box SSP
    participant SSP_Server    
    participant SSP_KV as SSP Key/Value Server
    participant SSP_CDN
    end
    box DSP
    participant DSP_Server
    participant DSP_CDN
    participant DSP_KV as DSP Key/Value Server    
    end
    box GAM
    participant GAM as GAM/OB/AdX (Top Seller)
    participant GAM_KV as GAM Key/Value Server
    end
    

    Pub->>GPT: Declare AdSlots on page <br/> (some PAAPI-enabled)
    Pub->>PBJS: Configure AdSlots for PBJS Auction <br/> (some PAAPI-enabled)
    Pub->>PBJS: Initiate PBJS Auction
    PBJS->>SSP_PBJS: Pass AdUnits and inform <br/> which ones are PAAPI-enabled
    SSP_PBJS->>SSP_Server: ORTB Request with ae:1 <br/> on some impression objects
    SSP_Server->>DSP_Server: ORTB Request
    DSP_Server->>SSP_Server: ORTB Response, DSP responds <br/> with Contextual and/or signals for PAAPI auction
    SSP_Server->>SSP_PBJS: ORTB Response with <br/> Contextual bid and/or ingredients for PAAPI AuctionConfig
    SSP_PBJS->>PBJS: Submit both Contextual <br/> and PAAPI AuctionConfig
    PBJS->>PBJS_FLEDGE: Request to pass the pAAPI AuctionConfig
    PBJS_FLEDGE->>GPT: Submit PAAPI AuctionConfig <br/> for PAAPI enabled AdSlots
    PBJS->>PBJS: Conduct Contextual Bids Auction
    PBJS->>GPT: Add Key-Value pairs for Contxtual bid on AdSlot
    GPT->>GAM: Make GAM Ad Request
    GAM->>GPT: Returns GAM Ad Response
    GPT->>FLEDGE: Invokes runAdAuction with ComponentAuctions
    alt A Ineterest Group Auction Winner
        FLEDGE->>GPT: FLEDGE Returns Opaque Pointer
        GPT->>Pub: Returns IG Ad in fenced frame
    else B Contextual Auction Winner    
        FLEDGE->>GPT: FLEDGE Returns Null
        alt B1 GAM Winner
            GPT->>Pub: Renders Contextual GAM bid
        else B2 Prebid Winner    
            GPT->>PBJS: Signal Prebid to render Contextual Prebid bid
            PBJS->>Pub: Renders Contextual Prebid bid
        end    
    end    
```
