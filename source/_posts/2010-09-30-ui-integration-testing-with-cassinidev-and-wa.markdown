---
layout: post
title: "UI Integration Testing with CassiniDev and WatiN, not Selenium"
description: "This was to be my first foray into UI integration testing. I've always been a sceptic, scare by the brittleness of any kind of recorded UI test. Luckily Gemma and Jonathan in the dev team persisted and showed me Selenium RC and WatiN. Both these s..."
date: 2010-09-30 00:00
comments: true
author: Adam
categories: [dev]
---

This was to be my first foray into UI integration testing. I've always been a sceptic, scare by the brittleness of any kind of recorded UI test. Luckily Gemma and Jonathan in the dev team persisted and showed me Selenium RC and WatiN. Both these solutions allow you to write the integration tests in code, a critical requirement for me.

Also key for me was this had to work in Hudson, my chosen Continuous Integration server, so simple deployment and management was key. I love being able to check everything I need into source control and have it 'miraculously' run on the CI server with little or no config.

The first thing I needed was a deployable web server I could, ideally, run in process. Enter <a href="http://cassinidev.codeplex.com/" title="CassiniDev" target="_blank">CassiniDev</a>&nbsp;and specifically the CassiniDev4-Lib.dll. Now this was really tricky to get going ;).

1. Add reference to CassiniDev4-Lib.dll

2. Put following code in my TestFixtureSetup

<div class="CodeRay">
  <div class="code"><pre>_hostServer = new CassiniDevServer();
_hostServer.StartServer(@&quot;..\..\..\mywebapp&quot;);</pre></div>
</div>

&nbsp;

<a href="http://seleniumhq.org/" target="_blank">Selenium</a> wasn't quite so simple.

It failed the simple deploy requirement because you have to run the Selenium Server and then use the RC libraries to interact with it and send commands to then run on browsers.

It also has/d a bug where it sends a HEAD before it sends a GET which breaks if your MVC Controller Action as an HttpGet attribute. It sends a 404 because HEAD isn't acceptable.

Sky from the CassiniDev team was &uuml;ber-helpful finding this out for me&nbsp;<a href="http://cassinidev.codeplex.com/Thread/View.aspx?ThreadId=227174" target="_blank">http://cassinidev.codeplex.com/Thread/View.aspx?ThreadId=227174</a>.

<a href="http://watin.sourceforge.net/" target="_self">WatiN</a> was a different story though. Very simple to use and all run in process. There were a couple of gotcha's though.

I had to make sure NUnit was properly running .net 4, stackoverflow helped me there&nbsp;<a href="http://stackoverflow.com/questions/2635794/nunit-fail-with-system-argumentexception-the-net-4-0-framework-is-not-available" target="_blank">http://stackoverflow.com/questions/2635794/nunit-fail-with-system-argumentexception-the-net-4-0-framework-is-not-available</a>

And I got a rather gruesome COM exception when I pushed it all up to my CI server.&nbsp;

<div class="CodeRay">
  <div class="code"><pre>NSystem.UnauthorizedAccessException: Retrieving the COM class factory for component with
        CLSID {0002DF01-0000-0000-C000-000000000046} failed due to the following error: 
        80070005 Access is denied. (Exception from HRESULT: 0x80070005 (E_ACCESSDENIED)).
    at WatiN.Core.IE.CreateNewIEAndGoToUri(Uri uri, IDialogHandler logonDialogHandler, 
        Boolean createInNewProcess)
    at WatiN.Core.IE..ctor(String url)</pre></div>
</div>

Luckily I found this post that talks through setting the COM permissions correctly for this kind of issue. Specifically giving the correct permissions to the user account used by the Hudson server. <a href="http://www.stuffthatjustworks.com/How+To+Fix+UnauthorizedAccessException+Retrieving+The+COM+Class+Factory+For+Component+With+CLSID.aspx" target="_blank">How To Fix UnauthorizedAccessException Retrieving The COM Class Factory For Component With CLSID</a>

Early days, but I now have a green build that includes actually navigating to one of my forms and entering text within an integration test.

Selenium seemed to be the obvious choice for UI testing. I read somewhere that Google is throwing loads of effort into developing it so it would be a good horse to back. However, when I look at my requirements I don't need what Selenium offers.

Multi, cross-browser testing is nice but I'm just looking to confirm stories are operational and routes through my application are valid. The simplicity of WatiN seems to satisfy that nicely.
