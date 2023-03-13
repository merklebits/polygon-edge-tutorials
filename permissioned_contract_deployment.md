# Permissioned Contract Deployment with Polygon Edge

To blacklist / whitelist specific addresses for contract deployment, simply edit the `genesis.json` file prior to starting the network.

Add the following lines under `params` 

```
"whitelists":{
    "deployment":[
        "0x7b36121C404fCe5385418941C3D44A1D5db20541"
    ]
},
"blacklists":{
    "deployment":[
        "0xba283e439Bd444B595Dd4790f662aD073cD103eE"
    ]
}
```

You can add as many addresses as you like to in the `deployment` array.