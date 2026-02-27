---
id: 262
title: 'Getting started with Linode Object Storage on Rails'
date: '2021-04-01T10:23:30-07:00'
author: Martin
layout: post
permalink: /getting-started-with-linode-object-storage-on-rails
categories:
    - Uncategorized
tags:
    - Linode
    - Programming
    - Rails
    - S3
---

A current client project of mine involves a *lot* of image storage.

A job for Amazon S3, right? That's the default answer a lot of the time, but I've always found Amazon's configuration and policy setups extremely long-winded so this time I thought what about using an alternative?

As the project itself is hosted with Linode, trying out [Linode Object Storage](https://www.linode.com/products/object-storage/) as an S3 compatible storage option for a fraction of the cost appealed to the both the client and I.

To get started, the first thing you need to do is enable **Object Storage** on your Linode account – this is done automatically the first time you create a **Bucket** (a named area for storing items) or an **Access Key** in the Object Storage area of your account.

Note that **this will start billing for Object Storage right away**, even if you don't store anything in your Bucket(s). Fortunately the pricing is very low at 5USD p/m for a generous amount of storage.

Let's show this is done by creating a single test bucket for Development purposes.

#### Steps to create a Bucket:

1\) log into your Linode account
2\) select Object Storage from the side menu
3\) hit the 'Create a Bucket' button and supply the name, e.g. 'mybucket-dev' plus select a datacenter close to you from the Region dropdown

Once your Bucket is created, you can create an Access Key (credentials to access the bucket) by selecting the '**Access Keys**' tab and following the instructions to create one with **Read/Write** access to your newly created Bucket – making a secure note somewhere of the secret key that will be displayed briefly after creation.

Those familiar with S3 will already notice how much easier this is than with the convoluted Amazon S3 management tools which, to me at least, require several hoops to jump through at every turn.

Armed with the Bucket and Access Key we are ready to wire these up as ActiveStorage ready services in our Rails app.

In `config/storage.yml` create an entry such as:

```yaml
linode_obj_dev:
  service: S3
  endpoint: https://eu-central-1.linodeobjects.com
  access_key_id:
  secret_access_key:
  region: default
  bucket: mybucket-dev
  public: true
```

…replacing the endpoint and bucket fields as appropriate.

Rather than storing the Access Key details in plain sight in **config/storage.yml** this example makes use of Rails credentials system for encrypted storage, i.e.

```bash
$ rails credentials:edit
```

and then entering:

```yaml
linode_obj_dev:
  access_key_id: THELONGACCESSKEY
  secret_access_key: THESECRETACCESSKEY
```

…replacing the actual key values with the ones supplied by Object Storage.

One final change is then to update your environment file at `config/environments/development.rb` setting to use the correct **ActiveStorage** service:

```ruby
config.active_storage.service = :linode_obj_dev
```

After doing this, and restarting your Rails server, any ActiveStorage fields you configure on your models will be stored on Linode Object Storage and not on disk (the default).

After doing a few test uploads you can view the Linode Object Storage page to verify the number of items and their content.

Once content you can then repeat the above instructions for Staging and Production environments – and that's 'all' there is to it!
