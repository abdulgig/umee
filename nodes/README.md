# Umee Node - Quick Setup 

The instructions below allow you to setup an Umee node within a minute. This is super fast and easy ! Under the "settings" directory, you can customize the configuration as per your requirements.

This is a dockerized and a parameterized setup. The paramaters can be optionally be overridden with your own values. 

## How to launch the node ? 

### Step 1 - Clone and build your node image

Clone the repo and navigate to the nodes folder and build your own node image. 

**You can also modify the go version in your build. In the Dockerfile, modify the ENV variable GO_VERSION , default is 1.20.** However, please ensure that your go version is compatible with the umeed binary you are going to use.

Here _my_umee_image_ is the name of your image you are going to build. Don't miss the **"."** in the end of the command

```
git clone https://github.com/umee-network/umee-infra.git -b main
cd umee-infra/nodes/single-node/
docker build -t my_umee_image:v1 .
```

### Step 2 - Launch your node

Once the node image is successfully built, you should see something like,

```
Removing intermediate container 7326b730beb4
 ---> c5f6bc6c639a
Step 15/15 : EXPOSE 26656 9090 1317
 ---> Running in d906a9899524
Removing intermediate container d906a9899524
 ---> c14c4d11e713
Successfully built c14c4d11e713
Successfully tagged my_umee_image:v1
```

Now you can launch the node, here is a sample command below,

```
docker run -dit --name umee-node --hostname umee-node my_umee_image:v1 bash
```

To modify node options on the fly, try using the command below

```
docker run -dit --name umee-node --hostname umee-node \
  -e UMEE_BINARY_VERSION=v4.2.0 \
  -e CONFIG_OVERRIDE=0 \
  -e MONIKER=my-node \
  -e CHAIN_ID=umee-001 \
  -e DB_BACKEND=badgerdb \
  -e MIN_GAS_PRICE=0.1uumee \
  -e LOG_LEVEL=info \
  -p 26656:26656 \
  -p 9090:9090 \
  -p 1317:1317 \
  my_umee_image:v1 

```

#### Understanding ENV variables

| FLAG / ENV VARIABLES           | VALUES                      |
|------------------------------- |-----------------------------|
| UMEE_BINARY_VERSION            | Set the binary you want to use, Default = v4.2.0|
| CONFIG_OVERRIDE                | Set it to 1 if you want to launch the node based upon the config files under the "settings" directory,  Default = 0|
| MONIKER                        | Set the node identifier, default is randomly generated|
| CHAIN_ID                       | Set the chainID, default is randomly generated|
| DB_BACKEND                     | Set the DB_BACKEND [goleveldb, cleveldb, boltdb, rocksdb, badgerdb], default is rocksdb|
| MIN_GAS_PRICE                  | Set MIN_GAS_PRICE, default is 0.1uumee|
| LOG_LEVEL                      | Set the log level, default is info|

Modify the **-p** flag and expose the ports based upon your requirements.


### Step 3 - Test your node

If your node is deployed successfully, run ```docker ps``` to verify it. You should see a running container ```umee-node```

Next you need to bash into your node(container) to verify if all services are up, try running the following commands,

To bash in,
```
docker exec -it umee-node bash
```

Once you have access to the shell, try running and you should see all service running,

```
netstat -lntupe

root@umee-node:/umee# netstat -lntupe
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode      PID/Program name    
tcp        0      0 127.0.0.1:6060          0.0.0.0:*               LISTEN      0          159002989  230/umeed           
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          159002983  1/sshd: /usr/sbin/s 
tcp        0      0 127.0.0.1:26657         0.0.0.0:*               LISTEN      0          158989300  230/umeed           
tcp6       0      0 :::9090                 :::*                    LISTEN      0          158993161  230/umeed           
tcp6       0      0 :::26656                :::*                    LISTEN      0          158989306  230/umeed           
tcp6       0      0 :::22                   :::*                    LISTEN      0          159002985  1/sshd: /usr/sbin/s 
tcp6       0      0 :::1317                 :::*                    LISTEN      0          158991232  230/umeed         
```

You can see live logs under a tmux-session, run ```tmux at```

```
64F0F1C4D77DCB8682315D3CD9D56285A46F6223BF5F90BA211E26","total":1}},"height":24,"pol_round":-1,"round":0,"signature":"j9sUF7izMNxPeedADXQkTwBdcrTw5GQwY/93v1AJIjfiEELcNH3kbPjK20u4ZTghjv2bBG47AOFgF8NnqS9VBA==","timestamp":"2023-03-20T07:01:46.178544125Z"}
7:01AM INF received complete proposal block hash=0F482FAF009F08574B69336F35E70AF7A734A985019745694B60D23A062189B6 height=24 module=consensus
7:01AM INF finalizing commit of block hash={} height=24 module=consensus num_txs=0 root=84E37BC2E4B20865BA1154585E8A9B42C17F239626D7841CFC579548A5290945
7:01AM INF minted coins from module account amount=41194uumee from=mint module=x/bank
7:01AM INF executed block height=24 module=state num_invalid_txs=0 num_valid_txs=0
7:01AM INF commit synced commit=436F6D6D697449447B5B32323220313638203130322032323220323131203233342031333720313435203838203135312032313520393420313936203131322035203135312032313120313733203839203131312032333420393720373520313138203138392032313020313420323530203133322031393920323530203231315D3A31387D
7:01AM INF committed state app_hash=DEA866DED3EA89915897D75EC4700597D3AD596FEA614B76BDD20EFA84C7FAD3 height=24 module=state num_txs=0
7:01AM INF indexed block exents height=24 module=txindex

```


