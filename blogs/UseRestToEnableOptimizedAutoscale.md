# How to enable optimized autoscale on your cluster

In this blog you would learn how to how to run REST requests on Kusto clusters to enable features, specifically in this post I will show you how to enable optimized autoscale feature which would automatically scale your cluster based on any need. 

## Preparation

To be able to download armclient you first need to download [chocolatey](https://chocolatey.org/docs/installation)

Then you can use chocolatey to download [armclient](https://chocolatey.org/packages/ARMClient)

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

The run the following command to PUT the cluster object including the optimized autoscale setting. This would enable the optimized autoscale on your cluster.

`armclient PUT /subscriptions/11d5f159-a21d-4a6c-8053-c3aae30057cf/resourceGroups/radennisgeneral/providers/Microsoft.Kusto/clusters/radennisscus?api-version=2019-05-15 @C:\Users\radennis\Desktop\temp\clusteroptimized.json -verbose`

This could take a few minutes, after the command returns your cluster will be all set.