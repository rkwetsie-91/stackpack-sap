#### Prerequisites

The following prerequisites need to be met:

* StackState Agent V2 must be installed on a single machine which can connect to SAP Instance and StackState. (See the [StackState Agent V2 StackPack](/#/stackpacks/stackstate-agent-v2/) for more details)
* A SAP instance must be running.


#### Enabling SAP check
To enable the SAP check which collects the data from SAP host instance:

Edit the `conf.yaml` file in your agent `/etc/stackstate-agent/conf.d/sap.d` directory, replacing `<sap_host_name>` and `<sap_host_url>` with the unique name of SAP host to identify and URL to connect from your SAP instance.

To Connect to your SAP System, there are 2 ways to do so as explained below:

##### 1. HTTP Basic Authentication Mechanism

This mechanism allows you to connect just using the `username` and `password` inside the `conf.yaml`.

_As an example, see the below config :_

```
# Section used for global SAP check config
init_config: {}

instances:
    - host: TEST-01         # <sap_host_name> 
      url: http://test-01   # <sap_host_url>   
      user: test            # <username>
      pass: test            # <password> 
```
**NOTE** - Make sure while using this mechanism, you have put `http` in the `url` of the config.


##### 2. Client Certificate Authentication Mechanism

This mechanism allows you to connect using the client certificate and the private key. 
The following new parameters are available :
* `verify` - `True` or `False` depending on verifying the client certificate or not. By default, it's `True`.
* `cert` - path containing the client side certificate
* `keyfile` - path containing the private key for certificate

_As an example, see the below config :_

```
# Section used for global SAP check config
init_config: {}

instances:
    - host: TEST-01             # <sap_host_name> 
      url: https://test-01      # <sap_host_url>   
      user: test                # <username>
      pass: test                # <password> 
      verify: False
      cert: /path/to/cert.pem   # <certificate_path>
      keyfile: /path/to/key.pem # <keyfile_path>
```
**NOTE** - Make sure while using this mechanism, you have put `https` in the `url` of the config.

Once the configuration changes are done as explained above, restart the StackState Agent(s) using the following command.

```
sudo /etc/init.d/stackstate-agent restart
```

Once the Agent is restarted, wait for the Agent to collect the data and send it to StackState.
