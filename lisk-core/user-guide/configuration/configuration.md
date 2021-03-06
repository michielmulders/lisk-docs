# Lisk Core Configuration

## Structure

- The **default** network is `devnet`. If you want to connect to another network specify the `network` when starting Lisk Core, as described in [Source Administration](../administration/source/admin-source.md#command-line-options)
- The Lisk configuration is managed under different folder structures.
- Root folder for all configuration is `./config/`.
- Default configuration file that is used as base is `config/default/config.json`
- You can find network specific configurations under `config/<network>/config.json`, where `<network>` can be any of these values:
   - `devnet`
   - `alphanet`
   - `betanet`
   - `testnet`
   - `mainnet`
- Configurations will be loaded in following order, each one will override the previous one:
   1. Default configuration file
   2. Network specific configuration file
   3. Custom configuration file (if specified by user)
   4. Command line configurations, specified as command-line flags or `ENV` variables.
- For development purposes use `devnet` as a network option, other networks are specific to public Lisk networks.

Info | Note 
--- | --- 
![info note](../../info-icon.png "Info Note") | If none is specified, the default config value is `devnet`.

Important | Note 
--- | --- 
![important note](../../important-icon.png "Important Note") | Don't override any value in above mentioned files if you need custom configuration.

Info | Note 
--- | --- 
![info note](../../info-icon.png "Info Note") | To use a custom configuration: Create your own `json` file and pass it as [command line option](../administration/source/admin-source.md#command-line-options)

For advanced configurations, please go directly to the sections listed below :

- [API Access Control](#api-access-control)
- [Forging](#forging)
  - [Enable/Disable Forging](#enable-disable-forging)
  - [Check Forging](#check-forging)
- [SSL](#ssl)

The `config.json` file and a description of each parameter.

```js
 {
    "wsPort": 8001, // The port Lisk will listen to for WebSocket connections, e.g. P2P broadcasts
    "httpPort": 8000, // The port Lisk will listen to for HTTP connections, e.g. API calls
    "address": "0.0.0.0", // Specify the IPv4 Lisk will listen on (0.0.0.0 will listen to any IP)
    "fileLogLevel": "info", // Logging level for Lisk: info, error, debug, none
    "logFileName": "logs/lisk.log", // The path and name of the logfile
    "consoleLogLevel": "none", // The console logging level for app.js: info, error, debug, none
    "trustProxy": false, // If true, client IP addresses are understood as the left-most entry in the X-Forwarded-* header
    "topAccounts": false, // Deprecated, will be removed in future releases
    "cacheEnabled": false, // If true, enables cache
    "wsWorkers": 1, // Number of Web Workers
    "db": {
        "host": "localhost", // The ip of the database
        "port": 5432, // The port of the database
        "database": "lisk_main", // The name of the instance to use
        "user": "", // The user to use with the database, defaults to the current user
        "password": "password", // The default password to use with the database
        "min": 10, // Specifies the minimum amount of database handles
        "max": 95, // Specifies the maximum amount of database handles
        "poolIdleTimeout": 30000, // This parameter sets how long to hold connection handles open
        "reapIntervalMillis": 1000, // Closes & removes clients which have been idle > 1 second
        "logEvents": [ "error"], // Database logging events: connect, disconnect, query, task, transact, error
        "logFileName": "logs/lisk_db.log" // Relative path of the log file
    },
    "redis": {
        "host": "127.0.0.1", // The ip of the Redis instance
        "port": 6380, // The port Redis will listen on
        "db": 0, // Set the database to use. Default is '0'
        "password": null // If null, Redis is not password protected
    },
    "api": {
        "enabled": true, // Controls the API's availability. If disabled no API access is possible
        "access": {
            "public": false, // Controls the whitelist. When true all incoming connections are allowed
            "whiteList": ["127.0.0.1"] // This parameter allows connections to the API by IP. Defaults to only allow local host
        },
        "options": {
            "limits": {
                "max": 0, // Maximum of API conncections
                "delayMs": 0, // Minimum delay between API calls in ms
                "delayAfter": 0, // Minimum delay after an API call in ms
                "windowMs": 60000 // Minimum delay between API calls from the same window
            },
            "cors": {
               "origin": "*", // Defines the domains, that the resource can be accessed by in a cross-site manner. Defaults to all domains. 
               "methods": ["GET", "POST", "PUT"] // Defines the allowed methods for CORS
            }
        }
    },
    "peers": {
        "enabled": true, // Controls the Peers API's availability. If disabled, no inbound network communications will function
        "list": [ // Specifies a list of seed peers to connect to 
            {
                "ip": "192.168.1.1", // The ip of the peer
                "wsPort": 8000 // The WebSocket Port of the peer
            }
        ],
        "access": {
                    "blackList": [] // Peers to exclude from communicating with
                },
        "options": {
            "timeout": 5000, // How long to wait for peers to respond with data. Defaults to 5 seconds
            "broadhashConsensusCalculationInterval": 5000 // Interval for recalculating the broadhash consensus. Defaults to 5 seconds
        }
    },
    "broadcasts": {
        "active": true, // If true, enables broadcasts
        "broadcastInterval": 5000, // Specifies how often the node will broadcast transaction bundles
        "broadcastLimit": 25, // How many nodes will be used in a single broadcast
        "parallelLimit": 20, // Specifies how many parallel threads will be used to broadcast transactions
        "releaseLimit": 25, // How many transactions can be included in a single bundle
        "relayLimit": 3 // Specifies how many times a transaction broadcast from the node will be relayed
    },
    "transactions": {
        "maxTxsPerQueue": 1000 // Sets the maximum size of each transaction queue. Default: 1000
    },
    "forging": {
        "force": false, // Forces forging to be on, only used on local development networks
        "delegates": [ // Lists delegates, who are authorised to forge on this node.
           {
            "encryptedPassphrase":  "salt=5426da113a5896f11255f69bb49c49eb&cipherText=947b537de9&iv=67d7344ce8a3b2fc879e316a&tag=dc5db5bfb41a3e968278e99651c68523&version=1",
            "publicKey": "9d3058175acab969f41ad9b86f7a2926c74258670fe56b37c429c01fca9f2f0f"
	       }
        ], 
        "access": {
            "whiteList": [ "127.0.0.1" ]// This parameter allows connections to the Forging API by IP. Defaults to allow only local connections
        }
    },
    "syncing": {
        "active": true // If true, enables syncing (fallback for broadcasts)
    },
    "loading": {
        "loadPerIteration": 5000 // How many blocks to load from a peer or the database during verification
    },
    "ssl": {
        "enabled": false, // Enables SSL for HTTP requests - Default is false
        "options": {
            "port": 443, // Port to host the Lisk Wallet on, default is 443 but is recommended to use a port above 1024 with iptables
            "address": "0.0.0.0", // Interface to listen on for the Lisk Wallet
            "key": "./ssl/lisk.key", // Required private key to decrypt and verify the SSL Certificate
            "cert": "./ssl/lisk.crt" // SSL certificate to use with the Lisk Wallet
        }
    },
    "nethash": "ed14889723f24ecc54871d058d98ce91ff2f973192075c0155ba2b7b70ad2511" // Network hash of the Genesis block, used to differentiate networks. This should never be manually edited
}
```

## API Access Control

Controlling access to a node plays a vital role in security. The following configurable flags are available in order to control the access to your node:

```js
     "api": {
        "enabled": true, // Controls the API's availability. If disabled no API access is possible
        "access": {
            "public": false, // Controls the whitelist. When true all incoming connections are allowed
            "whiteList": ["127.0.0.1"] // This parameter allows connections to the API by IP. Defaults to only allow local host
        },
```

The recommended setup is to configure a whitelist for only trusted IP addresses, such as your home connection. Use IPV4 addresses only as the whitelist does not support IPV6. 

To setup a public wallet, simply leave the`api.access.whitelist` array empty.

For best security, disable all access setting `api.enabled` to `false`.

Important | Note 
--- | --- 
![important note](../../important-icon.png "Important Note") | This last configuration may prevent monitoring scripts from functioning.

## Forging

In order to enable your node to forge, you need first to insert some encrypted data into the config file under forging.delegates array. To encrypt your passphrase, we offer and recommend one of the following alternatives:

- [Lisk Commander](/lisk-commander/user-guide/commands/commands.md) via `encrypt passphrase` command
- [Cryptography module from Lisk Elements](/lisk-elements/user-guide/cryptography/cryptography.md)

We explain further the first alternative. First, make sure you have installed Lisk Commander in a secure environment. Upon completion, please follow the commands below to generate the encryptedPassphrase:

```bash
$ lisk
lisk passphrase:encrypt --output-public-key
Please enter your secret passphrase: *****
Please re-enter your secret passphrase: *****
Please enter your password: ***
Please re-enter your password: ***
{
        "encryptedPassphrase": "iterations=1000000&cipherText=30a3c8&iv=b0d7322bf24e0dfe08462f4f&salt=aa7e26c9f4317b61b4f45b5c6909f941&tag=a2e0eadaf1f11a10b342965bc3bafc68&version=1",
        "publicKey": "a4465fd76c16fcc458448076372abf1912cc5b150663a64dffefe550f96feadd"
}
```

 1. In the first step, type in your passphrase and then type in the password you want to use for encryption. 
 2.  Afterwards you will get an `encryptedPassphrase` key value pair. 
 3. Create the JSON object and add it to your `config.json` under `forging.delegates`:

```js
Forging
     "forging": {
        "force": false,
        "delegates": [
                {
				"encryptedPassphrase":
 "salt=5426da113a5896f11255f69bb49c49eb&cipherText=947b537de9&iv=67d7344ce8a3b2fc879e316a&tag=dc5db5bfb41a3e968278e99651c68523&version=1",
				"publicKey":
					"9d3058175acab969f41ad9b86f7a2926c74258670fe56b37c429c01fca9f2f0f"
	       }              
         ],
        "access": {
            "whiteList": [
                "127.0.0.1", "REPLACE_ME" // Replace with the IP you will use to access your node
            ]
        }
    },
```

4. Reload your Lisk Core process to make the changes  in the config effective, e.g. for Binary install, run : `bash lisk.sh reload`

### Enable/Disable Forging

Info | Note 
--- | --- 
![info note](../../info-icon.png "Info Note") | The endpoint to perform this action is **idempotent** what it means, the result has to be the same, no matter how many times you execute the same command. 

If you are running your Lisk Node from a local machine, you can enable forging through the API client, without further interruption.

Important | Note 
--- | --- 
![important note](../../important-icon.png "Important Note") | Remember that after restarting you Lisk node, you must need to re-enable forging again.

Use the following curl command to **enable the forging** for your delegate:
```bash
curl -X PUT \
  http://127.0.0.1:7000/api/node/status/forging \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{
          "publicKey": "YYYYYYYYY",
          "password": "XXX",
          "forging": true
      }'
```
Use the following curl command to **disable the forging** for your delegate:
```bash
curl -X PUT \
  http://127.0.0.1:7000/api/node/status/forging \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{
          "publicKey": "YYYYYYYYY",
          "password": "XXX",
          "forging": false
      }'
```
- Where `publicKey` is the key for the delegate you want to enable/disbale
- `password` is the password used to encrypt your passphrase in `config.json`
-  `forging` is the boolean value to enable or disable the forging
- HTTP Port can be different based on your configuration, so check `httpPort` in your `config.json`

### Check Forging
Use the following curl command to verify the forging  status of your delegate:

```bash
curl \
  http://127.0.0.1:7000/api/node/status/forging \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' 
```

The result should be something like this:

```js
{
  "meta": {},
  "data": [
    {
      "forging": true,
      "publicKey": "9bc945f92141d5e11e97274c275d127dc7656dda5c8fcbf1df7d44827a732664"
    }
  ],
  "links": {}
}
```

## SSL

Info | Note 
--- | --- 
![info note](../../info-icon.png "Info Note") | To complete this step require a signed certificate (from a CA, such as Let's Encrypt) or a self-signed certificate. You will need both the private and public keys in a location that is accessible to Lisk.

Next snippet highlights the essential parameters to enable SSL security on your node's connections:

**SSL Configuration**

```js
 "ssl": {
  "enabled": false,         // Change FROM false TO true
  "options": {
    "port": 443,            // Default SSL Port
    "address": "0.0.0.0",   // Change only if you wish to block web access to the node
    "key": "path_to_key",   // Replace FROM path_to_key TO actual path to key file
    "cert": "path_to_cert"  // Replace FROM path_to_cert TO actual path to certificate file
  }
}
```

Important | Note 
--- | --- 
![important note](../../important-icon.png "Important Note") | If SSL Port configured above in `ssl.options.port` is a privileged port (below 1024), you must either allow node to use the specified port with `setcap` or change the configuration to use a port outside of that range.

**Setcap:** Only required to grant Lisk access to port 443

```bash
 sudo setcap cap_net_bind_service=+ep bin/node
```

To verify all you have properly configured your node, open the web client using `https://MY_IP_OR_HOST`. You should now see a secure SSL connection.
