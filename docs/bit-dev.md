---
id: bit-dev
title: bit.dev Overview
sidebar_label: Overview
---

Bit.dev server is a cloud service provided by Bit. Accessing bit.dev server requires registering a user account on the bit.dev server. To export and import components from a local workspace to the account, the developer must login from the local workspace.  

![Bit.dev](https://storage.googleapis.com/static.bit.dev/docs/images/bit.dev.png)

> Unlike bit-cli tool that is open-sourced, bit.dev server is proprietary and owned by Bit.

Bit.dev server provides these functions:  

## Remote Scopes Hosting

The primary goal of bit.dev is to be a SaaS service for hosting remote scopes.  
Bit.dev is managing scopes in **collections**. The collection contains additional information on top of the scope:  

- **Collection name** - The name by which the bit scope in the collection is available to developers for sharing or consuming components.  
- **Visibility** - Determines who can view the Collection: A public collection is a free collection that is visible for all registered users. A private collection is limited to the organizations registered users.  
- **License** - The default code license that is applicable for all the components shared in the collection (such as MIT, GPL or other licenses)  

In addition, Bit provides security controls for collections, so only authorized users may access them.  

### Permissions for Collections

bit.dev provides a secured and reliable hub for sharing both public and private code. There are few cases where dependencies between components that are hosted in different collections and owners may harm reliability to provide with working components and not exposing private code.  
To ensure this does not happen, bit.dev enforces the following rules on components exported to multiple collections:

- Components in public collections cannot depend on private components: A public component is available for the entire Bit community. Therefore, it may not depend on components that reside in public collections.  
- Components in private collection cannot depend on components in other organizations: Dependencies in private collections may depend on public collections or on private collections that belong to the same account (personal or organization).  

### Users and Roles

Collection's members are assigned with 3 possible roles:  

| Role | Admin | Developer | Viewer |
|---|---|---|---|
| View Components* | Yes | Yes | Yes |
| Import Components* | Yes | Yes | Yes |
| Install Components * | Yes| Yes | Yes |
| Export Components | Yes| No | No |
| Manage members | Yes | No | No |
| Edit collection's information | Yes | No | No |

> *Viewing, importing and installing is available for all users for public collections.  

## Package Registry

Bit.dev also manages a package registry for all components that are exported to bit.dev collections. Any component exported to bit.dev can be [installed](/docs/installing-components) using package managers (NPM or Yarn).  
All components in the Bit.dev registry are available with the `@bit` prefix. The full name of the component is in the format of `npm i @bit/<owner>.<collection-name>.<component-name>`.  

## Component Playground

![Component Playground](https://storage.googleapis.com/static.bit.dev/docs/images/playground.png)
The component playground is a web-based editor and a rendering environment for each component hosted on Bit.dev server.  
The component playground enables developing example wrappers for the component to show its usage.

## Component CI

When a component is exported to bit.dev, the components CI (Continuous Integration) runs a container for building and testing the component. The container uses the compiler and tester defined for the component in the original project.  

The component is built in an isolated environment. The isolated environment contains the packages and dependencies defined for the component but does not have all the remaining project definitions. This isolation makes sure the component is truly stand-alone and is consumable by other projects.  

The Component's CI displays the results for the component build and test run on the component page on [bit.dev](https://bit.dev). When finishing the build and test tasks, the remote container is purged.  
The component is then available in the component playground.

Each container on the bit.dev CI runs:  

- Ubuntu jessie
- headless chrome driver
- latest version of Bit
- node 8

Each container is limited to:  

- 10 minutes run time
- 2GB RAM
- 0.5 CPU core

## Components Explorer

Bit.dev components explorer allows searching across all the remote collections that the user has access to,  such as the public collections and the user’s or organization's components.  
The component explorer is using metadata on the component:  tags, language, framework, and size for advanced searching capabilities.  

> bit.dev server is using the following IP addresses:  
> 104.154.235.126:22  
> 35.184.176.52:443  
