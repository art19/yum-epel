# yum-epel Cookbook

[![Build Status](https://travis-ci.org/chef-cookbooks/yum-epel.svg?branch=master)](http://travis-ci.org/chef-cookbooks/yum-epel) [![Cookbook Version](https://img.shields.io/cookbook/v/yum-epel.svg)](https://supermarket.chef.io/cookbooks/yum-epel)

Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest Group that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux, including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).

The yum-epel cookbook takes over management of the default repositoryids shipped with epel-release. It allows attribute manipulation of `epel`, `epel-debuginfo`, `epel-source`, `epel-testing`, `epel-testing-debuginfo`, and `epel-testing-source`.

## Requirements

### Platforms

- RHEL/CentOS and derivatives

### Chef

- Chef 12.15+

### Cookbooks

- none

## Attributes

The following attributes are set by default

```ruby
default['yum-epel']['repos'] = %w(
  epel
  epel-debuginfo
  epel-source
  epel-testing
  epel-testing-debuginfo
  epel-testing-source
)
```

```ruby
default['yum']['epel']['repositoryid'] = 'epel'
default['yum']['epel']['description'] = 'Extra Packages for Enterprise Linux 7 - $basearch'
default['yum']['epel']['mirrorlist'] = 'http://mirrors.fedoraproject.org/mirrorlist?repo=epel-5&arch=$basearch'
default['yum']['epel']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7'
default['yum']['epel']['failovermethod'] = 'priority'
default['yum']['epel']['gpgcheck'] = true
default['yum']['epel']['enabled'] = true
default['yum']['epel']['managed'] = true
```

```ruby
default['yum']['epel-debuginfo']['repositoryid'] = 'epel-debuginfo'
default['yum']['epel-debuginfo']['description'] = 'Extra Packages for Enterprise Linux 7 - $basearch - Debug'
default['yum']['epel-debuginfo']['mirrorlist'] = 'https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch'
default['yum']['epel-debuginfo']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7'
default['yum']['epel-debuginfo']['failovermethod'] = 'priority'
default['yum']['epel-debuginfo']['gpgcheck'] = true
default['yum']['epel-debuginfo']['enabled'] = false
default['yum']['epel-debuginfo']['managed'] = false
```

```ruby
default['yum']['epel-source']['repositoryid'] = 'epel-source'
default['yum']['epel-source']['description'] = 'Extra Packages for Enterprise Linux 7 - $basearch - Source'
default['yum']['epel-source']['mirrorlist'] = 'http://mirrors.fedoraproject.org/mirrorlist?repo=epel-source-7&arch=$basearch'
default['yum']['epel-source']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7'
default['yum']['epel-source']['failovermethod'] = 'priority'
default['yum']['epel-source']['gpgcheck'] = true
default['yum']['epel-source']['enabled'] = false
default['yum']['epel-source']['managed'] = false
```

```ruby
default['yum']['epel-testing']['repositoryid'] = 'epel-testing'
default['yum']['epel-testing']['description'] = 'Extra Packages for Enterprise Linux 7 - Testing - $basearch'
default['yum']['epel-testing']['mirrorlist'] = 'https://mirrors.fedoraproject.org/metalink?repo=testing-epel7&arch=$basearch'
default['yum']['epel-testing']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7r'
default['yum']['epel-testing']['failovermethod'] = 'priority'
default['yum']['epel-testing']['gpgcheck'] = true
default['yum']['epel-testing']['enabled'] = false
default['yum']['epel-testing']['managed'] = false
```

```ruby
default['yum']['epel-testing-debuginfo']['repositoryid'] = 'epel-testing-debuginfo'
default['yum']['epel-testing-debuginfo']['description'] = 'Extra Packages for Enterprise Linux 7 - Testing - $basearch Debug'
default['yum']['epel-testing-debuginfo']['mirrorlist'] = 'https://mirrors.fedoraproject.org/metalink?repo=testing-debug-epel7&arch=$basearch'
default['yum']['epel-testing-debuginfo']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7'
default['yum']['epel-testing-debuginfo']['failovermethod'] = 'priority'
default['yum']['epel-testing-debuginfo']['gpgcheck'] = true
default['yum']['epel-testing-debuginfo']['enabled'] = false
default['yum']['epel-testing-debuginfo']['managed'] = false
```

```ruby
default['yum']['epel-testing-source']['repositoryid'] = 'epel-testing-source'
default['yum']['epel-testing-source']['description'] = 'Extra Packages for Enterprise Linux 7 - Testing - $basearch Source'
default['yum']['epel-testing-source']['mirrorlist'] = 'https://mirrors.fedoraproject.org/metalink?repo=testing-source-epel7&arch=$basearch'
default['yum']['epel-testing-source']['gpgkey'] = 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7'
default['yum']['epel-testing-source']['failovermethod'] = 'priority'
default['yum']['epel-testing-source']['gpgcheck'] = true
default['yum']['epel-testing-source']['enabled'] = false
default['yum']['epel-testing-source']['managed'] = false
```

## Recipes

- default - Walks through node attributes and feeds a yum_resource
- parameters. The following is an example a resource generated by the
- recipe during compilation.

```ruby
  yum_repository 'epel' do
    mirrorlist 'http://mirrors.fedoraproject.org/mirrorlist?repo=epel-5&arch=$basearch'
    description 'Extra Packages for Enterprise Linux 5 - $basearch'
    enabled true
    gpgcheck true
    gpgkey 'https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL'
  end
```

## Usage Example

To disable the epel repository through a Role or Environment definition

```
default_attributes(
  :yum => {
    :epel => {
      :enabled => {
        false
       }
     }
   }
 )
```

Uncommonly used repositoryids are not managed by default. This is speeds up integration testing pipelines by avoiding yum-cache builds that nobody cares about. To enable the epel-testing repository with a wrapper cookbook, place the following in a recipe:

```ruby
node.default['yum']['epel-testing']['enabled'] = true
node.default['yum']['epel-testing']['managed'] = true
include_recipe 'yum-epel'
```

## More Examples

Point the epel repositories at an internally hosted server.

```ruby
node.default['yum']['epel']['enabled'] = true
node.default['yum']['epel']['mirrorlist'] = nil
node.default['yum']['epel']['baseurl'] = 'https://internal.example.com/centos/7/os/x86_64'
node.default['yum']['epel']['sslverify'] = false

include_recipe 'yum-epel'
```

## License & Authors

**Author:** Cookbook Engineering Team ([cookbooks@chef.io](mailto:cookbooks@chef.io))

**Copyright:** 2011-2016, Chef Software, Inc.

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
