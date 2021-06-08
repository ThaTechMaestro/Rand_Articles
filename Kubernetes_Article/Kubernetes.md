# Kubernetes Environment Variables (Best Practices)
[Article0.png]

## Overview
### Definition of Key terms
* Introduction
* Declaration of environment variables in Kubernetes (**Best Practices**)
    * Direct declarations using name and value pairs from env fields
    * Use of valueFrom and container Variables
* Summary


## Introduction
Automation of software delivery by creation of pipelines shows that application software passes through different environments such as development, staging and finally gets to production. In this different environments there are some need for application independent settings which are allowed to be introduced through the use of environment variables
During deployments, Kubernetes provides a way for allowing these variables to be introduced into its pods. Just as software applications have some independent configuration setting, Kubernetes also allows this in the creation of its pods, preventing the dependence of container defined variables only. It gives flexibility to the storage of new values within the pod which can be introduced during the creation of containers.
This aids management of different states and defined variables across multiple containers in a pod. Some of its importance includes

1. Environment variables within the same pod can be easily referenced among different containers following the DRY principle.
2. Absence of environment defined variables can lead to repetition of configurations and total dependence on container defined variables.
3. Orchestration is made easier with the presence of environment variables.


## Declaring Environment Variables in Kubernetes (**Best Practices**)

### Direct declarations using name and value pairs from env fields
The env field holds variables in the form of key value pairs. Environment variables are named and assigned values under this field.
[Article4.png]

Considering the diagram above using the **name** and **value** fields.
I created two environment variables, note they are in capitals called **UBUNTU_VERSION** and **UBUNTU_SIZE**. In my configuration, I will be using these variables.
These variables were assigned values using the **value** field and their values are **ubuntu_16** and **100**.



This method is awesome if you have very few environment variables which contain insensitive data and you would like to create and assign values to your environment variables directly .

### Use of **valueFrom** and **container** Variables
The  use of **valueFrom** field replaces the previous value field in the config file. These can be implemented under different cases. These are explained below

 #### - **configMap** and **configMapKeyRef**
Whenever the environment variables values are really long and makes the main yaml file look unnecessary detailed. The values for these environment variables can be called using the configMapKeyRef field as shown below.
[Article5.png]

File name: mongodb-configmap.yaml
This contains the values stored in the data field to be stored in the environment variable.
A simple overview of mongodb pod.
The name of its metadata to provide access is **mongodb-configmap**
The value assigned to the created environment variable is stored using a key value pair in
the data section, with the key called **db_host** and its value which is the value of the created environment in mongo-express.yaml  is **mongodb-service**.
This value will be called using the **configMapKeyRef**
[Article6.png]

File name: mongo-express.yaml
This contains the name of the created environment variable called **MONGODB_SERVICE**.
**configMapKeyRef** references the original texts which are stored in the created environment variable. This is done by using the name and key variables where their values are mongodb-configmap(name of the previous file) and db_host.


#### - Secrets and secretKeyRef
Sensitive datas such as login credentials, accessibility details, can be stored as environment variables using this method. This provides security and prevents unnecessary exposure of sensitive data. 
[Article7.png]


File name: mongodb-secret.yaml
This contains dummy values, as you are not meant to expose any sensitive data.
The type is set to **opaque** to prevent validation which means no conditions have to be met.
The values stored in environment variables are kept by the use of keys **username** and **password** in the data field.
[Article8.png]



File name: mongodb-express.yaml
Environment variables created are **MONGODB_USERNAME** and **MONGODB_PASSWORD**. Values are passed to these variables using the secretKeyRef, values in name references the name of the previous file(mongodb-secret.yaml) and keys (**username** and **password**).
This makes it easy to change the values and it get reassigned to the declared environment variables discreetly.

## Summary

* Few environment variables can be created and assigned values directly using name and value pairs.

* Environment variables containing sensitive values should be passed using Secrets.

* Environment variables are important and makes development of config files flexible.


Read more: [Kubernetes Documentation](https://kubernetes.io/docs/tasks/inject-data-application/)

