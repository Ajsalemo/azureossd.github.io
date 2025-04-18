---
title: "How to troubleshooting Bring Your Own Storage (BYOS) issues on App Service Linux"
author_name: "Anthony Salemo"
tags:
    - Python
    - Deployments
    - Troubleshooting
categories:
    - Azure App Service on Linux # Azure App Service on Linux, Azure App Service on Windows, Function App, Azure VM, Azure SDK
    - Function App
    - Python # Python, Java, PHP, Nodejs, Ruby, .NET Core
    - Troubleshooting 
header:
    teaser: /assets/images/azurelinux.png # There are multiple logos that can be used in "/assets/images" if you choose to add one.
# If your Blog is long, you may want to consider adding a Table of Contents by adding the following two settings.
toc: true
toc_sticky: true
date: 2023-04-20 12:00:00
---

This post will cover troubleshooting issues that may arise when using Bring Your Own Storage with App Service Linux for both "Blessed" images and Web Apps for Containers. This is for scenarios that render the application and Kudu container unavailable.

# Overview
You can mount volumes to your container on App Service Linux or Web Apps for Containers, which map to a specified Azure Storage Account - and either use Azure Blob (read-only) or Azure File Shares (read/write) - which is named "[Bring Your Own Storage](https://learn.microsoft.com/en-us/azure/app-service/configure-connect-to-azure-storage?tabs=portal&pivots=container-linux)".

We'll go over some of the more common scenarios that may arise when an issue occurs.

# What kind of issues will I see?
If a volume from BYOS is mounted to your container and fails for any of the reasons below - this may cause your application to not show any logging - this will also show the typical "Application Error :(" page. This wil show both for the application container and the Kudu container.

The reason for the lack of application specific logging is that the volume is created and mounted very early on in the container lifecycle, prior to any point of where application logic is able to be invoked. Since it fails before any of this, which normally is what would generate `stdout/stderr` - there will likely be no logging.

The container will be unable to start until either the problem with BYOS is corrected, or, you remove the BYOS mount. You can change or remove a mount through the Azure Portal under **Configuration** -> **Path Mappings** or using something such as the Azure CLI.

## Observability
Although no application logs are generated - you can still use the **Diagnose and Solve Problems** blade to make use of the **Container Issues** detector to determine if there is an issue. If so, you will see some data like the below:

![BYOS failures](/media/2023/04/azure-oss-blog-byos-1.png)

In a scenario where the volume _is_ able to be mounted, you can review this from the application or Kudu container by running `df -h`. This will show the Storage Account and File Share or Blob (mounted with `blobfuse`) and then which path this is mounted to under the "Mounted on" column of the output.

![BYOS output](/media/2023/04/azure-oss-blog-byos-2.png)

Additionally, ensure to review `docker.log` to see if potential output about the errors described below are found in them. This can provide helpful context on where to troubleshoot faster.

# Scenarios
## Incorrect or rotated Access Keys
**Incorrect keys**:

When adding a Storage Mount, if using the "Basic" option, this will automatically handle adding in the **Primary** Access Key for you.

If you're using the "Advanced" option, you will need to manually copy and paste this key in.

If the key in incorrectly copied or mistyped - this will fail authentication for adding the mount. Thus, causing the container to crash.

**Keys that are rotated/refreshed**:

If you rotate an Access Key on the Storage Account side - the application will _only_ pick this change up where the container is needed to be restarted, such as:
- Restarting this specific application manually
- Other restart reasons such as deployment or site configuration updates
- Instance movement, such as scaling or typical PaaS instance movements

By default, the client (the App Service) has no self-awareness when a key is rotated on the Storage side other than after a restart. 

Therefor when the volume is attempted to be remounted (after the restart), this issue can occur since the key used on the Storage side will now differ from the key being used on the application side. 

In these scenarios, double check that the "Advanced" configuration blade has the correct Access Key and update it as needed.

Errors that are associated with this scenario is typically surfaced as `Output: mount error(13): Permission denied`


## Deleted Storage Account or Blob Container/Azure File Share
In the same vein as the [Keys that are rotated/refreshed](#incorrect-or-rotated-access-keys) section, if either the Storage Account, blob, or file share within this is deleted - the container will fail to start _after_ a restart - since it will now try to mount a non-existent file share.

However, read/write operations to the blob or share may fail even if the application isn't restarted - but while the share or blob is deleted still. This can be tested by going to an SSH session on the application or Kudu container and running `df -h` _after_ deleting the blob or share (but not restarting yet), you will notice this will disappear from available volumes shortly after. 

If the Blob or File Share is deleted - you can try to recreate these with the same name. A recreated Storage Account will need to use a different Access Key than the original.

- Errors that may be associated with trying to mount a file share or blob _after_ it's deleted may result in `could not resolve address` (or general DNS related) errors. Or, potentially `Output: mount error(2): No such file or directory`

## Networking and DNS
If the Storage Account cannot be accessed, then mounting the volume will fail.

Consider the following if your application is in a VNET and this is a possibility in case the application is down:
- Was there any networking changes on the Storage Account Side? 
    - Networking changes, such as changing traffic flow or any reconfiguration that may affect DNS resolution may not show up as a change on the application side.
- Storage firewall is supported only through service endpoints and private endpoints (when VNET integration is used).
    - If the app and Azure Storage account are in same Azure region, and if you grant access from App Service IP addresses in the Azure Storage firewall configuration, then these IP restrictions are not honored.
- Is custom DNS being used? Unresolvable DNS for the Storage Account will result in failure to mount the volume
  - A Private Endpoint on the Storage Account with misconfigured private DNS zones and/or records can also cause this
- When VNET integration is used, ensure the following ports are open:
    - Azure Files: 80 and 445.
    - Azure Blobs: 80 and 443.

If applicable, a relatively easy test for this is to disconnect the application from the VNET and open the Storage Account to public traffic. If this succeeds, then this can be a point of focus.

You can check connectivity and resolution with commands like the following - the below is using a Storage Account with an Azure File Share as an example:

**DNS related**:
- `nslookup yourstorage.file.core.windows.net`
- `dig yourstorage.file.core.windows.net`

Common _DNS-related_ errors are `Output: mount error: could not resolve address for someaddress.file.core.windows.net`
- Just like described below in the _connectivity related_ related section, you can additionally do testing from a test app (in the same subnet as the one failing), the Kudu (Bash) option on the _failing_ test app (SSH will not work as the main application container will not be created due to volume mount failures), or, from a VM within the same subnet
- You can try to test other DNS servers in certain scenarios such as with `nslookup yourstorage.file.core.windows.net 8.8.8.8`
- If using a custom DNS server, ensure your server can resolve Azure DNS FQDNs. You may need to add Azure DNS (168.63.129.16) to resolve unresolved DNS queries on your custom DNS server.

**Connectivity related**
- `tcpping yourstorage.file.core.windows.net`
- `nc -vz yourstorage.file.core.windows.net 443` and `nc -vz yourstorage.file.core.windows.net 445`

Common _connectivity errors_ are typically `Output: mount error(13): Permission denied` - such as a NSG blocking outbound traffic, or the subnet is not properly connected with the Storage Account, or, a Storage Firewall is enabled and denying incoming traffic. [Limitations](https://learn.microsoft.com/en-us/azure/app-service/configure-connect-to-azure-storage?tabs=basic%2Cportal&pivots=container-linux#limitations) may also cause `Permission denied` to be returned
- Note, this also includes unsupported **Storage Encryption** on the Storage Account. The default is _Maxmimum Compatability_. If using others such as _Maxmimum security_ or a custom implementation - this will likely end up with `Permission denied`. To be safe, use _Maxmimum Compatability_.

An error that is typically associated with required traffic to access storage and mount the volume being blocked through a User Defined Route (UDR) is `Output: mount error(115): Operation now in progress` - this is also _connectivity error_ based. You can also use the same `tcpping` / `nc` commands from either a test app, the Kudu console (the _Bash_ option - if doing tests on the same app that is failing to mount a volume, the container will be failing to be created, thus the _SSH_ option will be unavailable), or from a VM attached to the same subnet as the App Service. You can do that test for potential network-related `Permission denied` errors, too.



**NOTE**: If you enable a Storage Firewall after succesfully mounting storage - and try to `tcpping` the Storage Account, you may be unable to reach it.
 - Also if you try to list the contents while still in the mapped directory, you may recieve the following message:

```
root@8576066a2932:/home/site/files# ls -ltra
ls: cannot open directory '.': Host is down
```

## Mount Paths
`/home` or `/` cannot be mounted to (as well as from the `az cli`).

Mounting to `/home` or `/`, if it was possible, would likely cause the container to not start, as this directory and all subdirectories is already mounted via App Service built-in storage.

Mounting to `/tmp` or directories under `/tmp` is not recommended as this has the potential to cause the container to not start.

## File Locking
Depending on what the application is doing, file locking for certain services running over a volume may cause the application to crash (or not start) - depending on the application logic.

Below are some examples:

**Scenario**:

Trying to run MongoDB over mounted storage:

- Example error: `connection: /data/db/: handle-open: open: Operation not permitted`
    - MongoDB has limitations surrounding trying to mount a volume that is mapped to a Windows File Share (which in this case, is what Azure Files is)
    - There may be other limitations with Windows File Shares, MongoDB, and non-root users.
    - There may be lower level API's that are not supported with MongoDB when mapped to a Windows File Share.

    **Resolution**:
- Use a managed MongoDB instance like Mongo Atlas or the [MongoDB API For CosmosDB](https://learn.microsoft.com/en-us/azure/cosmos-db/mongodb/introduction).

**Scenario**:

Trying to run PostgreSQL over mounted storage:

- Example error: `data directory "/var/lib/postgresql/data" has wrong ownership`:
    - PostgreSQL may have limitations with being used over a Windows File Share mounted to the container due to the user being different in regards to the volume (and data ownership) - reference article - [pgdata has wrong ownership - forums.docker](https://forums.docker.com/t/data-directory-var-lib-postgresql-data-pgdata-has-wrong-ownership/17963)

    **Resolution**:
- Use a managed PostgreSQL instance like [Azure Database for PostgreSQL](https://learn.microsoft.com/en-us/azure/postgresql/)
    
**Scenario**:

- Trying to use SQLite over mounted storage:
    - As called out in [Best Practices](https://learn.microsoft.com/en-us/azure/app-service/configure-connect-to-azure-storage?pivots=container-linux&tabs=portal#best-practices) - _It's not recommended to use storage mounts for local databases (such as SQLite) or for any other applications and components that rely on file handles and locks._
    - SQLite is not recommended for production scenarios. Using a managed database offering should be used instead.
    - Since SQLite is a file-based database implementation, there are various reports throughout the years of SQLite file locking issues, including over volume mounts.

## Best practices and limitations
For general best practices and limiaations not within the scope of this post, review the below:
- [Mount Azure Storage as a local share - Azure App Service - Limitations](https://learn.microsoft.com/en-us/azure/app-service/configure-connect-to-azure-storage?tabs=cli&pivots=container-linux#limitations)
- [Mount Azure Storage as a local share - Azure App Service - Best Practices](https://learn.microsoft.com/en-us/azure/app-service/configure-connect-to-azure-storage?tabs=cli&pivots=container-linux#best-practices)

