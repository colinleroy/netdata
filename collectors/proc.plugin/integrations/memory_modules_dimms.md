<!--startmeta
custom_edit_url: "https://github.com/netdata/netdata/edit/master/collectors/proc.plugin/integrations/memory_modules_dimms.md"
meta_yaml: "https://github.com/netdata/netdata/edit/master/collectors/proc.plugin/metadata.yaml"
sidebar_label: "Memory modules (DIMMs)"
learn_status: "Published"
learn_rel_path: "Data Collection/Linux Systems/Memory"
message: "DO NOT EDIT THIS FILE DIRECTLY, IT IS GENERATED BY THE COLLECTOR'S metadata.yaml FILE"
endmeta-->

# Memory modules (DIMMs)

Plugin: proc.plugin
Module: /sys/devices/system/edac/mc

<img src="https://img.shields.io/badge/maintained%20by-Netdata-%2300ab44" />

## Overview

The Error Detection and Correction (EDAC) subsystem is detecting and reporting errors in the system's memory,
primarily ECC (Error-Correcting Code) memory errors.

The collector provides data for:

- Per memory controller (MC): correctable and uncorrectable errors. These can be of 2 kinds:
  - errors related to a DIMM
  - errors that cannot be associated with a DIMM

- Per memory DIMM: correctable and uncorrectable errors. There are 2 kinds:
  - memory controllers that can identify the physical DIMMS and report errors directly for them,
  - memory controllers that report errors for memory address ranges that can be linked to dimms.
    In this case the DIMMS reported may be more than the physical DIMMS installed.




This collector is supported on all platforms.

This collector supports collecting metrics from multiple instances of this integration, including remote instances.


### Default Behavior

#### Auto-Detection

This integration doesn't support auto-detection.

#### Limits

The default configuration for this integration does not impose any limits on data collection.

#### Performance Impact

The default configuration for this integration is not expected to impose a significant performance impact on the system.


## Metrics

Metrics grouped by *scope*.

The scope defines the instance that the metric belongs to. An instance is uniquely identified by a set of labels.



### Per memory controller

These metrics refer to the memory controller.

Labels:

| Label      | Description     |
|:-----------|:----------------|
| controller | [mcX](https://www.kernel.org/doc/html/v5.0/admin-guide/ras.html#mcx-directories) directory name of this memory controller. |
| mc_name | Memory controller type. |
| size_mb | The amount of memory in megabytes that this memory controller manages. |
| max_location | Last available memory slot in this memory controller. |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| mem.edac_mc | correctable, uncorrectable, correctable_noinfo, uncorrectable_noinfo | errors/s |

### Per memory module

These metrics refer to the memory module (or rank, [depends on the memory controller](https://www.kernel.org/doc/html/v5.0/admin-guide/ras.html#f5)).

Labels:

| Label      | Description     |
|:-----------|:----------------|
| controller | [mcX](https://www.kernel.org/doc/html/v5.0/admin-guide/ras.html#mcx-directories) directory name of this memory controller. |
| dimm | [dimmX or rankX](https://www.kernel.org/doc/html/v5.0/admin-guide/ras.html#dimmx-or-rankx-directories) directory name of this memory module. |
| dimm_dev_type | Type of DRAM device used in this memory module. For example, x1, x2, x4, x8. |
| dimm_edac_mode | Used type of error detection and correction. For example, S4ECD4ED would mean a Chipkill with x4 DRAM. |
| dimm_label | Label assigned to this memory module. |
| dimm_location | Location of the memory module. |
| dimm_mem_type | Type of the memory module. |
| size | The amount of memory in megabytes that this memory module manages. |

Metrics:

| Metric | Dimensions | Unit |
|:------|:----------|:----|
| mem.edac_mc | correctable, uncorrectable | errors/s |



## Alerts


The following alerts are available:

| Alert name  | On metric | Description |
|:------------|:----------|:------------|
| [ ecc_memory_mc_noinfo_correctable ](https://github.com/netdata/netdata/blob/master/health/health.d/memory.conf) | mem.edac_mc | memory controller ${label:controller} ECC correctable errors (unknown DIMM slot) in the last 10 minutes |
| [ ecc_memory_mc_noinfo_uncorrectable ](https://github.com/netdata/netdata/blob/master/health/health.d/memory.conf) | mem.edac_mc | memory controller ${label:controller} ECC uncorrectable errors (unknown DIMM slot) in the last 10 minutes |
| [ ecc_memory_dimm_correctable ](https://github.com/netdata/netdata/blob/master/health/health.d/memory.conf) | mem.edac_mc_dimm | DIMM ${label:dimm} controller ${label:controller} (location ${label:dimm_location}) ECC correctable errors in the last 10 minutes |
| [ ecc_memory_dimm_uncorrectable ](https://github.com/netdata/netdata/blob/master/health/health.d/memory.conf) | mem.edac_mc_dimm | DIMM ${label:dimm} controller ${label:controller} (location ${label:dimm_location}) ECC uncorrectable errors in the last 10 minutes |


## Setup

### Prerequisites

No action required.

### Configuration

#### File

There is no configuration file.
#### Options



There are no configuration options.

#### Examples
There are no configuration examples.

