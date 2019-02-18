---
layout: guide
title: StorageOS Docs - Labels
anchor: reference
module: reference/labels
---

# StorageOS Feature labels

Feature labels are a powerful and flexible way to control storage features,
especially when combined with [rules]({% link _docs/reference/cli/rule.md %}).

Labels can be applied to various StorageOS artefacts. Applying specific feature
labels triggers compression, replication and other storage features. No feature
labels are present by default.

## StorageOS Volume labels

Volumes do not have any feature labels present by default

| Feature             | Label                               | Values                               | Description                                                                                                                                                                                                                        |
| :------------------ | :---------------------------------- | :----------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------                                                                                     |
| Caching             | `storageos.com/nocache`             | true / false                         | Switches off caching.                                                                                                                                                                                                              |
| Compression         | `storageos.com/nocompress`          | true / false                         | Switches off compression of data at rest and in transit.                                                                                                                                                                           |
| Replication         | `storageos.com/replicas`            | integers [0, 5]                      | Replicates entire volume across nodes. Typically 1 replica is sufficient (2 copies of the data); more than 2 replicas is not recommended.                                                                                          |
| Failure mode        | `storageos.com/failure.mode`        | strings [`soft`,`hard`,`alwayson`]   | Soft failure mode works together with the failure tolerance. Hard is a mode where any loss in desired replicas count will mark volume as unavailable. AlwaysOn is a mode where as long as master is alive volume will be writable. |
| Failure tolerance   | `storageos.com/failure.tolerance`   | integers [0, 4]                      | Specifies how many failed replicas to tolerate, defaults to (Replicas - 1) if Replicas > 0, so if there are 2 replicas it will default to 1.                                                                                       |
| QoS                 | `storageos.com/throttle`            | true / false                         | Deprioritizes traffic by reducing the rate of disk I/O, when true.                                                                                                                                                                 |
| Placement           | `storageos.com/hint.master`         | Node hostname or uuid                | Requests master volume placement on the specified node.  Will use another node if request can't be satisfied.                                                                                                                      |

To create a volume with a feature labels:

```bash
storageos volume create --label storageos.com/throttle=true --label storageos.com/replicas=1 volumename
```

## StorageOS Pool labels

Pools do not have any labels present by default.

| Feature             | Label                               | Values                               | Description                                                                                                                                                                                                                        |
| :------------------ | :---------------------------------- | :----------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------                                                                                     |
| Overcommitment      | `storageos.com/overcommit`          | integers [+]                         | Sets the percentage of overcommitment allowed for the pool (see [here]({%link _docs/operations/overcommitment.md %})).                                                                                                                                                                        |

To add overcommit labels to a pool: 

```bash
storageos pool update --label-add storageos.com/overcommit=20 default
```

