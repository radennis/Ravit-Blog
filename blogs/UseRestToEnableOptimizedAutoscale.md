# How to enable optimized autoscale on your cluster

In this blog you learn how to how to  enable optimized autoscale feature on your cluster, which would enable your cluster to scale between a minimum and maximum capacity (number of instances) you set.

In addition, you will also learn how to run REST requests on Kusto resource and configure them and how to download armclient tool which enables you to run ARM requests very easily.

Note, you can enable optimized autoscale feature [using the portal](https://docs.microsoft.com/en-in/azure/data-explorer/manage-cluster-horizontal-scaling#optimized-autoscale), from the scale out blade, in your Kusto cluster.

## What is the optimized autoscale feature

When you create your cluster, it is created with 2 instances count. When the time comes and your data grows, consumption and CU grow you would want to scale out your cluster. When you consume less you would want to save COGS and scale out your cluster.

To do that, you can either configure an [Azure autoscale rule](https://docs.microsoft.com/en-in/azure/data-explorer/manage-cluster-horizontal-scaling#custom-autoscale) that based on some metrics would scale in/out your cluster.

Alternitively you can use the optimized autoscale feature, in which you provide the minimum and maximum instances count, and have Kusto autoscale your cluster based on optimized parameters.

## Preparation

To be able to download armclient you first need to download [chocolatey](https://chocolatey.org/docs/installation)

Then you can use chocolatey to download [armclient](https://chocolatey.org/packages/ARMClient)

ARM client is an easy tool that would help you send requests to ARM without having to generate a token etc.

## Instructions

Open a command prompt and login to armclient:

`armclient login`

Then you can run the following command to get the current settings of your cluster

`armclient GET /subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroupname/providers/Microsoft.Kusto/clusters/yourclustername?api-version=2019-05-15`

Take the response and add the following section, this is the configuration of the optimized autoscale. You can change the minimum and maximum capacity per your request. For instance if you don't want your cluster to go beyond 8 instances because of a cost limitaion then you can use maximum 8. 

`    "optimizedAutoscale": {
      "version": 1,
      "isEnabled": true,
      "minimum": 2,
      "maximum": 8
    },`

Take the response of the command the above section and save it into a json file.

Then run the following command to PUT the cluster object including the optimized autoscale setting. This would enable the optimized autoscale on your cluster.

`armclient PUT /subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroupname/providers/Microsoft.Kusto/clusters/yourclustername?api-version=2019-05-15 @pathToFileSaved.json -verbose`

This could take a few minutes, after the command returns, your cluster will be all set.