# Humanitec Roadmap

This document provides a high-level view on our product roadmap for this and upcoming quarters. It also lists roadmap items of past quarters.

## Q4 2020

### Volumes

Volumes pose a particular area of complexity for Kubernetes applications. This feature aims to simplify handling of volumes by treating volumes as External Resources inside Humanitec. This means developers can simply declare a dependency on a volume and where it should be mounted and Humanitec will figure out how to provision or connect the volume.

### Role-Based Access Control

Humanitec currently has a very simple permission model: any member of an organization can perform all actions in that organization. Many larger companies and companies in regulated markets have more complex needs and so require a finer-grained permission structure. Examples include limiting who can deploy to different environments, who can create new applications, or who can configure external resources. These fine-grained permissions should use a Role-Based Access Control (RBAC) model. To allow easy integration with corporate systems, Humanitec should be able to function as a SAML Service Provider.

### Better debugging for failed deployments

Debugging failed deployments in Kubernetes is particularly complex because of the number of moving parts involved. Some errors are straightforward and come to light immediately (e.g. container cannot be pulled from the registry) while others (e.g. a container entering a crash-loop) can come to light many minutes after a deployment has been kicked off. This feature set aims to outline the sequence of steps performed by Humanitec when initiating a deployment. It informs the user once the step is performed successfully, or alternatively provides details for easier debugging if an error occurs.

## Q1 2021 and beyond

The following features are in our backlog and planned for implementation from Q4 2020 onwards. They are subject to change based on additional feedback we will gather until then.

### Bring your Helm Charts

Many potential customers already have setups for Kubernetes involving Helm charts. Humanitec already has the ability to deploy arbitrary Helm charts alongside application modules. However, configuration through Humanitec is very limited. This feature aims to provide a similar level of support to users for arbitrary helm charts as they enjoy for modules fully managed by Humanitec.

### Debug Flow Tools

As applications become more complex, it is harder to run the full application locally on the developer’s laptop. Moving development environments into the cloud solves that problem but makes it very hard to debug services under development. Humanitec is investigating a range of technical fixes to help developers bridge the gap between the local machine and the cloud.

### Shared Services

Larger IT deployments in organizations often include services that are shared by multiple applications. (E.g., a product inventory service might be used by Point of Sale, logistics, forecasting, and financial reporting applications.)

For highly used services, a common pattern is to deploy the production version twice: once in the Production Environment and again in the Development environment. This allows developers to depend on the real service for testing while also not worrying about having to manage it as part of their workflow. This could be managed by Humanitec by marking an environment as being “exported” to all other development environments in all applications.

### On-premise Offering

Many companies in regulated industries are wary of the public cloud and so wish to run all services used by their in house engineers themselves. Companies in this situation wishing to use Humanitec would, therefore, require an on-premise version of Humanitec. Being able to deploy and use Humanitec in this configuration is not particularly complex, however, the support process around it and the pace of receiving updates is the biggest challenge.

### Team Performance Monitoring

Humanitec is sitting on a large amount of data and so has the potential to provide valuable insights about team performance. It would be relatively straightforward to measure time from “commit to production” for example.

## Past Quarters

### Q3 2020

#### Seamless Onboarding Experience

Users wishing to try Humanitec should have a seamless onboarding experience. Providing full in-app support and guided tutorials will significantly shorten new users’ time-to-value and help them to get started faster. Additionally, one of the key activation points is connecting Humanitec to your own infrastructure, therefore it is essential to provide users a way to easily use packaged CI steps for major CI tools such as GitLab, GitHub, CircleCI, TravisCI, DroneCI, and Jenkins.

#### Event-Driven Jobs

It is often useful to be able to perform actions on events such as “new environment created”, “deployment completed”, or “environment deleted”. The actions could be as simple as posting a Slack message or as complex as running a suite of integration tests or initializing data. This feature covers running basic webhooks to enable things like Slack integration and running Kubernetes Jobs that can support things like running automated tests in an environment.

#### Runtime Monitoring

When running applications in production, monitoring is essential. With this feature, Humanitec will provide CPU, memory, and disk space statistics and will inform developers when these limits are reached. This allows for easier debugging and enables developers to make educated decisions while configuring their containers and replicas on Humanitec. Furthermore, Humanitec should provide the required integrations to most commonly used monitoring tools (e.g., Prometheus, Grafana, DataDog) to fully support development teams’ existing infrastructures. 

#### Humanitec CLI

Many developers are not comfortable working with a GUI. At the same time, an API is hard to use for day-to-day deployments and application management. The Humanitec CLI will provide a middle ground between the API and the GUI. This feature is essential to pursue the API-first approach and provide full support to users that spend most of their time interacting with tools via their terminal. Using Humanitec CLI developers will be able to authenticate, create, and configure apps, deploy and manage environments, and much more. The CLI will also provide special functionality such as port forwarding into running applications or direct connection to managed databases.

### Q2 2020

#### Provisioning External Resources

Many applications rely on databases and services that run external to the cluster. Humanitec already supports the automatic generation of Postgres databases via Google CloudSQL. This feature would extend the range of services supported. A stretch objective here is also to provide a way for developers to build their own implementation if the set of services supported by Humanitec does not cover what they need.

#### Environment Management

Humanitec allows developers to easily spin up and tear down environments on demand. Additionally, developers need to manage their workflows across multiple environments. This feature aims to take out the manual work associated with environment management and ease team workflow. It allows developers to compare deployments in different environments, patch the difference to another environment, manage variables per environment, as well as filter and label them for automation.

#### Published API

Much of what Humanitec can do will help with various forms of automation. For example, a test automation system may want to use Humanitec to spin up a new, clean environment to run tests in and then tear it down once it is done. Humanitec has the potential to add a whole set of use cases by defining and publishing a well-designed API.

#### Runtime Status

Successful deployment does not mean successful running. Developers need to know the runtime status of what they have deployed. This feature aims to show developers an all-in-one overview of live pods statuses and provide basic monitoring of all services deployed with Humanitec.

#### Automatic Deployments

Most of the engineering teams are either running automated deployments or aspire to in the future. Humanitec has all of the functionality required to automate deployments but lacks the ability to perform a deployment based on a trigger. This feature would allow developers to define deployment rules that could be run based on triggers. Rules would be of the form, if a new container image is built on branch _x_, deploy to environment _y_ and would be triggered on an event like: a new container image has been built by a CI pipeline.

### Q1 2020

#### Dynamic Environment Variable Management

Environment management across environments is complex and error-prone. With this feature, environment variables are classified as one of 3 types:

* **Static:** The same for each environment deployed in
* **Dynamic:** Humanitec can infer the different values that a variable should have in each environment via a system of “placeholders” and string formatting
* **Environment Specific:** Developers must either accept or update the value of the variable when being deployed to a different environment

For example, a dynamic environmental variable definition which resolves to the external DNS name of a module called “api-server” would be:

        API_SERVER_NAME = "${modules.api-server.service.externalName}"

#### Deployment Sets

*Deployment Version History*

The full state of each deployment to an environment is recorded as a “deployment set”. Deployment sets can be compared to show differences between them. A previous deployment set can be redeployed to perform a rollback. Cloning one environment to another is done by applying a deployment set from one environment into another. As part of this, the Dynamic Environment Variable Management feature ensures that environmental variables are correctly handled.

*Serialization of Deployment Sets* [to be completed in Q2]

The deployment set is the unit of state that is used to serialize out Helm Charts that describe a particular deployment. In the future, these charts can be directed towards a git repository and so form part of a “GitOps” process.

#### Generalized Module Sources

The current version of Humanitec has a tight dependency on GitHub. This feature relaxes that dependency.

*Connect your CI*

Rather than building containers directly from source code, Humanitec will hook into the end of your CI pipeline. This allows any successfully built and tested container to be used directly in Humanitec.

#### External Modules (3rd Party In-Cluster Components / Services)

Many modern cloud-native applications make use of services and components based on popular open-source projects, for example, Redis or RabbitMQ. With this feature, we aim to build a growing set of curated, ready to deploy packages of these popular components and services. These packages can then be simply deployed into the cluster by developers.
