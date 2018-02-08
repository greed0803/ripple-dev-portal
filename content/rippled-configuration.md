# Configuring `rippled`

***TODO: This doc proposes some ideas for a topic that documents `rippled` configuration. That topic will be this same page, once it is cleaned up, improved and complete. At this early stage, it is in DRAFT form, and open to community and developer feedback. Comments about the document are added in these bold-italic TODO callouts. All of the document is open to change. _TODO's will not be part of the published document_.***

## Sample TOC

**Considerations for Configuring `rippled`**

- Determine your business goals
- Types of server -- crosslink to [Types of Servers](https://ripple.com/build/rippled-setup/#types-of-rippled-servers)
- Hardware requirements -- crosslink to [Hardware Requirements](https://ripple.com/build/rippled-setup/#system-requirements) (will be updated by ticket [DOC-1262](https://ripplelabs.atlassian.net/browse/DOC-1262))
- Complete installation -- crosslink to [Installing `rippled`](https://ripple.com/build/rippled-setup/#installing-rippled)

**Configuration Recommendations**

_Description here_

***TODO: We can recommend that users pick one of several starting places for configuration and call out those features here, such as using NuDB if history sharding. Additionally, we may want to offer off-the-shelf configs for each of the respective persona/server-types, which may be modified as required.***

- Stock server
- Stock server with history sharding
- Full history server
- Validator

**Configuration Reference Tables**

_Description here_

- Purpose, syntax, notation - crosslink to [rippled.cfg example config file](https://github.com/ripple/rippled/blob/develop/doc/rippled-example.cfg)
- Server
- Peer Protocol
- Ripple Protocol
- HTTPS Client
- Database
- Diagnostics
- Voting
- Example Settings

**Troubleshooting Common `rippled` Configuration Problems**

_Description here_

- Common Problem A and Solution
- Common Problem B and Solution
- Common Problem C and Solution

***TODO: It may be worth adding this troubleshooting section. Thoughts?***

## Example Model for Document
***TODO: One model for presenting the sections of the Configuration Reference Tables follows, where any peculiarities or special notes could be called out in the section description.***

```
Section Heading (such as Peer Protocol)
  Brief description, and any notes

TABLE: In-the-wild configuration example

TABLE: Configuration section reference

```


# Reference Tables and Examples
***TODO: This section contains a truncated example of how the above suggested format for Configuration Reference Tables could be applied to the documentation for Server configuration.***

## Server

_Description here_

### Server's Port List Example

_Description here_

| Example                                                                                                 | Section or Sub-section                                     |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------|
| ``` [server]``` <br> ```port_public``` <br>```port_private```  <br> ```port=80``` <br> ```protocol = http ```| Server's port list<br>Listening port name<br>Listening port name<br>Listening port default <br> Protocol |

<br>

**Tip:** To avoid a conflict with port names and future configuration sections, Ripple recommends prepending "port_" to the port name. This prefix is not required.

### Port Settings List Example

_Description here_

| Example                                                                                                 | Section or Sub-section                                     |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------|
| ``` [port_rpc_admin_local]``` <br> ```port = 5005``` <br>``` ip = 127.0.0.1```  <br> ```admin = 127.0.0.1``` <br> ```protocol = http ```| Port settings list<br> Port <br> IP address <br> IP address <br> Protocol |

### Server Reference Table

_Description here_

| Section Field      | Sub-section Field      | Required?    | Description                                                                                       | Format                                                               |
|:-------------------|:-----------------------|:-------------|:--------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------|
| Server's port list |                        | Yes          | Named list used to identify the list of port settings list, where name identifies server          | String in square brackets                                            |
|                    | Listening port name    | At least one | Named port                                                                                        | String, must begine with letter and contain only letters and numbers |
|                    | Listening port default | Optional     | Pair that defines default listening port, unless the port overrides the value in its port section | `port={port number}`                                                 |
| Port settings list |                        | Yes          | Named list of port whose settings are defined in its key/value pairs                              | String in square brackets                                            |
|                    | Port                   | Yes          | Sets the port number for this port                                                                | `port={port number}`                                                 |
|                    | IP address             | Yes          | Address to bind to the network inteface. Use 0.0.0.0 to bind to all available interfaces          | `ip={IP address}`                                                    |
|                    | Protocol               | Yes          | Pair that defines supported protocol                                                              | `protocol=http,https`                                                |

<br>

***TODO: There are a number of ways the table above could be formatted, and probably many that it could be improved. Any thoughts for improvement that jump out would be welcome as a comment.***

<!--
# What follows below is the Author's Research Notes from example-rippled.cfg file (JBH)

**Note:** Many of the the children/sub-sectional fields were skipped during the first round of research. See [rippled-example.cfg file on Github](https://github.com/ripple/rippled/blob/develop/doc/rippled-example.cfg).

## Configuration

### Purpose, syntax, notation

### Server
`rippled` offers various server protocols to clients making inbound connections. The listening ports `rippled` uses are "universal" ports which may be configured to handshake in one or more of the available supported protocols. These universal ports simplify administration: A single open port can be used for multiple protocols.

NOTE: At least one server port must be defined in order to accept incoming network connections.

server - defines the server

<name> - defines the settings for that server

rpc_startup - startup rpc commands (remote procedure calls)

websocket_ping_frequency - number of seconds to wait before websocket ping message

### Peer Protocol

These settings control security and access attributes of the Peer to Peer server section of the rippled process. Peer Protocol implements the Ripple Payment protocol. It is over peer connections that transactions and validations are passed from to machine to machine, to determine the contents of validated ledgers.

ips - list of ip addreses for servers that run the Ripple protocol

ips_fixed - list of 'always attempt' ips
***TODO: Is this akin to the trusted UNL. It looks like this is a list of IP addresses to, at least, use to query for any missing ledgers.***

peer_private - Boolean to set whether peers broadcast your address

peers_max - max peer connections

node_seed - clustering -
***TODO: Is this to set a key required to participate in an optional private cluster?***

cluster_nodes - public keys of cluster nodes

sntp_servers - time sync server ip address

overlay
***TODO: What is p2p overlay?***

transaction_queue - EXPERIMENTAL (not for production)
Modifies transaction queue settings for testing/dev environment

### Ripple Protocol

These settings affect the behavior of the server instance with respect to Ripple payment protocol level activities such as validating and closing ledgers or adjusting fees in response to server overloads.


node_size - 'tiny' (default), 'small', 'medium', 'large', and 'huge'
Based on expected load and available memory

ledger_history - past ledgers to acquire on startup and minimum to maintain during operation

fetch_depth - The number of past ledgers to serve to other peers that request historical ledger data (or "full" for no limit).
***TODO: Is this unrelated to history sharding? If so, this would populate ledger history for the ledger stores for those rippled servers?***

validation_seed
***TODO: Is this to set a key required to participate in validation? What is the validation key pair?***

validator_token - Instead of a key, a token can be used

validator_key_revocation - Tells peers that a key(s) have been revoked and not to trust that/those keys

validators_file - path to file for servers to always accept as validators, relative to rippled.cfg location unless absolute path given
	validators
	validator_list_sites - URIs
	validator_list_keys - public keys

path_search - aggressiveness for path searching, defaults to 7

path_search_fast - minimum search aggressiveness field, defaults to 2

path_search_max - max aggressive, defaults to 10. setting to zero disables it

path_search_old - legacy path finding, defaults to 7

fee_default - in drops, as a fall back, when no other fee info exists, such as when signing transactions offline

workers - number of threads for processing work from peers and clients. If blank determined automatically

### HTTPS Client

The rippled server instance uses HTTPS GET requests in a variety of circumstances, including but not limited to contacting trusted domains to fetch information such as mapping an email address to a Ripple Payment Network address.


ssl_verify - boolean to verify certificates upon HTTPS client connection, defaults to 1

ssl_verify_file - path to cert verification file for the above ssl_verify

ssl_verify_dir - path to dir for root cert verifications for outbound HTTPS client connections


### Database

`rippled` creates 4 SQLite database to hold bookkeeping information about transactions, local credentials, and various other things. It also creates the NodeDB, which holds all the objects that make up the current and historical ledgers.

The size of the NodeDB grows in proportion to the amount of new data and the amount of historical data (a configurable setting) so the performance of the underlying storage media where the NodeDB is placed can significantly affect the performance of the server.

Partial pathnames will be considered relative to the location of the rippled.cfg file.

node_db - settings for the Ledger Store (or Node Database as github example shows)

	type = NuDB

	NuDB is a high-performance database written by Ripple Labs and optimized for rippled and solid-state drives.

	NuDB maintains its high speed regardless of the amount of history stored. Online delete may be selected, but is not required. NuDB is available on all platforms that rippled runs on.

	-OR-

	type = RocksDB

	RocksDB is an open-source, general-purpose key/value store - see http://rocksdb.org/ for more details.

	RocksDB is an alternative backend for systems that don't use solid-state drives.  Because RocksDB's performance degrades as it stores more data, keeping full history is not advised, and using online delete is recommended.  RocksDB is not available on Windows.

	The RocksDB backend also provides these optional parameters:

		compression         0 for none, 1 for Snappy compression

		Required keys:

		path - Location to store the database (all types)

		Optional keys

		online_delete - minimum ledgers to maintain, must be ≥ ledger_history

		advisory_delete - boolean to require admin RPC 'can_delete' call to delete ledger records

import_db - used with '--import' command line option to migrate the db into current db listed (in [`node_db`] setting for performing a one-time import (optional)

database_path
   Path to the book-keeping databases.
   ***TODO: What are the four book-keeping SQLite dbs used for?***

shard_db
      Settings for the Shard Database (optional)

    type - recommend NuDB, can also be RocksDB

    	compression - boolean for Snappy compression
    	path - path to the db
    	max_size_gb - max disk size

### Diagnostics

These settings are designed to help server administrators diagnose problems, and obtain detailed information about the activities being performed by the rippled process.

debug_logfile - relative path for logfile, unless absolute path given

insight - configuration settings for the Beast, the insight stats collection module

	server - statsd is the only server to send UDP packets to, currently, which must be running with rippled is running

	address - udp address and port

	prefix - string to distinguish between different rippled instances

example:

[insight]
server=statsd
address=192.168.0.95
prefix=my_first_validator

### Voting

voting - k/v pair paraments for use during ledger voting

	reference_fee - number of drops for the reference transaction fee

***TODO: Note says to not change without understanding the consequence, which is very vague. What are the implications here for rippled users?***

***TODO: Note also mentions rippled will use an internal default if this is left blank. Is that default the fee_default setting?***

	account_reserve - minimum reserve in drops, currently 20 M or 20 xrp

	owner_reserve - minimum drops reserved in the account for each ledger item the account owns, such as trust lines, open orders and tickets, currently 5M, or 5 XRP

## Example Settings

Administrators can use these values as a starting point for configuring their instance of rippled, but each value should be checked to make sure it meets the business requirements for the organization.

Server

	These example configuration settings create these ports:

	"peer"

	    Peer protocol open to everyone. This is required to accept
	    incoming rippled connections. This does not affect automatic
	    or manual outgoing Peer protocol connections.

	"rpc"

	    Administrative RPC commands over HTTPS, when originating from
	    the same machine (via the loopback adapter at 127.0.0.1).

	"wss_admin"

	    Admin level API commands over Secure Websockets, when originating
	    from the same machine (via the loopback adapter at 127.0.0.1).

	This port is commented out but can be enabled by removing
	the '' from each corresponding line including the entry under [server]

	"wss_public"

	    Guest level API commands over Secure Websockets, open to everyone.

	For HTTPS and Secure Websockets ports, if no certificate and key file
	are specified then a self-signed certificate will be generated on startup.
	If you have a certificate and key file, uncomment the corresponding lines
	and ensure the paths to the files are correct.

	NOTE

	    To accept connections on well known ports such as 80 (HTTP) or
	    443 (HTTPS), most operating systems will require rippled to
	    run with administrator privileges, or else rippled will not start.

[server]
port_rpc_admin_local
port_peer
port_ws_admin_local

#port_ws_public
#ssl_key = /etc/ssl/private/server.key
#ssl_cert = /etc/ssl/certs/server.crt

[port_rpc_admin_local]
port = 5005
ip = 127.0.0.1
admin = 127.0.0.1
protocol = http

[port_peer]
port = 51235
ip = 0.0.0.0
protocol = peer

[port_ws_admin_local]
port = 6006
ip = 127.0.0.1
admin = 127.0.0.1
protocol = ws

#[port_ws_public]
#port = 5005
#ip = 127.0.0.1
#protocol = wss


[node_size]
medium

This is primary persistent datastore for rippled.  This includes transaction metadata, account states, and ledger headers.  Helpful information can be found here: https://ripple.com/wiki/NodeBackEnd delete old ledgers while maintaining at least 2000. Do not require an external administrative command to initiate deletion.

[node_db]
type=RocksDB
path=/var/lib/rippled/db/rocksdb
open_files=2000

BEGIN UNDOCUMENTED SETTINGS!
filter_bits=12
cache_mb=256
file_size_mb=8
file_size_mult=2
END UNDOCUMENTED SETTINGS!

***TODO: What are these undocumented settings? This is the first time they appear.***
***TODO: What other settings are undocumented in the example config?***
***TODO: Make a PR against the example ripple.cfg to fill in those gaps***

online_delete=2000
advisory_delete=0

[database_path]
/var/lib/rippled/db

[debug_logfile]
/var/log/rippled/debug.log

[sntp_servers]

One of the following would be used
time.windows.com
time.apple.com
time.nist.gov
pool.ntp.org
***TODO: Verify that only one should be given. Can multiple be given, as backups?***

[ips]
# Where to find some other servers speaking the Ripple protocol.
r.ripple.com 51235

[validators_file]
File containing trusted validator keys or validator list publishers.
validators.txt

[rpc_startup]
Turn down default logging to save disk space in the long run. Valid values here are trace, debug, info, warning, error, and fatal
{ "command": "log_level", "severity": "warning" }

[ssl_verify]
If ssl_verify is 1, certificates will be validated. For self-signed certificates for development or internal use, set to ssl_verify to 0.
1

-->
