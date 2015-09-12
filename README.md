# Bosh release for Minecraft Server

One of the fastest ways to get a [Minecraft](https://minecraft.net/) and/or a [Minecraft Pocket Edition](https://www.pocketmine.net/) server running on any infrastructure is to deploy this BOSH release.

This BOSH release supports the following [Minecraft versions](http://minecraft.gamepedia.com/Version_history):
* 1.7.2
* 1.7.4
* 1.7.5
* 1.7.9
* 1.7.10
* 1.8
* 1.8.1
* 1.8.2
* 1.8.3
* 1.8.4
* 1.8.5
* 1.8.6
* 1.8.7
* 1.8.8

This BOSH release supports the following Minecraft Pocket Edition versions:
* v0.12.1 alpha

## Usage

### Upload the BOSH release

To use this BOSH release, first upload it to your BOSH:

```
bosh target BOSH_HOST
git clone https://github.com/frodenas/minecraft-boshrelease.git
cd prometheus-boshrelease
bosh upload release releases/minecraft/minecraft-1.yml
```

### Create a BOSH deployment manifest

Now create a deployment file (using the files at the [examples](https://github.com/frodenas/minecraft-boshrelease/blob/master/examples/) directory as a starting point).

Note that the examples requires you to open some ports, so you will need to:

Create a security group (or the equivalent) named `bosh` with the following ports opened:
* TCP 22, 4222, 6868, 25250, 25555, 25777 from 0.0.0.0 to enable BOSH to communicate with the agents
* UDP 53 to enable using the BOSH DNS

Create a security group (or the equivalent) named `minecraft` (or the name of your deployment) with the following ports opened:
* TCP 25565 from 0.0.0.0 to enable access to the Minecraft server
* TCP/UDP 19132 from 0.0.0.0 to enable access to the Minecraft Pocker Edition server

### Deploy using the BOSH deployment manifest

Using the previous created deployment manifest, now we can deploy it:

```
bosh deployment path/to/deployment.yml
bosh -n deploy
```

## Contributing

In the spirit of [free software](http://www.fsf.org/licensing/essays/free-sw.html), **everyone** is encouraged to help improve this project.

Here are some ways *you* can contribute:

* by using alpha, beta, and prerelease versions
* by reporting bugs
* by suggesting new features
* by writing or editing documentation
* by writing specifications
* by writing code (**no patch is too small**: fix typos, add comments, clean up inconsistent whitespace)
* by refactoring code
* by closing [issues](https://github.com/frodenas/minecraft-boshrelease/issues)
* by reviewing patches

### Submitting an Issue
We use the [GitHub issue tracker](https://github.com/frodenas/minecraft-boshrelease/issues) to track bugs and features.
Before submitting a bug report or feature request, check to make sure it hasn't already been submitted. You can indicate
support for an existing issue by voting it up. When submitting a bug report, please include a
[Gist](http://gist.github.com/) that includes a stack trace and any details that may be necessary to reproduce the bug,
including your gem version, Ruby version, and operating system. Ideally, a bug report should include a pull request with
 failing specs.

### Submitting a Pull Request

1. Fork the project.
2. Create a topic branch.
3. Implement your feature or bug fix.
4. Commit and push your changes.
5. Submit a pull request.

### Create new release

#### Creating a final release

If you need to create a new final release, you will need to get read/write API credentials to the [@cloudfoundry-community](https://github.com/cloudfoundry-community) s3 account.

Please email [Dr Nic Williams](mailto:&#x64;&#x72;&#x6E;&#x69;&#x63;&#x77;&#x69;&#x6C;&#x6C;&#x69;&#x61;&#x6D;&#x73;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;) and he will create unique API credentials for you.

Create a `config/private.yml` file with the following contents:

``` yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
```

You can now create final releases for everyone to enjoy!

```
bosh create release
# test this dev release
git commit -m "updated minecraft"
bosh create release --final
git commit -m "creating vXYZ release"
git tag vXYZ
git push origin master --tags
```

## Copyright

Copyright (c) 2015 Ferran Rodenas. See [LICENSE](https://github.com/frodenas/minecraft-boshrelease/blob/master/LICENSE) for details.
