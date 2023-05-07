# Ansible — Dynamic DNS using Njalla

This playbook creates a bash script from the following fork. The script sends the public IP of an instance on Oracle Cloud to [Njalla](https://njal.la/). A [Cron](https://en.wikipedia.org/wiki/Cron) job runs the script once every hour.
* GitHub: [github.com/k3karthic/bash-updater](https://github.com/k3karthic/bash-updater)
* Codeberg: [codeberg.org/k3karthic/bash-updater](https://codeberg.org/k3karthic/bash-updater)

**Assumption:** Instance deployed using either one of the Terraform scripts below,
* terraform__oci-instance-1
	* GitHub: [github.com/k3karthic/terraform__oci-instance-1](https://github.com/k3karthic/terraform__oci-instance-1)
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-instance-1](https://codeberg.org/k3karthic/terraform__oci-instance-1)
* terraform__oci-instance-2
	* GitHub: [github.com/k3karthic/terraform__oci-instance-2](https://github.com/k3karthic/terraform__oci-instance-2)
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-instance-2](https://codeberg.org/k3karthic/terraform__oci-instance-2)

## Code Mirrors

* GitHub: [github.com/k3karthic/ansible__oci-njalla-dns](https://github.com/k3karthic/ansible__oci-njalla-dns/)
* Codeberg: [codeberg.org/k3karthic/ansible__oci-njalla-dns](https://codeberg.org/k3karthic/ansible__oci-njalla-dns)

## Requirements

Install the following before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Dynamic Inventory

The Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) populates public Ubuntu instances.

All target Ubuntu instances must have the freeform tags `njalla_domain: <domain name>` and `njalla_domain_id: <record id>`.

## Configuration

1. Update `inventory/oracle.oci.yml`,
    1. Specify the region where you have deployed your server on Oracle Cloud. List of regions are at [docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm](https://docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm).
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File)
1. Set username and ssh authentication in `inventory/group_vars/`
2. Set [API Token](https://njal.la/settings/api/) for Njalla in `inventory/group_vars/njalla.yml`. Use `inventory/group_vars/njalla.yml.sample` as a reference.

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private keys) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```
