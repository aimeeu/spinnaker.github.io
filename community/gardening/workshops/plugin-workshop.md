---
layout: single
title:  "Plugin Training Workshop"
sidebar:
  nav: community
---



This page contains notes for the _Plugins Training Workshop_ session at the July Gardening Days. Learn plugin concepts, the PF4J plugin framework, plugin development, and plugin deployment on Spinnaker 1.21.

Watch the workshop [recording](https://youtu.be/oEHPvO88ROA) from July 18, 2020.
<iframe width="560" height="315" src="https://www.youtube.com/embed/oEHPvO88ROA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

{% include toc %}

## Resources

* [Plugin Creators Guide Overview]
* [Test a Pipeline Stage Plugin]
* [Plugin Users Guide]
* [pf4jStagePlugin Deployment Example](https://www.spinnaker.io/guides/user/plugins/deploy-example/)
* [pf4jStagePlugin repo]
* [Orca repo]
* [Deck repo]

## Prerequisites

Completion of the [New Spinnaker Contribution Walkthrough Workshop]

**\*\* OR \*\***

* Basic knowledge of how to contribute to Spinnaker
* Spinnaker development environment configured on your workstation; you should be able to connect to a running Spinnaker instance that you installed using Halyard or the open source [Spinnaker Operator for Kubernetes](https://github.com/armory/spinnaker-operator/)

## Agenda

* Overview of Plugins

  * Plugins Concepts
  * Ways of Extending Spinnaker with the Plugin Framework
  * Backend Development
  * Frontend Development
  * Build
  * Deploy

* Workshop

  * Add a plugin to Spinnaker
  * Clone plugin repo and build a release bundle
  * Debug into plugin code via Orca
  * Use Telepresence to connect Orca to Spinnaker

## Overview of Plugins

### Plugin Concepts

See the [Plugin Creators Guide Overview] for more details.

* [PF4J](https://github.com/pf4j/pf4j) is the plugin framework
* An [ExtensionPoint](https://spinnaker.io/guides/developer/plugin-creators/overview/#finding-an-extension-point) is a way to hook into Spinnaker itself
* An [Extension](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/blob/master/random-wait-orca/src/main/kotlin/io/armory/plugin/stage/wait/random/RandomWaitPlugin.kt#L31) implements an ExtensionPoint; this code is in the plugin
* The [pf4jStagePlugin](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin) (RandomWait Plugin) implements a single extension point

Plugin developers should understand Spinnaker [architecture](/reference/architecture/).

### Ways of Extending Spinnaker with the Plugin Framework

You can also extend Spinnaker with a plugin that does not use an ExtensionPoint:

* Implement an Interface (non-ExtensionPoint)

  * [Details](/guides/developer/plugin-creators/overview#interface-non-extensionpoint-plugins)
  * Example [plugin](https://github.com/spinnaker-plugin-examples/pf4jPluginWithoutExtensionPoint)

* Spring-based plugins

  * [Details](/guides/developer/plugin-creators/overview/#spring-plugins)
  * Example [plugin](https://github.com/spinnaker-plugin-examples/awsCredOverrides)
  * _not recommended due to development complexity_

### Backend Development

`SimpleStage` extension point plugin:

* The pf4jStagePlugin implements the [SimpleStage.java](https://github.com/spinnaker/orca/blob/master/orca-api/src/main/java/com/netflix/spinnaker/orca/api/simplestage/SimpleStage.java) extension point
* [RandomWaitPlugin.kt](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/blob/203408aa0dcc7585e080d4a87dab4b94544be6ab/random-wait-orca/src/main/kotlin/io/armory/plugin/stage/wait/random/RandomWaitPlugin.kt)

See the [pf4jStagePlugin repo]'s README for architecture details.

_Note: SimpleStage will be moved or deprecated in the future._

#### Debugging a backend plugin

See the [New Spinnaker Contribution Walkthrough Workshop] and [Test a Pipeline Stage Plugin] docs for instructions on how to debug a Spinnaker service and a plugin.

### Frontend Development

* [rollup.config.js](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/blob/master/random-wait-deck/rollup.config.js)
* [package.json](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/blob/master/random-wait-deck/package.json)
* [RandomWaitStage.tsx](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/blob/master/random-wait-deck/src/RandomWaitStage.tsx)

You can generate a Deck plugin component skeleton by executing:

```bash
npx -p @spinnaker/pluginsdk scaffold
```

### Build

[Project structure](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin)

Component names should contain the name of the Spinnaker service the plugin extends: <plugin-name>-<service>

* `gradle.properties`
* `settings.gradle`
* `build.gradle`
* `orca.gradle`
* GitHub actions: the `pf4jStagePlugin` repo has GitHub [actions](https://github.com/spinnaker-plugin-examples/pf4jStagePlugin/actions) that keep libraries updated and build releases
* Release bundle


### Deploy

Resources: [Plugin Users Guide](https://www.spinnaker.io/guides/user/plugins/) explains the files and configuration you need to deploy a plugin using Halyard. The [pf4jStagePluginDeployment Example](https://www.spinnaker.io/guides/user/plugins/deploy-example/) walks you through deploying the `pf4jStagePlugin`.

* [plugins.json](https://github.com/spinnaker-plugin-examples/examplePluginRepository/blob/master/plugins.json)
* [repositories.json](https://github.com/spinnaker-plugin-examples/examplePluginRepository/blob/master/repositories.json)
* Halyard (to be replaced by Kleat)
* Spinnaker Operator for Kubernetes
* [Backend services (PF4J Update)](https://github.com/pf4j/pf4j-update)
* Deck

  * CORS
  * Serve via Gate


## Workshop

### Add a plugin to Spinnaker

### Clone plugin repo and build a release bundle

### Debug into plugin code via Orca

### Use Telepresence to connect Orca to Spinnaker




[New Spinnaker Contribution Walkthrough Workshop]: (/community/gardening/workshops/spin-contrib/)
[Test a Pipeline Stage Plugin]: (/guides/developer/plugin-creators/deck-plugin/)
[Plugin Creators Guide Overview]: (/guides/developer/plugin-creators/overview/)
[Plugin Users Guide]: (https://www.spinnaker.io/guides/user/plugins/)
[pf4jStagePlugin repo]: (https://github.com/spinnaker-plugin-examples/pf4jStagePlugin)
[Orca repo]: (https://github.com/spinnaker/orca/)
[Deck repo]: (https://github.com/spinnaker/deck/)