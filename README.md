
This plugin collects metrics from MongoDB database.  

It's used in the [Snap framework](http://github.com:intelsdi-x/snap).

1. [Getting Started](#getting-started)
  * [System Requirements](#system-requirements)
  * [Operating systems](#operating-systems)
  * [Installation](#installation)
  * [Configuration and Usage](#configuration-and-usage)
2. [Documentation](#documentation)
  * [Global Config](#global-config)
  * [Collected Metrics](#collected-metrics)
  * [Examples](#examples)
  * [Roadmap](#roadmap)
3. [Community Support](#community-support)
4. [Contributing](#contributing)
5. [License](#license-and-authors)
6. [Acknowledgements](#acknowledgements)

## Getting Started
### System Requirements
* [golang 1.7+](https://golang.org/dl/)  - needed only for building
* [mongodb 3.2.x](https://mongodb.com/)
### Operating systems
All OSs currently supported by snap:
* Linux/amd64
* Darwin/amd64

### Installation


#### Download the plugin binary:

You can get the pre-built binaries for your OS and architecture from the plugin's [GitHub Releases](https://github.com/intelsdi-x/snap-plugin-collector-mongodb/releasess) page. Download the plugin from the latest release and load it into `snapd` (`/opt/snap/plugins` is the default location for snap packages).


#### To build the plugin binary:

Fork https://github.com/intelsdi-x/snap-plugin-collector-mongodb
Clone repo into `$GOPATH/src/github.com/intelsdi-x/`:

```
$ git clone https://github.com/<yourGithubID>/snap-plugin-collector-mongodb.git
```

Build the snap mongodb plugin by running make within the cloned repo:
```
$ make
```
This builds the plugin in `./build/`

### Configuration and Usage
* Set up the [snap framework](https://github.com/intelsdi-x/snap/blob/master/README.md#getting-started).
* Configure connection strings to MongoDB service which includes username , password , hostname (default "localhost:27017").

* Load the plugin and create a task, see example in [Examples](#examples).

## Documentation

This collector gathers metrics from mongodb server status command. 

### Global config
Global configuration files are described in [snap's documentation](https://github.com/intelsdi-x/snap/blob/master/docs/SNAPD_CONFIGURATION.md). You have to add `"mongodb"` section with following entries:

 - `"uri"` -  it's hostname and port pair in format `"hostname:port"`,  as default set to `"localhost:27017"`. 
 - `"username"` - username if authentification is enabled
 - `"password"` - password if authentification is enabled

See exemplary Global configuration files in [examplary config files] (examples/configs/).

### Collected Metrics

List of collected metrics is described in [METRICS.md](METRICS.md).

### Examples

Example of running snap mongodb collector and writing data to file.

Ensure [snap daemon is running](https://github.com/intelsdi-x/snap#running-snap):
* initd: `service snap-telemetry start`
* systemd: `systemctl start snap-telemetry`
* command line: `snapd -l 1 -t 0 &`

Download and load snap plugins:
```
$ wget http://snap.ci.snap-telemetry.io/plugins/snap-plugin-collector-mongodb/latest/linux/x86_64/snap-plugin-collector-mongodb
$ wget http://snap.ci.snap-telemetry.io/plugins/snap-plugin-publisher-file/latest/linux/x86_64/snap-plugin-publisher-file
$ chmod 755 snap-plugin-*
$ snapctl plugin load snap-plugin-collector-mongodb
$ snapctl plugin load snap-plugin-publisher-file

Create a task manifest file  (exemplary files in [examples/tasks/] (examples/tasks/)):
```yaml
---
  version: 1
  schedule:
    type: "simple"
    interval: "1s"
  max-failures: 10
  workflow:
    collect:
      metrics:
         /intel/mongodb/opscounters/insert: {}
         /intel/mongodb/opscounters/query: {}
         /intel/mongodb/opscounters/update: {}
         /intel/mongodb/opscounters/delete: {}
         /intel/mongodb/opscounters/getmore: {}
         /intel/mongodb/opscounters/command: {}
         /intel/mongodb/tmalloc/current_allocated_bytes: {}
         /intel/mongodb/tmalloc/heap_size: {}
         /intel/mongodb/tmalloc/pageheap_free_bytes: {}
         /intel/mongodb/tmalloc/pageheap_unmapped_bytes: {}
         /intel/mongodb/tmalloc/max_total_thread_cache_bytes: {}
         /intel/mongodb/tmalloc/current_total_thread_cache_bytes: {}
         /intel/mongodb/tmalloc/total_free_bytes: {}
         /intel/mongodb/tmalloc/central_cache_free_bytes: {}
         /intel/mongodb/tmalloc/transfer_cache_free_bytes: {}
         /intel/mongodb/tmalloc/thread_cache_free_bytes: {}
         /intel/mongodb/tmalloc/aggressive_memory_decommit: {}
      publish:
        - plugin_name: "file"
          config:
            file: "/tmp/mongodb_metrics.log"
```
Download an [example task file](https://github.com/intelsdi-x/snap-plugin-collector-mongodb/blob/master/examples/tasks/) and load it:
```
$ curl -sfLO https://raw.githubusercontent.com/intelsdi-x/snap-plugin-collector-mongodb/master/examples/tasks/mongodbb-file.json
$ snapctl task create -t mongodb-file.json
Using task manifest to create task
Task created
ID: 480323af-15b0-4af8-a526-eb2ca6d8ae67
Name: Task-480323af-15b0-4af8-a526-eb2ca6d8ae67
State: Running
```

See realtime output from `snapctl task watch <task_id>` (CTRL+C to exit)
```
$ snapctl task watch 480323af-15b0-4af8-a526-eb2ca6d8ae67
```

This data is published to a file `/tmp/mongodb_metrics` per task specification

Stop task:
```
$ snapctl task stop 480323af-15b0-4af8-a526-eb2ca6d8ae67
Task stopped:
ID: 480323af-15b0-4af8-a526-eb2ca6d8ae67
```



### Roadmap
There isn't a current roadmap for this plugin, but it is in active development. As we launch this plugin, we do not have any outstanding requirements for the next release. 

If you have a feature request, please add it as an [issue](https://github.com/intelsdi-x/kubesnap-plugin-collector-mongodb/issues/new) and/or submit a [pull request](https://github.com/intelsdi-x/kubesnap-plugin-collector-mongodb/pulls).

## Community Support
This repository is one of **many** plugins in **snap**, a powerful telemetry framework. See the full project at http://github.com/intelsdi-x/snap.

To reach out to other users, head to the [main framework](https://github.com/intelsdi-x/snap#community-support) or visit [Slack](http://slack.snap-telemetry.io).

## Contributing
We love contributions!

There's more than one way to give back, from examples to blogs to code updates. See our recommended process in [CONTRIBUTING.md](CONTRIBUTING.md).

## License
Snap, along with this plugin, is an Open Source software released under the Apache 2.0 [License](LICENSE).

## Acknowledgements
* Author: [@Marcin Spoczynski](https://github.com/sandlbn/)