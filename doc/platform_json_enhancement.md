# Platform capability file enhancement #

### Rev 0.1

## Table of Content 

## Revision  

 | Rev |     Date    |            Author            | Change Description   |
 |:---:|:-----------:|:----------------------------:|----------------------|
 | 0.1 |             | Arun Saravanan Balachandran  | Initial version      |

## Scope

This document provides information on the enhancements for platform capability file `platform.json`.

## Definitions/Abbreviations 

| Definitions/Abbreviation | Description |
|--------------------------|-------------|
| BMC | Baseband Management Controller |
| NOS | Network Operating System |
| PSU | Power Supply Unit |

## Overview 

Each networking switch has a set of platform components (e.g: Fans, PSUs, LEDs, etc.) and these components in each platform can have different characteristics (like supported colors for a LED). In a given platform, the components could be controlled by a dedicated platform controller (like BMC) or the NOS running on the CPU is required to control it and in the former the control of the components from the NOS could be limited.

In SONiC the platform components' supported attributes are made available via Platform API, but certain platform specific capabilties for the components are not available for the applications.

This document provides the enhancement for `platform.json` to address the above issue.

## Design 

Currently, `platform.json` is used for providing the expected structure of the platform components and interface details for supporting dynamic port breakout.

### Platform capabilities field 

A new `capabilities` field is introduced in platform.json for providing platform specific capablities on control and characteristics of the components.

Sample `capabilities` field is as below:
```
"capabilities": {
	"chassis": {
	    "status_led": {
	    	"control": true,
		"colors": ["off", "amber", "green"]
	     },
	     "fan_drawers": {
	     	 "fans": {
		     "speed": {
		         "control": true,
		         "minimum": 40
		      }
		  },
		 "status_led": {
		      "control": true,
		      "colors": ["red", "green"]
		  }
              },
	      "psus": {
	           "fans": {
		       "speed": {
		           "control": false
		       }
	            },  
		    "status_led": {
		        "control": false
	            }
	      },
	      "thermals": {
	      	    "control": false
	      }

 	 ...
 }
```

In the `capablities` field, the platform components are provided in a hierarchical structure. For each component's attribute, the defined fields are as follows:
- "control" : A boolean, 'true' if the given attribute can be controlled from the NOS,'false' otherwise.
- Attribute specific fields:
	- status led - "color" - A list of the supported colors.
	- speed - "minimum" - Minimum recommended fan speed that can be set.

### SAI API 

### Configuration and management 

#### CLI Enhancements 

#### Config DB Enhancements  
		
### Warmboot and Fastboot Design Impact  

### Restrictions/Limitations  

### Testing Requirements/Design  

#### Unit Test cases  

#### System Test cases

### Open/Action items - if any 
