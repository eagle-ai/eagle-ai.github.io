---
layout: doc
title:  "Deploy Eagle in Sandbox"
permalink: /docs/deploymentInSandbox.html
---

### Pre-requisites

##### Download HDP sandbox
> To install eagle on a sandbox you need a HDP sandbox image imported in a virtual machine.
>
> 1. [Virtualization environment](http://hortonworks.com/products/hortonworks-sandbox/#install) two options
> 2. [Hortonworks Sandbox](http://hortonworks.com/products/hortonworks-sandbox/#install) v 2.2.4 or later.

##### Register HDP sandbox
> 1. [Register](http://127.0.0.1:8888/) Hortonworks sandbox.
> 2. [Enable Ambari](http://127.0.0.1:8000/). Click on Enable Button.
> 3. [Login](http://127.0.0.1:8080) as admin/admin.

##### Start Storm & HBase & Kafka via Ambari
> 1. Add root as a HBase superuser via [Ambari](http://127.0.0.1:8080/#/main/services/HBASE/configs) (Optional, a user can operate HBase by sudo su hbase, as an alternative).
>
    ![Hbase User](/images/docs/AmbariHbaseConfig.png "Hbase User")
> 2. Start HBase & Storm & Kafka from Ambari UI. Here using Storm as an example
![Restart Services](/images/docs/startStorm.png "Services")



### Steps of Eagle Installation
> Step 1: Import Eagle tarball into Sandbox
>
* option 1: build form source code. Clone stable version from [eagle github](https://github.corp.ebay.com/eagle/eagle/tree/release1.0), and
> build the project `mvn clean install -DskipTests=true`, and then a package eagle-xxx-bin.tar.gz will be generated under ./eagle-assembly/target
* option 2: download the successful built package through the Internet.

> Step 2:  copy the package into your sandbox
>
    scp -P 2222 eagle-0.1.0-bin.tar.gz root@127.0.0.1:/usr/hdp/current/.

> Step 3: Extract eagle tarball package
>
    cd /usr/hdp/current
    tar -zxvf eagle-0.1.0-bin.tar.gz
    mv eagle-0.1.0 eagle

> Step 4: Install Eagle service in sandbox
>
* option 1: start with eagle command line
>
        example/eagle-sandbox-starter.sh

>
* option 2: start with eagle ambari plugin
    1. `/usr/hdp/current/eagle/bin/eagle-ambari.sh`
    2. Restart [Ambari](http://127.0.0.1:8000/) click on disable and enable Ambari back.
    3. Add Eagle Service to Ambari. Click on "Add Service" on Ambari Main page
        ![AddService](/images/docs/AddService.png "AddService")
        ![Eagle Services](/images/docs/EagleServiceSuccess.png "Eagle Services")

>Step 5: Add Policies and meta data required by running below script.
>
        /usr/hdp/current/eagle/examples/sample-sensitivity-resource-create.sh
        /usr/hdp/current/eagle/examples/sample-policy-create.sh

