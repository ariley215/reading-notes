# Develop for Storage on CDNs

Objectives:

- Explain how the Azure Content Delivery Network works and how it can improve the user experience.
- Control caching behavior and purge content.
- Perform actions on Azure CDN by using the Azure CDN Library for .NET.

## Explore Azure Content Delivery Networks

- **Purpose:**
  - Speeds up delivery of high-bandwidth content.
  - Caches content at global nodes.

- **Dynamic Content:**
  - Enhances delivery through network optimizations.

### Benefits of Using Azure CDN

- **Performance and User Experience:**
  - Improves performance for end users, especially when applications require multiple round-trips to load content.
- **Large Scaling:**
  - Handles instantaneous high loads, such as during a product launch event.
- **Traffic Distribution:**
  - Distributes user requests and serves content directly from edge servers, reducing traffic to the origin server.

### How Azure CDN Works

1. **User Request:**
   - A user requests a file using a URL with a special domain name (e.g., `<endpoint name>.azureedge.net`).
   - The DNS routes the request to the best performing POP location, usually the closest geographically.

2. **POP Request:**
   - If no edge servers in the POP have the file in their cache, the POP requests the file from the origin server.
   - The origin server can be an Azure Web App, Azure Cloud Service, Azure Storage account, or any publicly accessible web server.

3. **File Delivery:**
   - The origin server returns the file to an edge server in the POP.
   - An edge server in the POP caches the file and returns it to the original requester.
   - The file remains cached on the edge server in the POP until the time-to-live (TTL) specified by its HTTP headers expires (default TTL is seven days if not specified).

4. **Subsequent Requests:**
   - Other users can request the same file using the same URL.
   - If the TTL hasn't expired, the POP edge server returns the file directly from the cache, providing a faster user experience.

### Requirements

- **CDN Profile:**
  - You need at least one CDN profile, which is a collection of CDN endpoints.
  - Each endpoint represents a specific configuration of content delivery behavior and access.
  - To organize CDN endpoints, use multiple profiles.
  - Pricing is applied at the CDN profile level; create multiple profiles if you need a mix of pricing tiers.

### Limitations

- **Subscription Limits:**
  - Number of CDN profiles per subscription.
  - Number of endpoints per CDN profile.
  - Number of custom domains per endpoint.
  - For detailed information, refer to the [CDN limits](https://docs.microsoft.com/azure/cdn/cdn-limits).

### Azure CDN Features

- **Dynamic Site Acceleration**
- **CDN Caching Rules**
- **HTTPS Custom Domain Support**
- **Azure Diagnostics Logs**
- **File Compression**
- **Geo-filtering**

For a complete list of features supported by each Azure CDN product, visit [Compare Azure CDN product features](https://docs.microsoft.com/azure/cdn/cdn-features).

![Azure CDN Diagram](https://learn.microsoft.com/en-us/training/wwl-azure/develop-for-storage-cdns/media/azure-content-delivery-network.png)

## Control Cache Behavior on Azure Content Delivery Networks

### Caching Freshness

"fresh" cached resource:

- The most current version and are sent directly to the client.
- its age is less than the period defined by cache settings

When a browser reloads a webpage, it verifies that each cached resource is fresh and loads it. If the resource is stale, an up-to-date copy is loaded from the server.

### Controlling Caching Behavior

- **Mechanisms:**
  - **Caching Rules:**
    - **Global Rules:** Apply to all content from a specified endpoint.
    - **Custom Rules:** Apply to specific paths and file extensions.
  - **Query String Caching:** Configure how Azure CDN responds to a query string.
- **Azure CDN Standard for Microsoft Tier:**
  - **Ignore Query Strings (Default):**
    - Passes request and query strings to the origin server on the first request.
    - Caches the asset and ignores query strings for subsequent requests until TTL expires.
  - **Bypass Caching for Query Strings:**
    - Passes each query request directly to the origin server without caching.
  - **Cache Every Unique URL:**
    - Caches the response for each unique URL with its own TTL.
    - Inefficient if each request generates a unique URL due to low cache-hit ratio.
- **Changing Settings:**
  - In the Endpoint pane, select **Caching rules**.
  - Choose the desired caching option.
  - Click **Save**.

### Caching and Time to Live (TTL)

- **Cache-Control Header:**
  - Determines TTL duration from the HTTP response of the origin server.
  - Default TTL values if not set:
    - General web delivery: 7 days
    - Large file delivery: 1 day
    - Media streaming: 1 year

### Content Updating

- **Normal Operation:**
  - Edge node serves an asset until TTL expires.
  - Reconnects to origin server to fetch a new copy when TTL expires.
- **Ensuring Latest Version:**
  - Include a version string in the asset URL for immediate CDN retrieval.
  - Purge cached content to refresh on the next request.
- **Purging Methods:**
  - Endpoint by endpoint or all endpoints simultaneously.
  - Specify a file or all assets using the Purge All checkbox.
  - Use wildcards (*) or root (/).
- **CLI Command to Purge:**

  ```bash
  az cdn endpoint purge \
      --content-paths '/css/*' '/js/app.js' \
      --name ContosoEndpoint \
      --profile-name DemoProfile \
      --resource-group ExampleGroup
  ```

### Preloading Assets

- Prepopulate the cache to improve user experience.
- CLI command to preload:

  ```bash
  az cdn endpoint load \
      --content-paths '/img/*' '/js/module.js' \
      --name ContosoEndpoint \
      --profile-name DemoProfile \
      --resource-group ExampleGroup
  ```

### Geo-Filtering

- **Functionality:**
  - Allow or block content in specific countries/regions based on country/region code.
- **In Azure CDN Standard for Microsoft Tier, geo-filtering applies to the entire site.**
