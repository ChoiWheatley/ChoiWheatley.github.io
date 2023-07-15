---
description:
aliases: 
tags: 
created: 2023-06-07T11:03:42
updated: 2023-07-15T21:30:20
title: django migration 순서 꼬인경우 {GPT}
---
The error message suggests that there is an inconsistency in the migration history. The `admin.0001_initial` migration is applied before its dependency `users.0001_initial` on the database.

To resolve this issue, you can try the following steps:

1. Make sure you have a backup of your database before proceeding.

2. Delete the migration files for the `admin` and `users` apps, except for the `__init__.py` file, which should be kept.

3. Open the `django_migrations` table in your database and delete the records for the `admin` and `users` apps.

4. Run the following command to fake the initial migration for the `users` app:

   ```bash
   python manage.py migrate users --fake-initial
   ```

   This command will mark the initial migration for the `users` app as applied without actually running the migration.

5. Run the following command to apply the migrations for the `users` app:

   ```bash
   python manage.py migrate users
   ```

   This will apply any remaining migrations for the `users` app.

6. Finally, run the following command to apply the migrations for the `admin` app:

   ```bash
   python manage.py migrate admin
   ```

   This will apply any remaining migrations for the `admin` app.

By following these steps, you should be able to resolve the inconsistency in the migration history and apply the migrations for both the `users` and `admin` apps successfully.
