# Docker images in ansible playbook or requirements.yaml skipped 

I'm trying to onboard a repository with an ansible playbook which contains some docker images. However renovate just skipped it. I also added some fileMatch and matchStrings, but it still was skipped. I also tried to outsource my docker images in requirements.yaml and import them into the playbook, but it still was skipped.

The playbook is located in a tasks folder, so the path to the ansible playbook would be tasks/ansible.yaml for the version where the docker images are in the playbook and tasks/ansible_req.yaml where the docker images are in the requirements.yaml and they are imported in the tasks/ansible_req.yaml file.

## renovate config


```
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:recommended"
  ],
  "customManagers": [
    {
      "customType": "regex",
      "fileMatch": ["^defaults/vars\\.yml$"],
      "matchStrings": [
        "\"(?<depName>docker.io/ethereum/client-go):(?<currentValue>v[0-9.]+)\"",
        "\"(?<depName>docker.io/sigp/lighthouse):(?<currentValue>v[0-9.]+)\"",
        "\"(?<depName>docker.io/flashbots/mev-boost):(?<currentValue>[0-9.]+)\""
      ],
      "datasourceTemplate": "docker"
    }
  ]
}
```