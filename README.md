# Mojaloop Control Center (Fork of mojaloop/iac-modules)

This repository is a fork of [mojaloop/iac-modules](https://github.com/mojaloop/iac-modules), customized for specific control center deployments. It provides Kubernetes-based infrastructure automation and configuration management for Mojaloop.

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Maintenance](#maintenance)
- [Scripts](#scripts)
- [License](#license)
- [Differences from Upstream](#differences-from-upstream)

## Overview
This repository contains infrastructure as code (IaC) and deployment automation for setting up and managing Mojaloop control center environments. It supports both AWS and bare-metal deployments using Terraform/Terragrunt and Ansible.

## Architecture
The control center consists of:
- Kubernetes cluster deployment (MicroK8s)
- Configuration management system
- State management utilities
- Environment-specific configurations

Key components:
- `ansible-k8s-deploy/`: Ansible playbooks for Kubernetes deployment
- `k8s-deploy/`: Kubernetes resource definitions
- `default-config/`: Base configuration templates
- `custom-config/`: Environment-specific overrides
- `scripts/`: Utility scripts for configuration merging and setup

## Prerequisites
- Terraform >= 1.2
- Terragrunt
- Ansible
- kubectl
- yq (YAML processor)
- AWS CLI (for AWS deployments)

## Installation
1. Clone this repository:
   ```bash
   git clone https://github.com/mojaloop/mojaloop-control-center-v2.git
   cd mojaloop-control-center-v2
   ```

2. Set up required environment variables:
   ```bash
   export AWS_PROFILE=your_profile
   export PRIVATE_REPO_TOKEN=your_token
   export PRIVATE_REPO_USER=your_username
   ```

3. Initialize Terraform/Terragrunt:
   ```bash
   terragrunt run-all init
   ```

## Configuration
Configuration is managed through YAML files in:
- `default-config/`: Contains base configurations
- `custom-config/`: Contains environment-specific overrides

Key configuration files:
- `cluster-config.yaml`: Main cluster configuration
- `aws-vars.yaml`: AWS-specific variables
- `bare-metal-vars.yaml`: Bare-metal specific variables
- `environment.yaml`: Environment definitions

Configuration merging is handled automatically by the scripts in `scripts/`.

## Usage
### Deploying the Control Center
```bash
./externalrunner.sh
```

### Managing State
- Move state to Kubernetes:
  ```bash
  ./movestatetok8s.sh
  ```

- Move state from Kubernetes:
  ```bash
  ./movestatefromk8s.sh
  ```

### Destroying the Control Center
```bash
./destroy-cc.sh
```

## Maintenance
### Updating Configurations
1. Modify files in `custom-config/` for environment-specific changes
2. Run configuration merge:
   ```bash
   ./scripts/mergeconfigs.sh
   ```

### Upgrading Components
Update version numbers in:
- `default-config/common-vars.yaml`
- `default-config/cluster-config.yaml`

## Scripts
- `externalrunner.sh`: Main deployment script
- `movestate*.sh`: State management utilities
- `destroy-cc.sh`: Cleanup script
- `scripts/dictmerge.py`: Configuration merger
- `scripts/mergeconfigs.sh`: Configuration merge wrapper
- `scripts/setlocalvars.sh`: Environment setup

## License
This project inherits the original license from [mojaloop/iac-modules](https://github.com/mojaloop/iac-modules). Please refer to the original repository for license details.

## Differences from Upstream
This fork includes the following customizations:
- Control center specific configurations
- Additional deployment scripts
- Modified state management utilities
- Custom configuration merging system
