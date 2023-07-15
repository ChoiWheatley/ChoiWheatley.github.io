---
description:
aliases: 
tags: 
created: 2023-06-02T11:25:11
updated: 2023-07-15T21:30:20
title: django model.ForeignKey.on_delete
---
https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.ForeignKey.on_delete

The possible values for [`on_delete`](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.ForeignKey.on_delete "django.db.models.ForeignKey.on_delete") are found in [`django.db.models`](https://docs.djangoproject.com/en/4.2/topics/db/models/#module-django.db.models "django.db.models"):

- `CASCADE`[¶](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.CASCADE "Permalink to this definition")
    
    Cascade deletes. Django emulates the behavior of the SQL constraint ON DELETE CASCADE and also deletes the object containing the ForeignKey.
    
    [`Model.delete()`](https://docs.djangoproject.com/en/4.2/ref/models/instances/#django.db.models.Model.delete "django.db.models.Model.delete") isn’t called on related models, but the [`pre_delete`](https://docs.djangoproject.com/en/4.2/ref/signals/#django.db.models.signals.pre_delete "django.db.models.signals.pre_delete") and [`post_delete`](https://docs.djangoproject.com/en/4.2/ref/signals/#django.db.models.signals.post_delete "django.db.models.signals.post_delete") signals are sent for all deleted objects.
    
- `PROTECT`[¶](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.PROTECT "Permalink to this definition")
    
    Prevent deletion of the referenced object by raising [`ProtectedError`](https://docs.djangoproject.com/en/4.2/ref/exceptions/#django.db.models.ProtectedError "django.db.models.ProtectedError"), a subclass of [`django.db.IntegrityError`](https://docs.djangoproject.com/en/4.2/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError").
    
- `RESTRICT`[¶](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.RESTRICT "Permalink to this definition")
    
    Prevent deletion of the referenced object by raising [`RestrictedError`](https://docs.djangoproject.com/en/4.2/ref/exceptions/#django.db.models.RestrictedError "django.db.models.RestrictedError") (a subclass of [`django.db.IntegrityError`](https://docs.djangoproject.com/en/4.2/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError")). Unlike [`PROTECT`](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.PROTECT "django.db.models.PROTECT"), deletion of the referenced object is allowed if it also references a different object that is being deleted in the same operation, but via a [`CASCADE`](https://docs.djangoproject.com/en/4.2/ref/models/fields/#django.db.models.CASCADE "django.db.models.CASCADE") relationship.
