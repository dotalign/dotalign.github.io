<div style="font-size: 40px">DotAlign Cloud deployment > Troubleshooting</div>

<br />

<!-- TOC -->

<div style="font-size: 20px">Table of contents</div>

<br />

- [Function App doesn't seem to be starting up](###Website-is-accessible-but-the-function-app-is-not-starting-up)
- [Updating a deployment fails](###updating-a-deployment-fails)

<!-- /TOC -->

### Website is accessible but the function app is not-starting-up

DotAlign Cloud comprises, among other things, a web app and a function app. The web app allows access to pages and the API, where as the function app is where the data processing happens.

If data processing is not happening, the following may be the issues:

1. Trigger messages are not added to the filter queue. Try turning indexing off and then on again on the web app. That should cause trigger messages to be added to the filter queue.

1. The function app is not in a good state. Try restarting the function app in the Azure portal.

1. The storage account associated with the function app settings is not the same as the storage account url in the web app. Fix the app's settings. Both the web app and the function app must point to the same storage account. This fix can be made in the application settings of the function and/or web apps on the Azure portal.

1. The function app is not deployed correctly. Redeploy DotAlign Cloud. An update should work, but in case it doesn't try a fresh deployment.

### Updating a deployment fails

One reason an update my fail is if the original deployment was made by a different person. The underlying cause is Key Vault, wheere the person updating the deployment does not have access to it, and hence the update fails. 

To fix this the person doing the update must go to the key vault in the [Azure portal](https://portal.azure.com), and add a policy giving them access to all keys, and then retry the update. 

{% include footer.html %}
