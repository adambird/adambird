---
layout: post
title: "Base View Model in ASP.NET MVC"
description: "I needed to reference some user context variables from within my site master page that had some logic behind their retrieval. In this example I need to choose the display language for a user. Check cookie present indicating user's language prefere..."
date: Mon Nov 08 05:00:00 -0800 2010
comments: true
author: Adam
categories: [dev]
---

I needed to reference some user context variables from within my site master page that had some logic behind their retrieval.

In this example I need to choose the display language for a user.

<ol>
<li>Check cookie present indicating user's language preference</li>
<li>if not use HTTP language header</li>
<li>if not use the application default language</li>
</ol>
The pattern I've ended up using is to implement a base view model from which all my specific view models inherit as follows:<span style=""> </span>

` `

` `

` `

<p><code>
<div class="CodeRay">
  <div class="code"><pre>public class BaseViewModel {     
    public string UserLanguage     
    {      
       get      
       {          
           return HttpContext.Current.Request.Cookies[&quot;language&quot;] != null        
               ? HttpContext.Current.Request.Cookies[&quot;language&quot;].Value           
               : CultureInfo.CurrentCulture.TwoLetterISOLanguageName;     
        }
    } 
}</pre></div>
</div>

</code></p>
&nbsp;

I then use the generic class for the master page

`&lt;%@ Master Language="C#" Inherits="System.Web.Mvc.ViewMasterPage&lt;BaseViewModel&gt;" %&gt;`

Which then allows me to reference the model in the master page and keep the logic for deciding the language to display centralised.

Not sure I'm wholly happy with the inheritance as it adds a level of dependencies which smells a bit off but in the small application I'm working in it was easy to implement and does what I need.
