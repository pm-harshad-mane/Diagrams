# Buyers / DSPs aquiring users from Advertiser sites to retarget

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant Browser
    participant Adv as Advertiser Site
    box DSP Infra
    participant DSP_pix as DSP Pixeling Server
    participant DSP_db as DSP Server Cookie-Store
    end

    User->>Browser: Opens Browser
    Browser->>Adv: Visits Advertiser Site
    Adv->>DSP_pix: A preconfigured DSP pixel with a campaign-Id executes from the Adveriser page  <br/> Makes a call to DSP Pixeling Server
    DSP_pix->>DSP_pix: Generate unique user-ID
    DSP_pix->>DSP_db: Store the information that the given user is retargatable for the given campaign-id
    DSP_pix->>Browser: Set third-party cookies to identify the user and to mark user taretable for the campaign

```

# User sync between SSP and DSP on Publisher site

# Displaying Retargeted Ads on Publisher web-sites




