---
id: quick-start
title: Quick Start
---

Bit is an [open-source](https://github.com/teambit/bit) tool for components collaboration.

This guide will teach you how to start with Bit and run a simple workflow for sharing components between projects.  

![Bit Workflow](https://storage.googleapis.com/static.bit.dev/docs/images/workflow-full.png)

## What Bit Does?

Bit wraps your code in shareable components that are used across multiple projects. You can get the source code to your local project and modify it or install Bit components as NPM packages.

Think about it this way: without Bit, if you want to share your code between projects, you need to create a separate repo (or a package in your monorepo) for the files, push the code to github (or similar), setup configuration for the project, create a package.json for the package, install all package dependencies, compile and publish the code to NPM.
Then you go to all your projects that need the component and and run `npm install <the new package>`.  
Bit saves you the above process by making the package available and published directly from your project.
All is let for you is to build your apps, be it React, Vue or other, and the code goes into the application using Webpack just like any other `node_modules` package in your project.

Key Features:  

- Extract a component for sharing directly from an existing project.
- Build and test each component separately from the rest of the app.
- Change the source code of the received components inside your project.  
- Get changes in components on top of local changes.
- Contribute back changes made to components directly from your project.
- Auto-generated package.json for each component. Reduces overhead.  
- Version each component individually.
- Automated component versioning according to changes in the dependency graph chain.
- Works alongside with Git, NPM and Yarn.

Bit is provided as a CLI tool. Bit is distributed so the same tool manages the developer’s workspace (client) and the centralized server for sharing components.  
You can setup your own server for sharing components or you can use the [bit.dev](https://bit.dev) cloud hosting solution for private and public components sharing with advanced features such as components CI and showcase.

<div class="learn-more">

- [How bit works](/docs/how-bit-works)
- Suggested [workflows](/docs/workflows) when using Bit

</div>

## Setup

### Install Bit

If you are using node 8.12 and above, you can use NPM or yarn to install bit:  

```bash
npm install bit-bin -g
```

If you are not using Nodejs 8.12 and above, see reference to other [installation methods](/docs/installation).  

### Create account and collection

To share components between different projects, you need to store the components in a centralized scope.  
You can use [bit.dev](https://bit.dev) cloud hosting to share components, or you can use your own server.  
To host components on bit dev [create an account](https://bit.dev/signup) and [run bit login](/docs/apis/cli-all#login).  

Enter a name for your collection and click on create.  
You now have a collection. You can see the export command that you will use later to export components to this collection.  

<div class="learn-more">

- Check out [bit.dev functionality](/docs/bit-dev)
- Find other [authentication methods](/docs/setup-authentication)
- Setup your own [Bit server](/docs/bit-server)

</div>

### Initialize Bit workspace

To share components from a project, you should initialize Bit in that project:  

```bash
cd project-directory
bit init
```

<div class="learn-more">

- Learn more about the [Bit workspace](/docs/workspace).
- The [Best Practices](/docs/best-practices) section has some very useful information on working with Bit.  
- Check out framework-specific guides for [React](/docs/react-guidelines), [Angular](/docs/angular-guidelines), [Vue](/docs/vue-guidelines), [Javascript](/docs/guidelines-js) or [Typescript](/docs/guideline-ts).

</div>

## Track Components

To start tracking components in Bit, use the `add` command. Once the files of a component are added, Bit can identify and verify that all the dependencies of the component exist and all files are tracked. Bit can also associate the component with utilities--such as a compiler or a tester--and start building and testing the component in isolation.

### Add Files

Start tracking components in the project. A good candidate for tracking is a component that is clearly defined and isolated.  
Use the `bit add` command to track a component:

```bash
bit add src/components/my-component.ts --id my-component
```

You can track multiple components at once:  

```bash
bit add src/components/*
```

This will track all the components in the `src/components` directory. Each component will be in a directory named after its component id.  

You can also [track test files](/docs/add-and-isolate-components#track-a-component-with-testspec-files) as part of your components by appending the `--tests` flag. Bit can run these tests in isolation and display the results.

Use `bit status` to verify that the component was tracked and no files are missing.  

<div class="learn-more">

- Advanced options for [tracking components](/docs/add-and-isolate-components)  
- [Best Practices](/docs/best-practices) for tracking components  
- Check out framework-specific guides for [React](/docs/react-guidelines), [Angular](/docs/angular-guidelines) or [Vue](/docs/vue-guidelines)  

</div>

### Add compiler

There are 2 ways to use Bit components in another project:

- Import the source code and embed into the consuming project.
- Install build artifacts (e.g. dist directory) that are consumed by the project as an NPM package.  

To build the component and create build artifacts, you need to define a compiler for the components you share from your project. A compiler is also a component itself, so we use the `bit import` command to import it into our project:

```bash
$ bit import bit.envs/compilers/babel --compiler
the following component environments were installed
- bit.envs/compilers/babel@0.0.7
```

<div class="learn-more">

- Find an [existing compiler](https://bit.dev/bit/envs).
- Implement [your own compiler configuration](/docs/ext-developing-extensions).
- Check out framework-specific guides for [React](/docs/react-guidelines), [Angular](/docs/angular-guidelines) or [Vue](/docs/vue-guidelines) to find relevant compilers.

</div>

### Add Tester

If you [track test files](/docs/add-and-isolate-components#track-a-component-with-testspec-files) for your components, you can define a testing framework Bit will use to run them.
A bit tester, like a compiler, is a tool provided as a component:  

```bash
$ bit import bit.envs/testers/mocha --tester
the following component environments were installed
- bit.envs/testers/mocha@0.0.7
```

<div class="learn-more">

- Find an [existing tester](https://bit.dev/bit/envs)
- Implement [your own tester](/docs/ext-compiling)

</div>

## Publish components

When a component is ready to be shared with others, the developer tags it with a version number following the semver conventions.  
Bit stores the snapshot of the component's source code, enabling it to notify consumers when any changes are made.  

The developer can export a tagged version of the component to a centralized server. The centralized server can be self owned or the [bit.dev](/docs/bit-dev) cloud service can be used as the centralized server.  

### Tag component's version

To set a version for your components use the `bit tag` command.  
Bit locks the state of the component's files and its dependency graph. The tagged version is now immutable (cannot be changed).  
When tagging components, Bit first runs the build and test tasks for the components.  
You can use the `--all` flag to tag all the components that changed in the workspace.

```bash
$ bit tag --all 1.0.0
3 components tagged | 3 added, 0 changed, 0 auto-tagged
added components: components/button@1.0.0, components/login@1.0.0, components/logo@1.0.0
```

### Export components

Now that our components are tracked and versioned, export (publish) them to a remote collection. Collections host and organize your components. Each component can be quickly discovered and consumed in any other project and application.  

Make sure you have a free account on [bit.dev](https://bit.dev) and a collection.  
You now need to authenticate the CLI with the bit.dev account.  
Run `bit login` to open a login page in the browser and authenticate.

```bash
$ bit login
Your browser has been opened to visit: http://bit.dev/bit-login?redirect_uri=http://localhost:8085...
```

Now you’re all set to [publish the components](/docs/export).  
Use the `bit export` command to publish the components from your workspace to [bit.dev](https://bit.dev).

```bash
$ bit export user-name.collection-name
exported 3 components to collection user-name.collection-name
```

Head over to your [bit.dev](https://bit.dev/) collection. All the components have been exported. Try creating and saving examples for your components, which Bit will also show as previews in the collection’s page.

## Consume components

Once exported, the component is now available for consumption by other developers in one of two ways: install or import.

### Install components

You can now use Npm or Yarn to install the components. The component will be installed with build artifacts (e.g., the dist folder) like any other NPM package. The installed component resides in the node_modules folder.

If using NPM or Yarn, NPM should configure Bit as a scoped registry (this is done automatically when doing bit login):

```bash
npm config set '@bit:registry' https://node.bit.dev
```

Then install components with your favorite package manager using the install command in the component’s page. [Example](https://bit.dev/mui-org/material-ui/button):

```bash
npm i @bit/mui-org.material-ui.button
```

<div class="learn-more">

- Use package managers to [Install components](/docs/installing-components)

</div>

## Modify Component

You may want to modify the component source code (for example, to fix a bug or change the functionality). To do so, you need to import the component into your project and apply the changes.

The updated component can now be tagged and re-exported so that other consumers of the component get the update.

### Import Component

Because Bit isolates components and decouples them from the repository, Bit components can be developed from any repository in your codebase.  
To develop a component from the [consuming repository](/docs/sourcing-components) use the `bit import` command:

```bash
bit import mui-org.material-ui/button
 ```

**Note**: If your changes are temporary, and you would like to revert back to using the components from `node_modules`, you can use the `eject` command to remove components from the local workspace and install them with the NPM client.

<div class="learn-more">

- [Importing components](/docs/sourcing-components)  
- [Eject command](/docs/apis/cli-all#eject)

</div>

### Publish component changes

Changes made to imported components can be updated in the collection and consumed across projects.  
When you’re done making changes, you can update them as a new version of the component in the collection (given you have permissions to update the collection). Alternatively, the changed component can be exported as a new component.  
When a component is updated with a new version in the collection, every repository that houses that component can get the changes.

<div class="learn-more">

- [Update component in workspace](/docs/sourcing-components#importing-for-modified-components)

</div>

## Merge changes

An important feature of Bit is that even if the component is imported and changed inside of a project it still receives all of the modifications made to the original component. Updates can be received in the original project as well as any project that imported the component. This helps teams sync changes when developing components from different projects.

```bash
bit import mui-org.material-ui/button
bit checkout mui-org.material-ui/button --interactive-merge
 ```

<div class="learn-more">  

- [Merging changes](/docs/sourcing-components#merge-incoming-changes) for components.
- [Handling merge conflicts](/docs/sourcing-components#handle-merge-conflicts).
- [Bit checkout command](/docs/apis/cli-all#checkout).

</div>

---

**Need help? Have a feature request? Want to talk over a beer?** don’t hesitate to [reach out](https://bit.dev/support) or chat with the team on [Gitter](https://gitter.im/bit-src/Bit).
