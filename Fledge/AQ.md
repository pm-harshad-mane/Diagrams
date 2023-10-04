# Ad Quality in Protected Audience

```mermaid
sequenceDiagram
    autonumber
    box FLEDGE
    participant PACore as PA Core
    participant SSPW as SSP Worklet
    end
    box SSP Infra
    participant SSPKV as SSP KV Server
    participant SSPKVDB as SSP KV Server DB
    participant SSPRE as SSP Reporting
    participant SSPAQ as SSP AQ Service
    end

    PACore->>SSPKV: Sends renderingURL
    SSPKV->>SSPKVDB: Request data from DB against given renderingURL
    SSPKVDB->>SSPKV: Data is found in DB and is returned
    SSPKV->>PACore: Returns a custom JSON say KV-JSON <br/> with renderingURL as key and an object against it
    PACore->>SSPW: Share KV-JSON to scoreAd function
    SSPW->>SSPW: Utilize KV-JSON to score bids
    SSPW->>PACore: Register forDebuggingOnly Win and Loss functions, push renderingURL, adMetaData, KV-JSON into it
    PACore->>SSPRE: Make a fetch call with URLs registered in forDebuggingOnly functions
```
