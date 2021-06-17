---
layout: post
title: NuGet - Problem starting the plugin CredentialProvider.Microsoft (timeout)
comments: true
disqus_identifier: 316f4e21-620b-4dfe-a746-0cb2987c86bf
tags: [nuget,credentialprovider,devops,azure]
---

Plugin 'CredentialProvider.Microsoft' can fail due to timeouts!

Sometimes when you try to restore packages via "nuget restore" or "dotnet restore" in your build pipelines or in your everyday development job, you can get the following error (text has been formatted to increase readability):

```
...
##[error]The nuget command failed with exit code(1) and error(Problem starting the plugin
'X:\DevOpsAgents\2.179.0\CredentialProviderV2\plugins\netfx\CredentialProvider.Microsoft\CredentialProvider.Microsoft.exe'.
Plugin 'CredentialProvider.Microsoft' failed within 5,378 seconds with exit code -1.
Problem starting the plugin 'X:\DevOpsAgents\2.179.0\CredentialProviderV2\plugins\netfx\CredentialProvider.Microsoft\CredentialProvider.Microsoft.exe'.
Plugin 'CredentialProvider.Microsoft' failed within 5,378 seconds with exit code -1.
NuGet.Protocol.Plugins.PluginException: Problem starting the plugin 'X:\DevOpsAgents\2.179.0\CredentialProviderV2\plugins\netfx\CredentialProvider.Microsoft\CredentialProvider.Microsoft.exe'.
Plugin 'CredentialProvider.Microsoft' failed within 5,378 seconds with exit code -1.
   in NuGet.Protocol.Core.Types.PluginResource.<GetPluginAsync>d__5.MoveNext()
...
```

One of the problems is the slowness of the pipeline itself in obtaining authentication request responses (especially if you use multiple nuget feeds): the default timeout for the CredentialProvider is 5 seconds, which can sometimes be too low.

The solution is to increate the timeouts, just add the following code in your azure-pipelines.yml buil files:

```
variables:
- name: NUGET.PLUGIN.HANDSHAKE.TIMEOUT.IN.SECONDS
  value: 30
- name: NUGET.PLUGIN.REQUEST.TIMEOUT.IN.SECONDS
  value: 30
```

Or define the following environment variables:

```
$env:NUGET_PLUGIN_HANDSHAKE_TIMEOUT_IN_SECONDS=30
$env:NUGET_PLUGIN_REQUEST_TIMEOUT_IN_SECONDS=30
```