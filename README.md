# ad_inventory

Welcome to your new module. A short overview of the generated parts can be found in the PDK documentation at https://puppet.com/pdk/latest/pdk_generating_modules.html .

The README template below provides a starting point with details about what information to include in your README.

#### Table of Contents

1. [Description](#description)
2. [Setup - The basics of getting started with ad_inventory](#setup)
    * [What ad_inventory affects](#what-ad_inventory-affects)
    * [Setup requirements](#setup-requirements)
    * [Beginning with ad_inventory](#beginning-with-ad_inventory)
3. [Usage - Configuration options and additional functionality](#usage)
4. [Limitations - OS compatibility, etc.](#limitations)
5. [Development - Guide for contributing to the module](#development)

## Description

Briefly tell users why they might want to use your module. Explain what your module does and what kind of problems users can solve with it.

This should be a fairly short description helps the user decide if your module is what they want.

## Setup

Install net-ldap gem into Bolt

``` text
> "C:/Program Files/Puppet Labs/Bolt/bin/gem.bat" install --user-install net-ldap
```

### Development

``` text
> bundle exec rake spec_prep

...

> bundle exec bolt command run "Write-host 'Hello'" --modulepath spec/fixtures/modules  --inventoryfile example_inventory.yaml
```

Example Inventory
``` yaml
# inventory.yaml
version: 2
groups:
  - name: all_bolt_domain
    targets:
      - _plugin: ad_inventory
        ad_domain: 'bolt.local'
        domain_controller: '192.168.200.200'
        user: 'BOLT\\Administrator'
        password: 'Password1'
```

### Example: Filter out computers older than given number of days

Sometimes in Active Directory land, computer objects are not removed. This plugin
allows you to filter out objects older than a given number of days. 
To do this there are two options available on the plugin:
  * `filter_older_attribute` : This is the name of the LDAP attribute that contains a LDAP timestamp value that we'll use for filtering. Some good options here are `'pwdLastSet'` and `'lastLogonTimestamp'`.
  * `filter_older_than_days` : Number of days (integer) that will be used as the cut-off point. If an object is older than this many days it will be filtered out of the results.

``` yaml
# inventory.yaml
version: 2
groups:
  - name: all_bolt_domain
    targets:
      - _plugin: ad_inventory
        ad_domain: 'bolt.local'
        domain_controller: '192.168.200.200'
        user: 'BOLT\\Administrator'
        password: 'Password1'
        filter_older_attribute: 'pwdLastSet'
        filter_older_than_days: 30
```

### Example: Ignore computers by DNS hostname

It's common that a computer may be returned from AD, but you have problems connecting to it.
We provide the ability to filter out hosts, by name, coming form this plugin so you can get your work done.
To accomplish this, we've provided the option `ignore_dns_hostnames`. Simply pass in an array 
of hostnames into this option, and any host matching that name will be excluded.

``` yaml
# inventory.yaml
version: 2
groups:
  - name: all_bolt_domain
    targets:
      - _plugin: ad_inventory
        ad_domain: 'bolt.local'
        domain_controller: '192.168.200.200'
        user: 'BOLT\\Administrator'
        password: 'Password1'
        ignore_dns_hostnames:
          - mybadwinrm.bolt.local
          - mybaddc01.bolt.local
          - sccm03.bolt.local
```

### Example: Only return members of a given group

It's common for AD admins to use groups for targetting. We provide the ability to 
only return computers/hosts that are members of a specified group using the
`member_of_group_dn` parameter. When passing a group to this parameter, you'll need to
pass the full Distinguished Named (`dn`) of the group (example: `CN=Patching Group,OU=GPO Groups,OU=Groups,DC=domain,DC=tld,DC=tech`).

``` yaml
# inventory.yaml
version: 2
groups:
  - name: patching_group_bolt_domain
    targets:
      - _plugin: ad_inventory
        ad_domain: 'bolt.local'
        domain_controller: '192.168.200.200'
        user: 'BOLT\\Administrator'
        password: 'Password1'
        member_of_group_dn: 'CN=Patching Group,OU=GPO Groups,OU=Groups,DC=domain,DC=tld,DC=tech'
```

### What ad_inventory affects **OPTIONAL**

If it's obvious what your module touches, you can skip this section. For example, folks can probably figure out that your mysql_instance module affects their MySQL instances.

If there's more that they should know about, though, this is the place to mention:

* Files, packages, services, or operations that the module will alter, impact, or execute.
* Dependencies that your module automatically installs.
* Warnings or other important notices.

### Setup Requirements **OPTIONAL**

If your module requires anything extra before setting up (pluginsync enabled, another module, etc.), mention it here.

If your most recent release breaks compatibility or requires particular steps for upgrading, you might want to include an additional "Upgrading" section here.

### Beginning with ad_inventory

The very basic steps needed for a user to get the module up and running. This can include setup steps, if necessary, or it can be an example of the most basic use of the module.

## Usage

Include usage examples for common use cases in the **Usage** section. Show your users how to use your module to solve problems, and be sure to include code examples. Include three to five examples of the most important or common tasks a user can accomplish with your module. Show users how to accomplish more complex tasks that involve different types, classes, and functions working in tandem.

## Reference

This section is deprecated. Instead, add reference information to your code as Puppet Strings comments, and then use Strings to generate a REFERENCE.md in your module. For details on how to add code comments and generate documentation with Strings, see the Puppet Strings [documentation](https://puppet.com/docs/puppet/latest/puppet_strings.html) and [style guide](https://puppet.com/docs/puppet/latest/puppet_strings_style.html)

If you aren't ready to use Strings yet, manually create a REFERENCE.md in the root of your module directory and list out each of your module's classes, defined types, facts, functions, Puppet tasks, task plans, and resource types and providers, along with the parameters for each.

For each element (class, defined type, function, and so on), list:

  * The data type, if applicable.
  * A description of what the element does.
  * Valid values, if the data type doesn't make it obvious.
  * Default value, if any.

For example:

```
### `pet::cat`

#### Parameters

##### `meow`

Enables vocalization in your cat. Valid options: 'string'.

Default: 'medium-loud'.
```

## Limitations

In the Limitations section, list any incompatibilities, known issues, or other warnings.

## Development

In the Development section, tell other users the ground rules for contributing to your project and how they should submit their work.

## Release Notes/Contributors/Etc. **Optional**

If you aren't using changelog, put your release notes here (though you should consider using changelog). You can also add any additional sections you feel are necessary or important to include here. Please use the `## ` header.
