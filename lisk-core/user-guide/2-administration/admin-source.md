Author: diego

----

Created: 2018-06-19

----

Updated: 2018-07-26

----

Metadescription: In this section you can find a list of the basic commands needed to manage your Lisk node on the different distributions.

----

Metakeywords: Lisk Core Administration, Commands

----

Title: Administration

----

Htmltitle: Lisk Core - Administration User Guide | Lisk Documentation

----

Opengraphtitle: Lisk Core Administration

----

Opengraphimage:

----

Opengraphdescription:

----

Content:

# Lisk Core Administration

Subcategories | Description
--- | ---
[Binary](/documentation/lisk-core/user-guide/administration/binary) | Administration using Binary
[Docker](/documentation/lisk-core/user-guide/administration/docker) | Administration using Docker
[Source](/documentation/lisk-core/user-guide/administration/source) | Administration for Installation from Source

In this section you can find a list of the basic commands needed to manage your Lisk node on the different distributions, e.g.:

- Start / Stop the Lisk Core process
- Get the status of your Lisk Node
- Create snapshots
- Reload / Rebuild Lisk Core

## Snapshots

A snapshot is a backup of the complete blockchain. It can be used to speed up the sync process, instead of having to validate all transactions starting from genesis block to current block height.
Lisk provides official snapshots of the blockchain, see [http://snapshots.lisk.io](http://snapshots.lisk.io).

<boxinfo markdown="1">
[Creating own snapshots](/documentation/lisk-core/user-guide/administration/binary#create-snapshot) is only supported for Lisk Core Binary distributions.
Rebuilding from snapshot is explained for each distribution in the Administration section.
</boxinfo>

----

Whatsnext: custom

----

Whatsnextheadline: What's next?

----

Whatsnextpagelinktext: Lisk Core API Specification

----

Whatsnextpagelink: documentation/lisk-core/user-guide/api