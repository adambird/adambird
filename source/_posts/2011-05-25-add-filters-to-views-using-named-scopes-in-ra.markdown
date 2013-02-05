---
layout: post
title: "Add Filters to Views Using Named Scopes in Rails"
description: "This really helped me deliver a rather lovely solution to filtering records on Bunch Rides. http://www.idolhands.com/ruby-on-rails/guides-tips-and-tutorials/add-filters-... and here's what I did: http://rides.bunch.cc/clubs/54 and the helper code:..."
date: 2011-05-25 00:00
comments: true
author: Adam
categories: [dev]
---

This really helped me deliver a rather lovely solution to filtering records on Bunch Rides.

<a href="http://www.idolhands.com/ruby-on-rails/guides-tips-and-tutorials/add-filters-to-views-using-named-scopes-in-rails">http://www.idolhands.com/ruby-on-rails/guides-tips-and-tutorials/add-filters-...</a>

and here's what I did: <a href="http://rides.bunch.cc/clubs/54">http://rides.bunch.cc/clubs/54</a>

and the helper code:

    def table_filter(filters, selected_scope)
      content_tag(:div,
        raw(filters.collect { |filter| 
          content_tag(:a, filter[:label], :href =&gt; &quot;?show=#{filter[:scope]}&quot;, :class =&gt; ('selected' if filter[:scope] == selected_scope)) }),  
        :class =&gt; 'table-filter')
    end