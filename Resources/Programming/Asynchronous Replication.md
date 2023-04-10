#backend #database

---
### Asynchronous Replication
>https://blog.purestorage.com/purely-informational/synchronous-replication-vs-aynchronous-replication/

With synchronous replication, data is written to the primary storage and the replica simultaneously. In contrast, with [asynchronous replication](https://support.purestorage.com/FlashArray/PurityFA/Replication), data is first written to primary storage and then to the replica. 

#### What Is Synchronous Replication?
Synchronous replication is a process of writing data to two systems at once, rather than one at a time. It allows for simultaneous updates of multiple repositories and is often used with a storage area network (SAN), local area network (LAN), wireless network, or other type of segmented system. 

#### What Is Asynchronous Replication?
Products that use asynchronous replication copy data to the replica after the data is already written to the primary storage. This replication process typically occurs on a scheduled basis. For example, write operations may be transmitted to a replica in batches periodically (i.e., every minute). 

---
#### Asynchronous vs. Synchronous: Key Differences

##### Asynchronous vs. Synchronous: Difference #1 
The first key difference between asynchronous and synchronous is the one we’ve already mentioned: the timing of the replication (synchronous replicates the data as it’s written to primary storage, asynchronous replicates it afterward).

##### Asynchronous vs. Synchronous: Difference #2
The second key difference between synchronous and asynchronous replication is the type of data storage that supports them. Asynchronous replication is more widely supported by array-, network-, and host-based replication products, while synchronous replication typically uses [higher-end, block-based storage arrays.](https://www.purestorage.com/products/file-and-object/flashblade.html) 

##### Asynchronous vs. Synchronous: Difference #3
The third key difference between asynchronous and synchronous is in the way they’re typically used. Synchronous replication is mainly used for high-end transactional applications that require instant failover if the primary node fails. Asynchronous replication, on the other hand, is mainly used for data backups. 

---
#### Asynchronous Replication: Advantages & Disadvantages

##### Advantages of Asynchronous Replication
In comparison to synchronous replication, asynchronous replication offers advantages in the following areas: 
-   **Cost:** Asynchronous replication tends to cost way less than synchronous replication because it doesn’t require as much bandwidth or specialized hardware.
-   **Distance:** Asynchronous replication is designed to work over long distances.
-   **Resiliency:** Asynchronous replication can tolerate some degradation in connectivity since the replication process doesn’t have to occur in real time. 

##### Disadvantages of Asynchronous Replication
The primary disadvantage of asynchronous replication is the time lag between data being stored at the primary and remote sites. If there’s an accident or outage, then transactions and data that aren’t replicated at the time of the incident will be lost, and data in secondary storage may not always be current.

---
#### Synchronous Replication: Advantages & Disadvantages

##### Advantages of Synchronous Replication
The primary advantage of synchronous replication is that the data is replicated to a secondary remote location at the same time as the new data is being created or updated in the primary data center.  This means the replication is nearly instant and enables you to have data replicas that are only a few minutes older than the source material.

##### Disadvantages of Synchronous Replication
Synchronous replication is more expensive than other forms of data replication. It also introduces latency that slows down the primary application and only works for distances of up to 300 km.

---
#### When to Use Synchronous Replication in a Database
It’s best to use synchronous replication in a database when you have high-end transactional applications requiring instant failover if the primary node fails. Synchronous replication is not typically used in network attached storage (NAS) unless the NAS device can also function as block-based storage for those high-end transactional applications.

#### When to Use Asynchronous Replication in a Database
Databases typically use asynchronous replication when things like [cloud backups](https://blog.purestorage.com/products/cloud-block-storage-disaster-recovery/) are needed. Asynchronous replication is also commonly used in virtual machines (VMs): Some hypervisors include asynchronous replication to allow entire VMs to be replicated to a remote location so the VM can fail over to that location in the event of a disaster. Another common use case for asynchronous replication is storage snapshots for continuous data protection.

![](https://pixel.welcomesoftware.com/px.gif?key=YXJ0aWNsZT1mOTUxODk5NmUyODIxMWVjYmRjMDYyYmU2N2FhNDRhYQ==)