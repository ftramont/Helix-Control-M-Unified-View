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
* #### 3. Connect the server to Helix Control-M/EM
* #### 4. Repeat for each server

CLI instructions will be provided for steps 2 and 3, specifically for Control-M/Server running on Linux. 

### 2. Disconnect a server from self-hosted Control-M/EM

From the self-hosted Control-M command line use _ctm_ utility to perform this instruction. 
For each server to be disconnected, follow these steps.

#### a. Pause the server
Pausing prevents the execution of new jobs until you resume the server.

```ruby 
ctm config server::pause <server>
```

For example:

```ruby 
ctm config server::pause "smprod"
```
**Note:** All servers must be paused before proceeding with the next step. If only one server is still up while the others are paused, events raised by that server will not be captured by the paused servers.

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




