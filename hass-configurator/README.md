## The HASS Configurator is now a part of the official repository for Hass.IO add-ons at: https://github.com/home-assistant/hassio-addons. Get the configurator from there to ensure you are receiving the latest updates.

# HASS Configurator (Hass.IO add-on)
### Configuration UI for Home Assistant

Since there currently is no nice way to edit the yaml-files HASS is using through the HASS frontend, you can use this add-on to add a browser based file-editor to your Hass.IO installation. By default it will listen on port `3218` of the host Hass.IO is running on.  
More information and a standalone version for regular HASS installations can be found in the original repository at https://github.com/danielperna84/hass-configurator.

### Feature list:

- Web-Based editor to modify your files with syntax highlighting
- Upload and download files
- Stage and commit changes in Git repositories, create and switch between branches
- Lists of available triggers, events, entities, conditions and services. Selected element gets inserted into the editor at the last cursor position.
- Restart HASS directly with the click of a button (API-password required)
- SSL support
- Optional authentication and IP filtering for added security
- Direct links to HASS documentation and icons
- Execute shell commands
- Modified editor settings can be saved using [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

#### Screenshot of the configurator embedded into HASS:
![Screenshot](https://github.com/danielperna84/hass-configurator/blob/master/hass-poc-configurator.png)

_WARNING_: This tool allows you to browse your filesystem and modify files. So be careful which files you edit, or you might break critical parts of your system.

### Add-on Configuration

#### BASEPATH (string)
Use the `BASEPATH` parameter to specify the directory from which the configurator initially serves files. For regular Hass.IO installations the default of `/config` should be fine.
#### SSL_CERTIFICATE / SSL_KEY (string)
If you're using SSL, set the paths to your SSL files here. This is similar to the SSL setup you can do in HASS.
#### HASS_API (string)
The configurator fetches some data from your running HASS instance. If the API isn't available through the default URL, modify this variable to fix this.
#### HASS_API_PASSWORD (string)
If you plan on using API functions, you have to set your API password. Calling services of HASS is prohibited without authentication.
#### CREDENTIALS (string)
Set credentials in the form of `"username:password"` so access to your configuration is protected.
#### ALLOWED_NETWORKS (list)
Limit access to the configurator by adding allowed IP addresses / networks to the list, e.g `ALLOWED_NETWORKS = ["192.168.0.0/24", "172.16.47.23"]`
#### BANNED_IPS (list)
List of statically banned IP addresses, e.g. `BANNED_IPS = ["1.1.1.1", "2.2.2.2"]`
#### IGNORE_PATTERN (list)
Files and folders to ignore in the UI, e.g. `IGNORE_PATTERN = [".*", "*.log", "__pycache__"]`

__Note regarding `ALLOWED_NETWORKS` and `BANNED_IPS`:  
The way this is implemented works in the following order:

1. (Only if `CREDENTIALS` is set) Check credentials
  - Failure: Return error 420 (unless you try again without any authentication headers set, e.g. private tab of your browser)
  - Success: Continue
2. Check if client IP address is in `BANNED_IPS`
  - Yes: Return error 420
  - No: Continue
3. Check if client IP address is in `ALLOWED_NETWORKS`
  - No: Return error 420
  - Yes: Continue and display UI of configurator

### Embedding into HASS
HASS has the [panel_iframe](https://home-assistant.io/components/panel_iframe/) component. With this it is possible to embed the configurator directly into HASS, allowing you to modify your configuration through the HASS frontend.  
An example configuration would look like this:

```yaml
panel_iframe:
  configurator:
    title: Configurator
    icon: mdi:wrench
    url: http://hassio.local:3218
```
__IMPORTANT__: Be careful when setting up port forwarding to the configurator while embedding into HASS. If you don't restrict access by requiring authentication and / or blocking based on client IP addresses, your configuration will be exposed to the web!
