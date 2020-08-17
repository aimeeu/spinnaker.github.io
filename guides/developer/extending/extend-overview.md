---
layout: single
title:  "When and How to Extend Spinnaker"
sidebar:
  nav: guides
---

{% include toc %}

“What Spinnaker does is cool. My question is, can it do even **more** for me?”

We’ve categorized the options here for extending or customizing Spinnaker into three levels: Lightweight, Middleweight, and Heavyweight options. Which option you choose depends on how extensive the change is that you want to make and whether you want to reflect this change for all your Spinnaker users or on a more ad-hoc basis.

## Lightweight Options:

These are the “quick-and-easy” ways to extend Spinnaker that don’t typically require a lot of effort to get working. They typically depend on external systems and might be too limited in scope for some use-cases you’re considering, in which case you would want to take a look at the Middleweight or Heavyweight options further below.

| Extension type                                              | How it works                                                                                                                                                                                              | What you need to use this extension                                                                                                                                      | Example use-cases                                                                                                                                                                                        | Further reading                                                    |
| ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Script Stage                                                | Run a script in Jenkins                                                                                                                                                                                   | Requires Jenkins to run the scripts; this stage just passes a command to Jenkins to run the target script                                                                | - you have scripts setup in Jenkins that you’d like to quickly and easily trigger from Spinnaker                                                                                                         | https://www.spinnaker.io/reference/pipeline/stages/#script         |
| Run <CI> Job                                                | There are Run Job jobs intended specifically for Jenkins, ConcourseCI, and Travis CI.                                                                                                                     | Requires these external CI tools to be setup with jobs to be run by Spinnaker.                                                                                           | - you have some pre-existing automation in your preferred CI tool that you’d like to incorporate into your pipelines                                                                                     | https://www.spinnaker.io/reference/pipeline/stages/#jenkins        |
| Run <build> Job                                             | Trigger a build job in AWS CodeBuild, Google Cloud Build, or Wercker                                                                                                                                      | Requires these external tools to be setup with desired build jobs                                                                                                        | - you want a build process to be part of your pipeline                                                                                                                                                   | https://spinnaker.io/reference/pipeline/stages/#google-cloud-build |
| Run Job Stage                                               | Run a Docker container                                                                                                                                                                                    | An accessible Docker registry that supplies your Docker images                                                                                                           | - it’s easiest for you to just add new services or integrations via separate container images (you prefer to use your own tech stack / languages) and you just want Spinnaker to help with orchestration | https://www.spinnaker.io/reference/pipeline/stages/#run-job        |
| Custom Run jobs<br>(also known as “preconfigured run jobs”) | Add a new custom Run Job stage via a K8s job that you configure on Orca (this differs from the Run Job above in that this adds a stage that can be reused by all pipelines for your Spinnaker deployment) |                                                                                                                                                                          |                                                                                                                                                                                                          | https://www.spinnaker.io/guides/operator/custom-job-stages/        |
| Webhook Stages                                              | Send API requests in a pipeline stage                                                                                                                                                                     | Configure the stage to send desired requests to an external API; you can also create a standard webhook stage for your deployment that can be re-used in other pipelines |                                                                                                                                                                                                          |                                                                    |
| Custom Webhooks                                             | (this differs from the Webhook above in that this adds a stage that can be reused by all pipelines for your Spinnaker deployment)                                                                         |                                                                                                                                                                          |                                                                                                                                                                                                          | https://www.spinnaker.io/guides/operator/custom-webhook-stages/    |
| Pipeline Triggers                                           | Setup your pipelines to be triggered by pub/sub messages, webhooks, artifacts, or Jenkins.                                                                                                                |                                                                                                                                                                          |                                                                                                                                                                                                          |                                                                    |
| Spinnaker API                                               | Various functions in Spinnaker’s UI can also be accessed via a set of API endpoints                                                                                                                       | The GET methods cover almost all the information you can find in the UI, whereas POST/PUT/DELETE methods are more limited to Pipelines, Tasks, and Applications.         |                                                                                                                                                                                                          | https://www.spinnaker.io/reference/api/docs.html                   |
| Echo endpoints                                              |                                                                                                                                                                                                           |                                                                                                                                                                          |                                                                                                                                                                                                          |                                                                    |

## Middleweight Options (Plugins):

Plugins are the best option if your desire is to tailor some functionality in Spinnaker to your specific needs or add some new functionality that doesn’t yet exist, but you also don’t want to dive too deep into the Spinnaker project. Plugins give you the benefit of being able to isolate your own customizations from the Spinnaker codebase, but still modify almost anything you want in Spinnaker.

Depending on whether your plugin modifies the frontend (Deck service) or the backend (all the other Spinnaker services), you will have different options. We’ve described those options here for both in two separate tables. Note that a single plugin can package and deliver both frontend and backend components; there’s just a mix of different content (React/Typescript vs. Java/Kotlin, typically).

### Backend Options for Plugins:
| Extension type   | What it does                                                                                                                                                                                         | What you need to use this extension                                                                                                                                                     | Example use-cases                                                              | Further reading |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | --------------- |
| Extension points | Some Spinnaker services have defined extension points for which code can be written to modify their functionality, without requiring core Spinnaker source code modification or shared dependencies. | It helps to have a little familiarity with Java/Kotlin. You can build a plugin which is deployed on the Spinnaker services you want to modify.                                          | - https://github.com/spinnaker-plugin-examples/pf4jStagePlugin                 |                 |
| Plain interfaces | Use a plugin to implement Spinnaker interfaces that aren’t extension points (yet).                                                                                                                   | It helps to have a little familiarity with Java/Kotlin and with the Spinnaker interface objects. You can build a plugin which is deployed on the Spinnaker services you want to modify. | - https://github.com/spinnaker-plugin-examples/pf4jPluginWithoutExtensionPoint |                 |
| Spring           | Make any modifications you’d like via Spring                                                                                                                                                         | You will need to be comfortable coding in Spring Java to write this type of plugin, and have some familiarity with Spinnaker.                                                           | - https://github.com/spinnaker-plugin-examples/springExamplePlugin             |                 |

### Frontend Options for Plugins:
| Extension type                                                       | What it does                                                                                                                                                            | What you need to use this extension                                                                                | Some example use-cases | Further reading                                                              |
| -------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ---------------------- | ---------------------------------------------------------------------------- |
| Updating or adding to Deck registries                                | There are a few registries in Deck, like PipelineRegistry, CloudProviderRegistry, and others which provide entry points for plugins to add more options in these areas. | It helps to have a little experience with React/Typescript. You can register your new modules in these registries. |                        |                                                                              |
| Extending the interfacable objects in Deck?                          |                                                                                                                                                                         |                                                                                                                    |                        |                                                                              |
| Overridable React components **(this will be usable in the future)** |                                                                                                                                                                         |                                                                                                                    |                        | https://github.com/spinnaker-plugin-examples/pf4jPluginWithoutExtensionPoint |

Fig 1: Anatomy of plugins - this depicts a simple plugin (an event listener) that modifies just one Spinnaker service and another plugin which modifies multiple services (database snapshotter):

![](https://paper-attachments.dropbox.com/s_C11CEAFB54BA7E52B30A6280BA95EF599FDE2844CE074182C1E46D7E7A51512C_1589484590801_Screen+Shot+2020-05-14+at+12.15.05+PM.png)

## Heavyweight Options:

If you don’t see an option that works for you above, and you want to customize Spinnaker in a way that cannot be done with modifications to just a few services or requires non-overridable code to be modified, then you may require one of these options.

| Extension type                    | What it does                                                                            | What you need to use this extension                                                                                                                                                                                                                                                            | Further reading                                                           |
| --------------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| (Classpath injection?) Extensions | Adds on new functionality on top of the OSS Spinnaker services as additional libraries. | This is how several long-time contributors to the Spinnaker project have made modifications to Spinnaker. This requires pretty deep knowledge of the Spinnaker services you want to extend, and expertise in Spring Java. You also need a custom release process for Spinnaker to enable this. | https://blog.spinnaker.io/how-netflix-has-extended-spinnaker-baf1a9d6b6e3 |
| Forking Spinnaker                 | The “last resort” option, if you really want to give life to your own creation 👾       | As with forks of any major project, if you plan on using this longterm, be prepared to invest heavily in maintaining this.                                                                                                                                                                     |                                                                           |

|               | Personas who would be motivated to build an extension of this | Skills/steps required to create an extension of this                      | Examples   | Spinnaker “sweet spot”? Strategic advantage? | Number of different potential extensions | Integration or new value add |
| ------------- | ------------------------------------------------------------- | ------------------------------------------------------------------------- | ---------- | -------------------------------------------- | ---------------------------------------- | ---------------------------- |
| EventListener | Platform / DevOps                                             | - Clone the example plugin<br>- Update ~5 lines of Kotlin, + name replace | - NewRelic |                                              |                                          |                              |
|               |                                                               |                                                                           |            |                                              |                                          |                              |

## Here’s another way to categorize these extensions:

Here’s my initial idea about how we can garner interest in A) what’s been accomplished thus far with Spinnaker extensibility overall and B) what’s potentially coming next.

The plugin framework has recently become available for Spinnaker developers to use to extend Spinnaker to create integrations with other services or add new services. And plugins are just one of many different ways now that one can extend Spinnaker. If we consider what extension features were available in the 2018/early-2019 timeframe vs today, I can categorize those as “Extensibility 1.0” and “Extensibility 2.0”, and also talk about what we hope to deliver in 2021+ as part of “Extensibility 3.0”:

Extensibility 1.0:
“Low-touch” extensibility options (require little to no coding with required knowledge of Spinnaker code):

- script stages
- run jobs
- webhooks
- Jenkins stages
- echo REST endpoint configuration
- pipeline triggers
- SpEL expressions
- Spinnaker API

“Deeply integrated” extensibility options (require more knowledge of Spinnaker codebase):

- Classpath injection extensions (essentially what Armory features were made of, before plugins existed)
- Making OSS contributions
- Forking Spinnaker services

Extensibility 2.0:


## And… Here’s another way to categorize these extensions:

“Automate more”

- custom stages
- pipeline triggers
- run jobs
- webhooks

“See and measure more”

- echo endpoints / eventListener plugins
- Spinnaker API
- Observability plugin

“More (multi)-cloud with less maintenance overhead”

- cloud provider plugins
- custom k8s CRD handlers

“More safe”

- Fiat plugins
- credentials override plugins
- Vault integration
- (coming soon) app secrets integration

“More reliable”

- (coming soon-ish) Gremlin integration

“More innovative”

- don’t see anything yet mentioned above that fits your need? you can build your own plugins, add your own services, etc.

