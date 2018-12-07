# Hdac Node Mining Portal

**`Hdac Node Mining`** Portal uses open source.

See [Open Source NOMP](https://github.com/foxer666/node-open-mining-portal)


## 1. OS Setup

ubuntu 16.04. * Based on the latest version.

```
>sudo apt-get update

>sudo apt-get dist-upgrade

>sudo apt-get install git

>sudo apt-get install build-essential autoconf libtool libboost-all-dev libcurl4-openssl-dev libdb5.1-dev libdb5.1++-dev mysql-server
```

* ubuntu 14.04.* Version

In addition, library should be installed in order.

```
>sudo add-apt-repository ppa:ubuntu-toolchain-r/test

>sudo apt-get update

>sudo apt-get install libstdc++6
```

## 2. NVM Setup

Install and manage the version of nodejs by using nvm. nvm can not manage nodeJs not installed via nvm.    
Therefore, if nodeJs not installed through the existing nvm are already installed, delete and reinstall them.    


### NVM Install

```
>curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

>source ~/.nvm/nvm.sh

>nvm install 8.9.4

>nvm use 8.9.4
```

## 3. Redis Setup

Download And Unzip

```
>wget http://download.redis.io/redis-stable.tar.gz

>tar xvzf redis-stable.tar.gz

```

Move To redis-stable folder

```
>cd redis-stable

>make

>sudo make install


>redis-server
```

# NOMP Install

```
>git clone https://github.com/Hdactech/nomp.git nomp
>cd nomp
>npm update
```

## 1. NOMP Main Constructor

```
nomp
┖ coins
       - many coin.json 
	.
	.
┖ libs
┖ node_modules
    ┖ other libs
    ┖ multi-hashing
    ┖ stratum-pool
       ┖ lib
          - many js
		.
		.
┖ pool_configs
   - litecoin_example.json
   - vertcoin_example.json
┖ scripts
┖ website
- config.json
- init.js 
```

## 2. Main Configuration

##### 1. Portal Configuration

###### config.json
```javascript
{
    /* Specifies the level of log output verbosity. Anything more severe than the level specified
       will also be logged. */
    "logLevel": "debug", //or "warning", "error"
    
    /* By default NOMP logs to console and gives pretty colors. If you direct that output to a
       log file then disable this feature to avoid nasty characters in your log file. */
    "logColors": true, 


    /* The NOMP CLI (command-line interface) will listen for commands on this port. For example,
       blocknotify messages are sent to NOMP through this. */
    "cliPort": 17117,

    /* By default 'forks' is set to "auto" which will spawn one process/fork/worker for each CPU
       core in your system. Each of these workers will run a separate instance of your pool(s),
       and the kernel will load balance miners using these forks. Optionally, the 'forks' field
       can be a number for how many forks will be spawned. */
    "clustering": {
        "enabled": true,
        "forks": "auto"
    },
    
    /**
     * @Hdac
     * Add the 'blockWindowSizeRefreshInterval' key value.
     * This is used to set the schedule period associated with ePOW application.
     * This annotation should be removed when reflecting on the server due to the JSON formatting convention.
     */
    "defaultPoolConfigs": {
        "blockRefreshInterval": 1000,
        "blockWindowSizeRefreshInterval": 50000,
        "jobRebroadcastTimeout": 55,
        "connectionTimeout": 600,
        "emitInvalidBlockHashes": false,
        "validateWorkerUsername": true,
        "tcpProxyProtocol": false,
        "banning": {
            "enabled": false,
            "time": 600,
            "invalidPercent": 50,
            "checkThreshold": 500,
            "purgeInterval": 300
        },
        "redis": {
            "host": "127.0.0.1",
            "port": 6379
        }
    },

    /* This is the front-end. Its not finished. When it is finished, this comment will say so. */
    "website": {
        "enabled": true,
        /* If you are using a reverse-proxy like nginx to display the website then set this to
           127.0.0.1 to not expose the port. */
        "host": "0.0.0.0",
        "port": 80,
        /* Used for displaying stratum connection data on the Getting Started page. */
        "stratumHost": "cryppit.com",
        "stats": {
            /* Gather stats to broadcast to page viewers and store in redis for historical stats
               every this many seconds. */
            "updateInterval": 15,
            /* How many seconds to hold onto historical stats. Currently set to 24 hours. */
            "historicalRetention": 43200,
            /* How many seconds worth of shares should be gathered to generate hashrate. */
            "hashrateWindow": 300
        },
        /* Not done yet. */
        "adminCenter": {
            "enabled": true,
            "password": "password"
        }
    },

    /* Redis instance of where to store global portal data such as historical stats, proxy states,
       ect.. */
    "redis": {
        "host": "127.0.0.1",
        "port": 6379
    },


    /* With this switching configuration, you can setup ports that accept miners for work based on
       a specific algorithm instead of a specific coin. Miners that connect to these ports are
       automatically switched a coin determined by the server. The default coin is the first
       configured pool for each algorithm and coin switching can be triggered using the
       cli.js script in the scripts folder.

       Miners connecting to these switching ports must use their public key in the format of
       RIPEMD160(SHA256(public-key)). An address for each type of coin is derived from the miner's
       public key, and payments are sent to that address. */
    "switching": {
        "switch1": {
            "enabled": false,
            "algorithm": "sha256",
            "ports": {
                "3333": {
                    "diff": 10,
                    "varDiff": {
                        "minDiff": 16,
                        "maxDiff": 512,
                        "targetTime": 15,
                        "retargetTime": 90,
                        "variancePercent": 30
                    }
                }
            }
        },
        "switch2": {
            "enabled": false,
            "algorithm": "scrypt",
            "ports": {
                "4444": {
                    "diff": 10,
                    "varDiff": {
                        "minDiff": 16,
                        "maxDiff": 512,
                        "targetTime": 15,
                        "retargetTime": 90,
                        "variancePercent": 30
                    }
                }
            }
        },
        "switch3": {
            "enabled": false,
            "algorithm": "x11",
            "ports": {
                "5555": {
                    "diff": 0.001
                }
            }
        }
    },

    "profitSwitch": {
        "enabled": false,
        "updateInterval": 600,
        "depth": 0.90,
        "usePoloniex": true,
        "useCryptsy": true,
        "useMintpal": true
    }
}

```

##### 2. Coin Configuration

###### config.json

There are a number of coin configuration files in the coin folder. The coin setting information of the **`HDAC`** is as follows.

```javascript
{
    "name": "HDAC",
    "symbol": "DAC",
    "algorithm": "lyra2rev2",
    /**
     * @Hdac
     * Add the 'reward' key value for ePoW, a unique consensus algorithm of Hdac.
     * This annotation should be removed when reflecting on the server due to the JSON formatting convention.
     */
    "reward": "ePoW"
}
```
Check [here](https://github.com/zone117x/node-stratum-pool#module-usage) for how to set the algorithm setting information of the other coins.


##### 3. Pool Configuration

###### h-dac.json

The **`HDAC`** Pool Configuration defined in the pool_configs folder is as follows.

```javascript
{
    "enabled": true, //Set this to false and a pool will not be created from this config file
    "coin": "hdac.json", //Reference to coin config file in 'coins' directory

    "address": "hdac main address", //Address to where block rewards are given

	/* Block rewards go to the configured pool wallet address to later be paid out to miners,
       except for a percentage that can go to, for examples, pool operator(s) as pool fees or
       or to donations address. Addresses or hashed public keys can be used. Here is an example
       of rewards going to the main pool op, a pool co-owner, and NOMP donation. */
    "rewardRecipients": {
        "hdac reward address": 1
    },
    
    /* Not Supported as security issue. Do not use */
    "paymentProcessing": {
        "enabled": false,
        "paymentInterval": 30,
        "minimumPayment": 0.01,
        "daemon": {
            "host": "127.0.0.1",
            "port": 19332,
            "user": "testuser",
            "password": "testpass"
        }
    },

	/* Each pool can have as many ports for your miners to connect to as you wish. Each port can
       be configured to use its own pool difficulty and variable difficulty settings. varDiff is
       optional and will only be used for the ports you configure it for. */
    "ports": {
        "3008": {
            "diff": 0.00002
        },
        "3333": {
            "diff": 0.00002,
            "varDiff": {
                "minDiff": 0.00002,
                "maxDiff": 256,
                "targetTime": 15,
                "retargetTime": 90,
                "variancePercent": 30
            }
        },
        "3256": {
            "diff": 1
        }
    },

  	/* More than one daemon instances can be setup in case one drops out-of-sync or dies. */
    "daemons": [
        {
            "host": "set your hdac ip",
            "port": "set your hdac port",
            "user": "set your hdac user",
            "password": "set your hdac password"
        }
    ],
    
	/* This allows the pool to connect to the daemon as a node peer to receive block updates.
       It may be the most efficient way to get block updates (faster than polling, less
       intensive than blocknotify script). It requires the additional field "peerMagic" in
       the coin config. */
    "p2p": {
        "enabled": false,

        /* Host for daemon */
        "host": "127.0.0.1",

        /* Port configured for daemon (this is the actual peer port not RPC port) */
        "port": 19333,

        /* If your coin daemon is new enough (i.e. not a shitcoin) then it will support a p2p
           feature that prevents the daemon from spamming our peer node with unnecessary
           transaction data. Assume its supported but if you have problems try disabling it. */
        "disableTransactions": true
    },
    
    /* Enabled this mode and shares will be inserted into in a MySQL database. You may also want
       to use the "emitInvalidBlockHashes" option below if you require it. The config options
       "redis" and "paymentProcessing" will be ignored/unused if this is enabled. */
    "mposMode": {
        "enabled": false,
        "host": "set your mysql host",
        "port": 3306,
        "user": "set your mysql user id",
        "password": "set your mysql password",
        "database": "mpos",
        "checkPassword": true,
        "autoCreateWorker": false
    },

}
```

##### 4. Server Start
Into nomp folder
```
node init.js
```

## About ePoW And Stratum Pool

See [Here](https://github.com/Hdactech/stratum-pool)


## More NOMP

For more information about NOMP, check the following [author's notes](https://github.com/zone117x/node-open-mining-portal) on gihub.
