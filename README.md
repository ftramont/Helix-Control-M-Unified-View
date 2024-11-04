# Helix Control-M Unified View

### Short description
Control-M CLI commands to implement the Helix Control-M Unified View.
 
### Detailed description

Please refer to the following technote for a comprehensive overview of the Helix Control-M Unified View, 
including its value, use cases, and a step-by-step guide on implementing it through the Control-M and Helix Control-M web interfaces.

Below you'll find the Control-M and Helix Control-M CLI commands whereas they can be used as alternatives to the web interface. 

For more details on using the CLI, please refer to the following documentation: [Control-M Automation API Guidelines](https://documents.bmc.com/supportu/API/Monthly/en-US/Documentation/Automation_API_Guidelines.htm?)

#### Pre requisites

Control-M Version 9.0.21.300

### Getting Started

The following steps outline the process for achieving the unified view. 

* #### 1. Migrate EM data from self-hosted to Helix Control-M
* #### 2. Disconnect a server from self-hosted Control-M/EM
* #### 3. Connect a server to Helix Control-M/EM
* #### 4. Repeat for all servers

CLI instructions will be provided for steps 2 and 3, specifically for Control-M/Server running on Linux. 

### 2. Disconnect a server from self-hosted Control-M/EM

From the self-hosted Control-M command line use `ctm` utility to perform all the steps below, except step d.

#### a. Pause the server
Pausing prevents the execution of new jobs until you resume the server.

```ruby 
ctm config server::pause <server>
```

For example:

```ruby 
ctm config server::pause "smprod"
```
**Note:** Ensure to pause all servers before proceeding with the next individual server steps. If you pause only one server while the others are still processing, it will miss events raised from the other servers, leading to a loss of synchronization and coordination.

#### b. Disable the server
Disabling removes the ability to monitor and manage the server and its components.

```ruby 
ctm config server::disable <server>
```

For example:

```ruby 
ctm config server::disable "smprod"
```

#### c. Unmanage the server
Unmanaging disables the ability to change the server status.

```ruby 
ctm config server::unmanaged <server> <host>
```

For example:

```ruby 
ctm config server::unmanaged "smprod" "ip-xx-xx-xx-xx.region.compute.internal"
```

#### d. Stop the gateway
Stopping the gateway halts the communication between the server and its current Control-M/EM.

Run the `ccmcli` utility from the a self-hosted Control-M/EM account to perform this instruction. 
For more details on this utility, please refer to the documentation: [ccmcli](https://documents.bmc.com/supportu/9.0.21.300/en-US/Documentation/Utilities/ccmcli.htm?).

```ruby 
em ccmcli -u <Username> -p <Password> -t Gateway -n <Component_Name> -h <Component_Host> -cmd stop
```

For example:

```ruby
em ccmcli -u "emuser" -p "********" -t Gateway -n "smprod" -h "ip-xx-xx-xx-xx.region.compute.internal" -cmd stop
```

### 3. Connect a server to Helix Control-M/EM

The first step in this section is to add the self-hosted server to Helix Control-M. 
This step must be completed using the web interface. For more details, please refer to Step 3a in the [technote](tbd) for more details.

#### b.	Register the server
Registering establishes a connection between the self-hosted Control-M/Server and Helix Control-M/EM.

Use the `ctm_menu` utility from a self-hosted Control-M/Server account line to perform this instruction. 
For more details on this utility, please refer to the documentation: [ctm_menu](https://documents.bmc.com/supportu/9.0.21.300/en-US/Documentation/Utilities/ctm_menu.htm)).

```
ctm_menu <enter>
1 - CONTROL-M Manager <enter>
11 - Register Control-M/Server in Helix Control-M/EM <enter>
Enter the token obtained with the previous step
```

**Note:** Ensure that all servers have been successfully registered before proceeding with the next step.

#### c.	Resume the server
Resuming changes the server Actual State to Up and allows to start job executions.

```ruby 
ctm config server::resume "smprod"
```

### 4. Repeat for all servers

You'll complete the process once all steps have been executed for each server. At that point, the self-hosted Control-M/EM will be ready for decommissioning. 
