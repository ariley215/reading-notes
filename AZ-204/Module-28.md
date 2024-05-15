# Develop for Storage on CDNs

Objectives:

- Explain how the Azure Content Delivery Network works and how it can improve the user experience.
- Control caching behavior and purge content.
- Perform actions on Azure CDN by using the Azure CDN Library for .NET.

## Explore Azure Content Delivery Networks

Azure Content Delivery Network (CDN) offers developers a global solution for rapidly delivering high-bandwidth content to users by caching their content at strategically placed physical nodes across the world. Azure CDN can also accelerate dynamic content, which can't be cached, by using various network optimizations using CDN POPs (Points of Presence). For example, route optimization to bypass Border Gateway Protocol (BGP).

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

![Azure CDN Diagram](url-to-azure-cdn-diagram-image)
