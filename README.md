# Docker images in ansible playbook or requirements.yaml skipped 

I'm trying to onboard a repository with an ansible playbook which contains some docker images. However renovate just skipped it. I also added some fileMatch and matchStrings, but it still was skipped. I also tried to outsource my docker images in requirements.yaml and import them into the playbook, but it still was skipped.

The playbook is located in a tasks folder, so the path to the ansible playbook would be tasks/ansible.yaml for the version where the docker images are in the playbook and tasks/ansible_req.yaml where the docker images are in the requirements.yaml and they are imported in the tasks/ansible_req.yaml file.

## renovate config


```
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Renovate preset to update gitlab remote include for renovate templates",
  "customManagers": [
    {
      "customType": "regex",
      "description": "Update renovate remote includes version",
      "fileMatch": ["\\.gitlab-ci\\.ya?ml$"],
      "matchStrings": [
        "-\\s+remote:\\s+https://gitlab\\.com/renovate-bot/renovate-runner/-/raw/(?<currentValue>v[0-9.]+)/"
      ],
      "datasourceTemplate": "gitlab-releases",
      "depNameTemplate": "renovate-runner",
      "packageNameTemplate": "renovate-bot/renovate-runner",
      "versioningTemplate": "semver"
    }
  ],
  "ansible": {
    "packageRules": [
      {
        "matchDatasources": ["docker"],
        "enabled": false
      },
      {
        "matchDatasources": ["docker"],
        "matchPackageNames": ["ethereum/client-go", "sigp/lighthouse", "flashbots/mev-boost"],
        "versioning": "semver",
        "enabled": true
      }
    ],
    "regexManagers": [
        {
          "description": "Update Docker images in Ansible playbook",
          "fileMatch":["^tasks/.*\\.ya?ml$"],
          "matchStrings": [
            "\\s*geth_container_image:\\s+?(:?[\\\/a-z\\.-]+\\\/)*(?<depName>[\\\/a-z-]+)(?::(?<currentValue>[a-z0-9.-]+))?",
            "\\s*lighthouse_container_image:\\s+?(:?[\\\/a-z\\.-]+\\\/)*(?<depName>[\\\/a-z-]+)(?::(?<currentValue>[a-z0-9.-]+))?",
            "\\s*mev_boost_container_image:\\s+?(:?[\\\/a-z\\.-]+\\\/)*(?<depName>[\\\/a-z-]+)(?::(?<currentValue>[a-z0-9.-]+))?"
          ],
        "datasourceTemplate": "docker"
        }
    ]
  }
}

```