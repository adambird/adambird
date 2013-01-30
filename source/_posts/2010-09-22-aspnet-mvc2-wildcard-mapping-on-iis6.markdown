---
layout: post
title: "ASP.NET MVC2 Wildcard Mapping on IIS6"
description: "Spent a little while tearing my hair out on this one. I followed Steve Sanderson's post ( http://blog.stevensanderson.com/2008/07/04/options-for-deploying-aspnet-mvc-t... ) but I was still getting the classic IIS 404Sorted"
date: Wed Sep 22 02:09:00 -0700 2010
comments: true
author: Adam
categories: [dev]
---

Spent a little while tearing my hair out on this one. I followed Steve Sanderson's post ( <a href="http://blog.stevensanderson.com/2008/07/04/options-for-deploying-aspnet-mvc-to-iis-6/">http://blog.stevensanderson.com/2008/07/04/options-for-deploying-aspnet-mvc-t...</a> ) but I was still getting the classic IIS 404<img src="/images/aspnet-mvc2-wildcard-mapping-on-iis6/Capture_dcran_2010-09-22_10.02.png">
Finally found the answer in the Web Service Extensions section of IIS Manager. I had to enable the ASP.NET v4 extension
<p><div class='p_embed p_image_embed'>
<img alt="0capture_dcran_2010-09-22_10" height="205" src="http://getfile2.posterous.com/getfile/files.posterous.com/adambird/MDvQ69C5ItPKEoB4kuMHKPCzWXJi4I0MxhPWWTm3jjWokINR6lokcYWbDQlN/0Capture_dcran_2010-09-22_10.02.png" width="486" />
</div>
</p>But just clicking Allow here didn't work. I actually had to go to Properties > Required Files and Allow the aspnet_isapi.dll.
<p><div class='p_embed p_image_embed'>
<img alt="1capture_dcran_2010-09-22_10" height="436" src="http://getfile6.posterous.com/getfile/files.posterous.com/adambird/eh52RV4z3yq4Yu0S0vgoYfCVzroU6kiczhVuZzYpp6Epkq5dcpgVI6lo2iXJ/1Capture_dcran_2010-09-22_10.02.png" width="402" />
</div>
</p>Sorted
