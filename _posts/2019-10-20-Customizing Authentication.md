---
title: Customizing Authentication
tags: [Django]
mathjax: true
---

### Customizing Authentication

Django maintains a list of “authentication backends” that it checks for authentication. When somebody calls [`django.contrib.auth.authenticate()`](https://docs.djangoproject.com/en/2.2/topics/auth/default/#django.contrib.auth.authenticate), Django tries authenticating across **all** of its authentication backends with **lazy evaluation**. 

**Therefore, the order of authentication backend matters.**

Django's default authentication backend doesn't provide protection against brute force attacks via any **rate limiting mechanism**.

> A **rate limiting algorithm** is used to check if the user session (or IP-address) has to be limited based on the information in the session cache.

> Once a user has authenticated, Django stores which backend was used to authenticate the user in the user’s session, and re-uses the same backend for the duration of that session whenever access to the currently authenticated user is needed. This effectively means that **authentication sources are cached on a per-session basis**, so if you change [`AUTHENTICATION_BACKENDS`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-AUTHENTICATION_BACKENDS), you’ll need to clear out session data if you need to force users to re-authenticate using different methods. A simple way to do that is simply to execute `Session.objects.all().delete()`.

### Using a custom user model when starting a project

##### Using a custom user model

1. Create a model inheritate from `AbstractUser`
2. Point [`AUTH_USER_MODEL`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-AUTH_USER_MODEL) to it
3. Register the model in the app's `admin.py`

##### Problems when changing to a custom user model mid-project

Due to limitations of Django’s dynamic dependency feature for swappable models, the model referenced by [`AUTH_USER_MODEL`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-AUTH_USER_MODEL) must be created in the first migration of its app (usually called `0001_initial`); otherwise, you’ll have **dependency issues**.

In addition, you may run into a **CircularDependencyError** when running your migrations as Django won’t be able to automatically break the dependency loop due to the dynamic dependency. If you see this error, you should break the loop by moving the models depended on by your user model into a second migration. (You can try making two normal models that have a `ForeignKey` to each other and seeing how `makemigrations` resolves that circular dependency if you want to see how it’s usually done.)

##### Reusable apps and `AUTH_USER_MODEL`

Reusable apps shouldn’t implement a custom user model. A project may use many apps, and **two reusable apps that implemented a custom user model couldn’t be used together**. If you need to store per user information in your app, use a [`ForeignKey`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.ForeignKey) or [`OneToOneField`](https://docs.djangoproject.com/en/2.2/ref/models/fields/#django.db.models.OneToOneField) to `settings.AUTH_USER_MODEL` as described below.

If you reference [`User`](https://docs.djangoproject.com/en/2.2/ref/contrib/auth/#django.contrib.auth.models.User) directly (for example, by referring to it in a foreign key), your code will not work in projects where the [`AUTH_USER_MODEL`](https://docs.djangoproject.com/en/2.2/ref/settings/#std:setting-AUTH_USER_MODEL) setting has been changed to a different user model.

##### Errors Encountered:

1. `CustomBackend` and `CustomUser` circular dependency
2. `authentication` should be a separate app
3. inherite from parent class can save a lot of efforts

