---
layout: default
title: User Application
nav_order: 8
has_children: false
permalink: /docs/User-Application
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Overview of DA App

## DA (Data Analytics) App

DA App enables edge computing advantage in the reader. DA App allows advanced filtering and analytics to be done before sending the tag data from the reader. 

The following are the core functions:

 - Filter, Analyze and act upon data 
 - Manage and control the reader via REST 
 - Implement Business logic and 
 - Control GPO

The reader supports hosting embedded applications developed in Python & Node.js.

# Python DA apps
The Data Analytics App can be implemented as Python using Zebra IOTC Python module available in the reader.

## ZIOTC Python Module
The Python module **pyziotc** abstracts the Zebra IoT Connector and allow the script to filter, analyze and act on the data.

The flow of the script is as follows:

 1. Define a new callback function.
	```
    def new_msg_callback(msg_type, msg_in):
    	    pass
	```
 2. Open pyziotc module. Establish connection between Python and IoT Connector
	```
    import pyziotc
    ziotcObject = pyziotc.Ziotc()
	```

 3. Register callback function. This function is called when new message arrives.
	```
    ziotcObject.reg_new_msg_callback(new_msg_callback)
	```
 
 ## New Message Callback

### Input Message Types
The user-defined new message callback function is the core of the script. The callback definition must contain 2 parameters, msg_type and msg_in. The function can then process the input message and send out an output message based on the input. All messages shall be Python  [bytearrays](https://docs.python.org/3/library/stdtypes.html#bytearray)

For example, the following callback function, only sends out a data message if the beginning of the tag’s ID begins with a prefix.

```
prefix = "38"
def new_msg_callback(msg_type, msg_in):
    if msg_type == pyziotc.MSG_IN_JSON:
        msg_in_json = json.loads(msg_in.decode('utf-8'))
        tag_id_hex = msg_in_json["data"]["idHex"]

    if tag_id_hex.startswith(prefix):
        ziotcObject.send_next_msg(pyziotc.MSG_OUT_DATA, msg_in) 
```

## Sample Python DA App



# Node.js DA apps


