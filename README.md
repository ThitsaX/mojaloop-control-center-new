# Mojaloop Control Center (Derived from mojaloop/iac-modules)

This repository is a customized version of [mojaloop/iac-modules](https://github.com/mojaloop/iac-modules), tailored for specific Mojaloop control center implementations. It offers Kubernetes-driven infrastructure automation and configuration management.

## Table of Contents
- [Introduction](#introduction)
- [Structure](#structure)
- [Requirements](#requirements)
- [Setup](#setup)
- [Customization](#customization)
- [Operation](#operation)
- [Upkeep](#upkeep)
- [Utilities](#utilities)
- [License](#license)
- [Changes from Original](#changes-from-original)

## Introduction
This repository provides infrastructure as code (IaC) and automated deployment tools for establishing and managing Mojaloop control center setups. It accommodates both AWS and bare-metal environments using Terraform/Terragrunt and Ansible.

## Structure
The control center is composed of:
- Kubernetes cluster setup (EKS or Microk8s)
- Configuration management framework
- State handling tools
- Environment-tailored settings

Core elements:
- `ansible-k8s-deploy/`: Ansible scripts for Kubernetes setup
- `k8s-deploy/`: Kubernetes resource specifications
- `default-config/`: Standard configuration templates
- `custom-config/`: Environment-specific adjustments
- `scripts/`: Helper scripts for merging configurations and initialization

## Requirements
- Terraform >= 1.2
- Terragrunt
- Ansible
- kubectl
- yq (YAML processor)
- AWS CLI (for AWS setups)

## Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/mojaloop/mojaloop-control-center-new.git
   cd mojaloop-control-center-new
   ```

2. Define necessary environment variables:
   ```bash
   source ~/.venv/bin/activate #activate python virtual environment for ansible

   #Configure externalrunner.sh
   export AWS_PROFILE=yourprofilename
   export PRIVATE_REPO_TOKEN=nullvalue
   export PRIVATE_REPO_USER=nullvalue
   export ANSIBLE_BASE_OUTPUT_DIR=/tmp/output
   export PRIVATE_REPO=example.com

   #Adjust custom-config/cluster-config.yaml as needed
   
   source externalrunner.sh
   source ./scripts/setlocalvars.sh
   ./scripts/mergeconfigs.sh 
   ```
## Customization
Settings are managed via YAML files in:
- `default-config/`: Base settings
- `custom-config/`: Environment-specific modifications

Primary configuration files:
- `cluster-config.yaml`: Core cluster settings
- `aws-vars.yaml`: AWS-specific parameters
- `bare-metal-vars.yaml`: Bare-metal specific parameters
- `environment.yaml`: Environment specifications

The `scripts/` directory handles automatic configuration merging.

## Operation
### Launching the Control Center Deployment
```bash
./wrapper.sh
```

### State Management
- Transfer state to Kubernetes:
  ```bash
  ./movestatetok8s.sh
  ```

- Retrieve state from Kubernetes:
  ```bash
  ./movestatefromk8s.sh
  ```

### Removing the Control Center
```bash
./destroy-cc.sh
```

## Upkeep
### Modifying Configurations
1. Edit files in `custom-config/` for environment-specific updates
2. Execute configuration merge:
   ```bash
   ./scripts/mergeconfigs.sh
   ```

### Updating Components
Adjust version numbers in:
- `custom-config/common-vars.yaml`
- `custom-config/cluster-config.yaml`

## Utilities
- `externalrunner.sh`: initializes the environment by setting key variables
- `wrapper.sh`: Orchestrates the deployment process with a locking mechanism to prevent concurrent runs. It creates a lock file (/tmp/myscript.lock)
- `movestate*.sh`: State management scripts
- `destroy-cc.sh`: Removal script
- `scripts/dictmerge.py`: Configuration merging tool
- `scripts/mergeconfigs.sh`: Configuration merge orchestrator
- `scripts/setlocalvars.sh`: Environment variable initializer

## License
This project adopts the license from [mojaloop/iac-modules](https://github.com/mojaloop/iac-modules). Refer to the original repository for licensing information.

## Changes from Original
This version includes the following adaptations:
- Control center-specific settings
- Extra deployment utilities
- Revised state management tools
- Enhanced configuration merging mechanism