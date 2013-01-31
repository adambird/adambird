---
layout: post
title: "Decoupling the domain from the interface with ReST and method objects"
description: "MVC (model-view-controller) is a well established pattern for developing web applications which I've used with both Rails and ASP.NET. Both of these frameworks provide powerful interface add-ons, for example validations, with which you can decorat..."
date: 2012-05-08 00:00
comments: true
author: Adam
categories: [dev]
---

MVC (model-view-controller) is a well established pattern for developing web applications which I've used with both Rails and ASP.NET. Both of these frameworks provide powerful interface add-ons, for example validations, with which you can decorate your models and let the framework do a lot of heavy lifting for you.

However, I've found that using these can lead to a leaky coupling between domain and UI logic which slowly and inexorably adds complexity and maintainability headaches.

I've started using a new approach that I'm finding more flexible but has enough pragmatism to leverage the framework hooks and features.
<!-- more -->
### Treat the web app as an API

Whatever you do, make the web application url scheme work with resources. For example if you need to expose a password reset feature you would do the following

    POST /password_resets

You would then provide a link to the created password reset request in an email

    GET /password_resets/wieurh2398hf38h83h028f2=f92-0fw0f

The the user would then submit a new password with

    PUT /password_resets/wieurh2398hf38h83h028f2=f92-0fw0f

All good so far. But here's the next stage

###Resources are not necessarily domain objects

When I create a password reset request I'm not actually creating a record in a database somewhere, I'm actually performing an operation on a User domain object to append a random token that I can use to look up the user record when the user clicks on the link.

    class User
      def create_password_reset_token(at)
        password_reset_token = SecureRandom.urlsafe_base64
        password_reset_token_expires_at = at.utc + (60*60*24)
      end
    end

Enter the method object. For this interaction I need two. CreatePasswordReset and ResetPassword.

    class CreatePasswordReset
      attr_accessor :user_id

      def execute(at)
        user = user_store.get(user_id)
        user.create_password_reset(at)
        user_store.save(user)
      end
    end

    class ResetPassword
      include ActiveModel::Validations
      attr_accessor :token, :password, :password_confirmation
      
      validate :password, :presence =&gt; true   
      validate :password, :confirmation =&gt; true    
      
      def execute(at)
        if valid?       
          user = user_store.get_by_token(token)       
          user.set_password(password)       
          user_store.save(user)     
        end   
      end 
    end

The corresponding controller methods are super simple as they just treat the result of the execute method as a binary result.

    class PasswordResetController &lt; ApplicationController   
      def update     
        command = ResetPassword.new(params[:reset_password])     
        if command.execute(current_time)       
          redirect_to account_path, :notice =&gt; &quot;We've updated your password&quot;     
        else       
          render :edit     
        end   
      end
    end

The result is

+ I have a web api that maps to operations that a user wants to perform and domain that maps to the entities in my business system.
+ I have very simple controllers that delegate the domain interactions to method objects that understand the required interactions.
+ I leverage the validation helpers provided by the framework without polluting my domain with UI detail

I do appreciate that validation logic is actually domain logic but this pattern allows a pragmatic allocation of responsibilities in order to increase productivity.

### Roles and responsibilities

**Controllers** to ensure only authorised users can perform operations on the domain

**Method Objects** to ensure domain interactions are valid and well formed.

**Models** to model the domain irrespective of how it's being interacted with.

What do you think?
