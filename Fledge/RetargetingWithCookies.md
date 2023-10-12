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
```mermaid
sequenceDiagram
    autonumber
    actor User
    participant Browser
    participant Pub as Publisher Site
    box SSP Infra
    participant SSP_pix as SSP Pxeling Server
    participant SSP_db as SSP Server Cookie-Store
    end
    box DSP Infra
    participant DSP_pix as DSP Pixeling Server
    participant DSP_db as DSP Server Cookie-Store
    end

    User->>Browser: Opens Browser
    Browser->>Pub: Visits Publisher Site
    Pub->>SSP_pix: A preconfigured SSP pixel executes from the Adveriser page  <br/> Makes a call to SSP Pixeling Server
    SSP_pix->>Browser: Set SSP-User-ID TPC and set a redirect call to DSP Pixeling server to share SSP-User-ID
    Browser->>DSP_pix: Call to DSP Pixeling Server to share SSP-User-ID
    DSP_pix->>DSP_pix: Generate DSP-User-ID
    DSP_pix->>DSP_db: Optionally maintain map of DSP-User-ID to SSP-User-ID 
    DSP_pix->>Browser: Set DSP-User-ID TPC and set a redirect call to SSP Pixeling server to share DSP-User-ID
    Browser->>SSP_pix: Call to DSP Pixeling Server to share DSP-User-ID
    SSP_pix->>SSP_db: Maintain a map of SSP-User-ID to DSP-User-ID
```


# Displaying Retargeted Ads on Publisher web-sites




