# The purpose of this repo is to guide a user creating an Opentelemetry agent / gateway config.
- This could be beneficial to you if you need to collect data from secure networks that only have the ability to talk to a DMZ or to a 'jumpbox'. 

- There are two components to this setup: 
    - 1. Creating an agent config. Please see [agent-config.md](/agent/agent-config.md)
    - 2. Creating a gateway config. Please see [gateway-config.md](/gateway/gateway-config.md)

*Improvements coming*: 

- Adding a file parser to handle how the data is processed to the gateway. 
- Adding another exporter to write a copy of the data to another directory (whether local or a network share)
