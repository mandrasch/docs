# Installation

## Step 1: Download Craft

Craft can be downloaded with [Composer](#downloading-with-composer) or by [manually downloading](#downloading-an-archive-file-manually) a zip or tar.gz archive. The end result will be the same, so go with whichever route you feel more comfortable with.

### Downloading with Composer

::: tip
You should be running Composer 1.3.0 or later. You can find out your installed version of Composer by running `composer -V` from your terminal. If that outputs something lower than 1.3.0, run `composer self-update` to update your Composer installation.
:::

To create a new Craft project, run this command (substituting `path/to/my-project` with the path where Composer should create the project):

```bash
composer create-project craftcms/craft path/to/my-project
```

Composer will take a few minutes to load everything. Once it’s done you’ll see a success message:

![The success message shown after loading Craft with Composer](./images/installation-command-line.png)

### Downloading an Archive File Manually

Download the archive format you prefer to work with:

- [zip](https://craftcms.com/latest-v4.zip)
- [tar.gz](https://craftcms.com/latest-v4.tar.gz)

Extract the archive wherever you want your new Craft project to live.

::: tip
If you’re on macOS, be careful not to lose the hidden files in there (`.env`, `.env.example`, `.gitignore`, and `web/.htaccess`). You can press <kbd>Command</kbd>+<kbd>Shift</kbd>+<kbd>.</kbd> to toggle hidden file visibility in Finder.
:::

### Directory Structure

Once Craft’s files are in place, your project directory should have a directory structure like this:

```treeview
my-project/
├── config/
│   └── ...
├── modules/
│   └── ...
├── storage/
│   └── ...
├── templates/
│   └── ...
├── vendor/
│   └── ...
├── web/
│   └── ...
├── .env
├── .env.example
├── .gitignore
├── composer.json
├── composer.lock
└── craft
```

::: tip
The `web/` folder represents your site’s web root, and it can be renamed to whatever you want (`www/`, `public/`, `public_html/`, etc.).
:::

::: tip
See [Moving Craft’s Files Below the Web Root](https://craftcms.com/knowledge-base/moving-craft-files) if your hosting setup does not allow Craft’s files to exist outside the web root.
:::

See the [Directory Structure](directory-structure.md) page to learn what these folders and files are for and how you can customize them.

## Step 2: Set the File Permissions

::: tip
If you used Composer to download Craft, you can probably safely skip this step.
:::

For Craft to run properly, PHP needs to be able to write to the following places:

- `.env`
- `composer.json`
- `composer.lock`
- `config/license.key`
- `config/project/*`
- `storage/*`
- `vendor/*`
- `web/cpresources/*`

The exact permissions you should be setting depends on the relationship between the system user that runs PHP and whoever owns the folders and files.

- If they’re the same user, use `744`.
- If they’re in the same group, use `774`.
- If you’re not sure and enjoy life on the edge, use `777`.

::: warning HEY IIS FANS
Make sure your site’s AppPool account has write permissions to these folders and files.
:::

## Step 3: Set a Security Key

::: tip
If you used Composer to download Craft, you can probably safely skip this step.
:::

Each Craft project should have a unique security key, which is shared between each of the environments that the project is installed on.

You can generate and assign the key [manually](#set-the-key-manually), or have Craft do it for you with a [terminal command](#set-the-key-from-your-terminal).

### Set the Key Manually

First generate a cryptographically secure key, ideally using a password generator like [1Password](https://1password.com/password-generator/). (There’s no length limit.)

Then open up your `.env` file (you may need to use an app like [Transmit](https://panic.com/transmit/) to do this if you’re running macOS), and find this line:

    CRAFT_SECURITY_KEY=""

Paste your security key inside the quotes and save the file.

### Set the Key from Your Terminal

In your terminal, go to your project’s root directory and run the following command:

```bash
php craft setup/security-key
```

## Step 4: Create a Database

Next up, you need to create a database for your Craft project. Craft 4 supports both MySQL 5.7.8+ and PostgreSQL PostgreSQL 10+.

If you’re given a choice, we recommend the following database settings in most cases:

- **MySQL**

  - Default Character Set: `utf8`
  - Default Collation: `utf8_unicode_ci`

- **PostgreSQL**
  - Character Set: `UTF8`

## Step 5: Set up the Web Server

Create a new web server to host your Craft project. Its document root (or “web root”) should point to your `web/` directory (or whatever you’ve renamed it to).

You’ll also need to update your system’s `hosts` file so requests to your chosen hostname (e.g. `my-project.tld`) should be routed locally.

- **macOS/Linux/Unix**: `/etc/hosts`
- **Windows**: `\Windows\System32\drivers\etc\hosts`

::: tip
Some local development tools such as [DDEV](https://ddev.com/) will update your `hosts` file automatically for you.
:::

You can test whether you set everything up correctly by pointing your web browser to `https://<Hostname>/index.php?p=admin/install` (substituting `<Hostname>` with your web server’s hostname). If Craft’s Setup Wizard is shown, the hostname is correctly resolving to your Craft installation.

::: tip
We recommend using the `.test` TLD for local development, and specifically **not** `.local` on macOS since [conflicts with Bonjour can lead to performance issues](https://help.rm.com/technicalarticle.asp?cref=tec3015691).
:::

## Step 6: Run the Setup Wizard

Finally, it’s time to run Craft’s Setup Wizard from either your [terminal](#terminal-setup) or your [web browser](#web-browser-setup).

::: tip
If you used `composer create-project` earlier and chose to continue setup there, you can head straight to `https://my-project.tld/admin`.
:::

### Terminal Setup

In your terminal, go to your project’s root directory and run the following command to kick off the Setup Wizard:

```bash
php craft setup
```

The command will ask a few questions about your database connection and kick off Craft’s installer. Once it’s done, you should be able to access your new Craft site from your web browser.

### Web Browser Setup

In your web browser, go to `https://my-project.tld/index.php?p=admin/install` (substituting `my-project.tld` with your web server’s hostname). If you’ve done everything right so far, you should be greeted by Craft’s Setup Wizard:

<BrowserShot url="https://my-project.tld/admin/install" :link="false">
<img src="./images/installation-step-0.png" alt="Craft Installation Screen">
</BrowserShot>

The first step of the installer is to accept the [license agreement](https://craftcms.com/license). Scroll down through the agreement (reading it all, of course) and press **Got it** to accept:

<BrowserShot url="https://my-project.tld/admin/install" :link="false">
<img src="./images/installation-step-1.png" alt="Craft Installation License Agreement">
</BrowserShot>

The second step is to enter your database connection information:

::: tip
If the Setup Wizard skips this step, it’s because Craft is already able to connect to your database.
:::

<BrowserShot url="https://my-project.tld/admin/install" :link="false">
<img src="./images/installation-step-2.png" alt="Craft Installation Database Connection Information">
</BrowserShot>

The third step is to create an admin account. Don’t be one of _those people_—be sure to pick a strong password:

<BrowserShot url="https://my-project.tld/admin/install" :link="false">
<img src="./images/installation-step-3.png" alt="Craft Installation Create User Account">
</BrowserShot>

The final step is to define your System Name, Base URL, and Language:

<BrowserShot url="https://my-project.tld/admin/install" :link="false">
<img src="./images/installation-step-4.png" alt="Craft Installation System Settings">
</BrowserShot>

Press **Finish up** to complete the setup process. A few seconds later, you should have a working Craft install!

If it was successful, Craft will redirect your browser to the control panel:

<BrowserShot url="https://my-project.tld/admin/dashboard" :link="false">
<img src="./images/installation-step-5.png" alt="Craft Installation Complete">
</BrowserShot>

Congratulations, you’ve installed Craft!

Now build something incredible.

## Troubleshooting

See the [Troubleshooting a Failed Craft Installation](https://craftcms.com/knowledge-base/troubleshooting-failed-installation) Knowledge Base article if something went wrong along the way.
