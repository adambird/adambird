---
layout: post
title: "Automating Azure deployment with Windows PowerShell"
description: "One of the pre-requisites I had for using Azure was that it could be deployed automatically as part of an integration and deployment process. A quick scan through the Labs in the Windows Azure Platform Kit, which by the way have been an excellent ..."
date: 2010-01-22 00:00
comments: true
author: Adam
categories: []
---

One of the pre-requisites I had for using Azure was that it could be deployed automatically as part of an integration and deployment process.

A quick scan through the Labs in the <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=413E88F8-5966-4A83-B309-53B7B77EDF78&amp;displaylang=en" target="_blank">Windows Azure Platform Kit</a>, which by the way have been an excellent resource so far, gave me Windows PowerShell as the option.

&nbsp;

It proved pretty easy to get up and running. Here's my quick start guide.

<h4>Download PowerShell and the Azure CmdLets</h4>
If you're running anything other than Windows 7 you'll need to download and install PowerShell from here: <a href="http://support.microsoft.com/kb/926139" target="_blank">http://support.microsoft.com/kb/926139</a>.

Next you need the Azure Power Shell CmdLets available here: <a href="http://code.msdn.microsoft.com/azurecmdlets" target="_blank">http://code.msdn.microsoft.com/azurecmdlets</a>. Once you've downloaded and extracted the scripts, you then just need to execute the <em>startHere</em> script to get them installed.

<h4>Script the deployment</h4>
I won't regurgitate the full instructions as it's best to read through the Lab <em>Deploying and Monitoring Applications in Windows Azure - Exercise 2 - Using PowerShell to Manage Windows Azure Applications</em> in the Windows Azure Platform Kit.

Is also worth checking reading about them on Ryan Dunn's blog: <a href="http://dunnry.com/blog/WindowsAzureServiceManagementCmdLets.aspx" target="_blank">http://dunnry.com/blog/WindowsAzureServiceManagementCmdLets.aspx</a>

This is the script I ended up with

<div class="CodeRay">
  <div class="code"><pre>Add-PSSnapin AzureManagementToolsSnapIn

$service = &quot;&lt;service_name&gt;&quot;
$sub = &quot;&lt;subscription_id&gt;&quot;
$cert = Get-Item cert:\CurrentUser\My\&quot;&lt;thumbnail&gt;&quot;
$package = &quot;&quot;&lt;package_file&gt;&quot;&quot;
$config = &quot;&quot;&lt;config_file&gt;&quot;&quot;

if ($args.Length -eq 1)
{
        $label = $args[0]
} 
else
{
        $label = &quot;unknown&quot;
}

/* 
    If the staging deployment doesn't exist, this will create it
*/
/*
New-Deployment -serviceName $service -subscriptionId $sub -certificate $cert -slot staging -package $package -configuration $config -label $label |
Get-OperationStatus –WaitToComplete
*/

/*
    Upgrade the current staging deployment
*/
Get-HostedService -serviceName $service -subscriptionId $sub -certificate $cert |
Get-Deployment -slot staging |
Set-Deployment -package $package -label $label |
Get-OperationStatus –WaitToComplete

/*
    Set to running
*/
Get-HostedService -serviceName $service -subscriptionId $sub -certificate $cert |
Get-Deployment -slot staging |
Set-DeploymentStatus running |
Get-OperationStatus –WaitToComplete

/*
    Move staging to production, this will actually swap them over
*/
Get-HostedService -serviceName $service -subscriptionId $sub -certificate $cert |
Get-Deployment -slot staging |
Move-Deployment |
Get-OperationStatus –WaitToComplete</pre></div>
</div>

You'll note that I have two options for the deployment <em>New-Deployment</em> and <em>Set-Deployment</em>.

When you start off, Azure has nothing in either of the staging or production slots so you have to use <em>New-Deployment</em>.

When you create the first staging deployment then move to production, this empties the staging slot.

When you run through the process again, with <em>New-Deployment</em>, the <em>Move</em> swaps the deployments over so staging then has the first deployment and production the second.

At this point you can then start upgrading the staging deployment with <em>Set-Deployment</em>

You can of course avoid all this by just doing two manual deploys before automating it.
