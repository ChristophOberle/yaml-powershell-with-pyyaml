# yaml-powershell-with-pyyaml
read YAML config files into Powershell objects using python with PyYaml

## Motivation
There is no native support for YAML files in Powershell.

Some implementations are based on a .NET library, only available under Windows.

This implementation uses python3 with the PyYaml package.

## Requirements
* python3 is available
* python package PyYaml is installed
* python program json_from_yaml.py is available
  
## Usage
Import the Powershell module YamlModule.psm1

``Import-Module -name YamlModule.psm1``

Read a YAML file into a Powershell object

``$config = ConvertFrom-Yaml -ProgramDir <path to the json_from_yaml.py script> -YamlFile <path to the YAML file to be read>``

Assuming the YAML files starts like this:

```
WillibaldChallenge_DataWarehouse:
  target: dev
  outputs:
    dev:
      type: postgres
      host: localhost
      port: 5432
      database: DataVault
      schema: DATAVAULT
      user: ***user***
      password: ***password***
      threads: 1
      keepalives_idle: 0 # default 0, indicating the system default. See below
      connect_timeout: 10 # default 10 seconds
      retries: 1  # default 1 retry on error/timeout when opening connections
```

you can retrieve the password like this:

``$password = $config.WillibaldChallenge_DataWarehouse.outputs.dev.password``

or with parameters you can retrieve the user like this:

```
$profile ="WillibaldChallenge_DataWarehouse"
$target = "dev"
$user = $config."$profile".outputs."$target".user
```

