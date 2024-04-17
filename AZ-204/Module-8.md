# Explore the Azure Blob Storgae Lifecycle

Objectives:

- Describe how each of the access tiers is optimized.
- Create and implement a lifecycle policy.
- Rehydrate blob data stored in an archive tier.

## Access Tiers

Azure Blob storage offers several access tiers to store data in the most cost-effective manner:

- **Hot**: Optimized for frequently accessed data
- **Cool**: Optimized for infrequently accessed data (stored for at least 30 days)
- **Cold**: Optimized for rarely accessed data (stored for at least 90 days)
- **Archive**: Optimized for rarely accessed data (stored for at least 180 days)

## Considerations for Access Tiers

- The access tier can be set on a blob during or after upload
- Only the Hot and Cool tiers can be set at the account level, the Archive tier is set at the blob level
- Data in the Cool tier has slightly lower availability, but similar durability, latency, and throughput as Hot
- Data in the Archive tier is stored offline, with the lowest storage costs but highest access costs and latency
- Hot and Cool tiers support all redundancy options, Archive supports only LRS, GRS, and RA-GRS
- Storage limits are set at the account level, not per access tier

## Lifecycle Management

Azure Blob storage lifecycle management offers a rule-based policy to:

- Transition blobs to cooler storage tiers (hot to cool, hot to archive, cool to archive)
- Delete blobs at the end of their lifecycle
- Define rules to run once per day at the storage account level
- Apply rules to containers or subsets of blobs using prefixes as filters

For example, you could move data from Hot to Cool after 2 weeks, then to Archive after 1 month, to optimize for performance and cost.

Here is a page for the digital study guide on discovering Blob storage lifecycle policies:

# Discovering Azure Blob Storage Lifecycle Policies

Azure Blob storage offers lifecycle management policies that allow you to automatically transition data between storage tiers or delete data based on defined rules.

## Lifecycle Policy Structure

- A lifecycle management policy is a collection of rules in a JSON document.
- Each rule definition includes a filter set and an action set.
- The filter set limits the rule actions to a certain set of objects within a container or based on object names.
- The action set applies the tier or delete actions to the filtered set of objects.

## Rule Definitions

Each rule within the policy has the following parameters:

Parameter | Type | Required | Notes
--- | --- | --- | ---
name | String | Yes | Rule name, up to 256 alphanumeric characters, case-sensitive, must be unique within a policy
enabled | Boolean | No | Allows a rule to be temporarily disabled, default is `true`
type | Enum | Yes | Currently only "Lifecycle" is supported
definition | Object | Yes | Defines the lifecycle rule with a filter set and an action set

## Rule Filters

Filters limit rule actions to a subset of blobs within the storage account. Supported filters include:

Filter | Type | Required
--- | --- | ---
blobTypes | Array of enum values | Yes
prefixMatch | Array of strings | No
blobIndexMatch | Array of key-value conditions | No

## Rule Actions

Supported actions include tiering and deletion of blobs, as well as deletion of blob snapshots. At least one action must be defined for each rule.

Action | Base Blob | Snapshot | Version
--- | --- | --- | ---
tierToCool | Supported | Supported | Supported
enableAutoTierToHotFromCool | Supported | Not supported | Not supported
tierToArchive | Supported | Supported | Supported
delete | Supported for block and append blobs | Supported | Supported

The actions are applied based on age, using conditions like `daysAfterModificationGreaterThan` and `daysAfterCreationGreaterThan`.

## Example Policy

Here's an example of a lifecycle policy that:

- Tiers blobs to the Cool tier 30 days after last modification
- Tiers blobs to the Archive tier 90 days after last modification
- Deletes blobs 2,555 days (7 years) after last modification
- Deletes blob snapshots 90 days after snapshot creation

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

## Key Takeaways

- Blob storage lifecycle policies allow you to define rules for transitioning data between storage tiers or deleting data.
- Policies consist of one or more rules, with each rule defining a filter set and an action set.
- Filters limit the rule actions to a subset of blobs, based on blob type, prefix, or blob index tags.
- Supported actions include tiering to Cool or Archive, and deleting blobs or snapshots based on age.
- Azure Blob storage offers different access tiers (Hot, Cool, Cold, Archive) to align with data lifecycle needs
- Lifecycle management policies can be used to automatically transition data between tiers or delete data
- Carefully aligning storage tiers with data access patterns can optimize performance and costs

## Additional Resources

- [Azure Blob storage access tiers](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)
- [Manage the Azure Blob storage lifecycle](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)
- [Manage the Azure Blob storage lifecycle](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts)
- [Blob storage lifecycle management policy examples](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-examples)
