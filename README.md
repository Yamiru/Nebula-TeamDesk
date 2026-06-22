# Nebula TeamDesk

A lightweight bug tracker and feedback board. People sign in with Discord, post
reports, vote and comment. No database - everything is stored in plain JSON
files on your own server.

![PHP](https://img.shields.io/badge/PHP-8.3%2B-777BB4?logo=php&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-3da639)
![Storage](https://img.shields.io/badge/storage-JSON%20files-f59e0b)
![Auth](https://img.shields.io/badge/auth-Discord%20OAuth2-5865F2?logo=discord&logoColor=white)
![Server](https://img.shields.io/badge/server-nginx%20%7C%20Apache-009639?logo=nginx&logoColor=white)
![i18n](https://img.shields.io/badge/i18n-15%20languages-06b6d4)

## What it does

- Sign in with Discord (admin, moderator and user roles)
- Reports with categories, tags, priorities and a simple status flow
- Votes and comments on every report
- A public read-only board for signed-out visitors
- Discord webhook notifications for new reports and status changes
- 15 languages, switchable per person, plus an in-app translation editor
- Three themes and a responsive layout that works on phones

## Screenshots
![images](https://i.imgur.com/zCm23Ht.png)
![images](https://i.imgur.com/cMMBRTM.png)
![images](https://i.imgur.com/T2FJ4u6.png)
## Requirements

- PHP 8.x
- nginx or Apache
- A Discord application (for sign-in)
- A writable `data/` folder

## Setup

1. Upload the files to your web space (the web root or a subfolder).

2. Make the `data/` folder writable by PHP:

   ```bash
   chmod -R 775 data
   ```

   If your host runs PHP as a different user, set the owner to your web-server
   user as well (for example `chown -R www-data:www-data data`).

3. Create an app at the Discord Developer Portal. Copy the **Client ID** and
   **Client Secret**, and add a redirect that points at `auth.php`, for example:

   ```
   https://your-domain.tld/auth.php?do=callback
   ```

4. Paste those values into `config.php`.

5. Open the site and sign in. The first person to sign in becomes the admin.
   Everyone after that waits for approval.

## Block the `data/` folder (important)

The `data/` folder holds your users and reports. It must never be reachable from
the web.

- **Apache:** nothing to do. A deny-all `.htaccess` is already inside `data/`.
- **nginx:** `.htaccess` is ignored, so add this inside your `server { }` block:

  ```nginx
  location ^~ /data/ {
      deny all;
      return 404;
  }
  ```

  If the app sits in a subfolder, match that path instead, for example
  `location ^~ /teamdesk/data/`. Then reload nginx and check that
  `https://your-domain.tld/data/settings.json` returns 404.

Run the site over HTTPS so sign-in stays secure.

## Links

- Repository: https://github.com/Yamiru/Nebula-TeamDesk
- Issues: https://github.com/Yamiru/Nebula-TeamDesk/issues
- Author: https://yamiru.com


## Licence

Commercial licence. Copyright (c) 2026 Yamiru. One purchase covers one
licensee. You may use and edit it on your own projects, but not resell or share
it. See [LICENSE](LICENSE) for the full terms.
