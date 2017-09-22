# BOSH Release for credhub-importer

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/credhub-importer-boshrelease.git
cd credhub-importer-boshrelease
bosh upload-release releases/credhub-importer/credhub-importer-1.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster. Note that this requires that you have installed [spruce](https://github.com/geofffranks/spruce).

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

``` yaml
---
networks:
  - name: credhub-importer1
    type: dynamic
    cloud_properties:
      security_groups:
        - credhub-importer
```

Where `- credhub-importer` means you wish to use an existing security group called `credhub-importer`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```

### Development

As a developer of this release, create new releases and upload them:

```
bosh create release --force && bosh -n upload release
```

### Final releases

To share final releases:

```
bosh create release --final
```

By default the version number will be bumped to the next major number. You can specify alternate versions:


```
bosh create release --final --version 2.1
```

After the first release you need to contact [Dmitriy Kalinin](mailto://dkalinin@pivotal.io) to request your project is added to https://bosh.io/releases (as mentioned in README above).
