## PubMatic User Syncup: Iframe Based flow

```mermaid
sequenceDiagram
    autonumber
    box Publisher Page
    participant PubCode as Publisher Code
    participant CMP as Consent Management Platform
    participant UserSync as user_sync.html
    end
    box PubMatic Server Side Components
    participant PugM as PugMaster
    participant Pug
    participant SPug
    end
    participant DSP as DSP UserSync end-point

    PubCode->>UserSync: Loaded in Iframe
    UserSync->>UserSync: Check if third party cookies are enabled
    alt A:Third party cookies are NOT enabled
        UserSync->>UserSync: Do Nothing
    else: B: Third Party cookies are enabled
        UserSync->>CMP: Request User Consent input
        CMP->>UserSync: Pass User Consent signals
    end
    
    
```
