# mkwebapp

**mkwebapp** is a utility script for [Termux](https://termux.com/) that allows you to create standalone `.deb` packages for any website. These packages install launchers that open your favorite web apps directly, integrating them into your Termux environment or Termux:X11 desktop.

Perfect for turning web services like ProtonMail, WhatsApp Web, or Google Docs into "native" feeling applications.

![Banner](https://img.shields.io/badge/Termux-Android-black?style=for-the-badge&logo=termux)
![Shell](https://img.shields.io/badge/Shell-Bash-green?style=for-the-badge)

## Features

*   **Instant Apps:** Convert any URL into an installable Termux package.
*   **Desktop Integration:** Automatically generates `.desktop` files for application menus (e.g., XFCE4 in Termux:X11).
*   **Icon Handling:**
    *   Supports custom local PNG icons.
    *   Automatically fetches high-quality favicons via Google's Favicon service if no icon is provided.
*   **Flexible Browser Support:** Defaults to standard browsers (Firefox, Chromium) but allows specifying a custom browser command.
*   **Clean Build:** Uses temporary directories and sanitizes filenames to ensure a clean package build.

## Requirements

This script runs on a standard Termux installation. **No additional packages need to be installed manually** to use the builder.

## Installation

1.  Save the script to your device (e.g., `mkwebapp.sh`).
2.  Make it executable:
    ```bash
    chmod +x mkwebapp.sh
    ```
3.  (Optional) Move it to your path for system-wide access:
    ```bash
    mv mkwebapp.sh $PREFIX/bin/mkwebapp 
    ```

## Usage

### Basic Syntax

```bash
./mkwebapp.sh --url <URL> --name <AppName> [options]
```

### Arguments

| Argument | Required | Description |
|----------|----------|-------------|
| `--url <URL>` | Yes | The website URL the app should open. |
| `--name <APPNAME>` | Yes | The display name of the application. |

### Options

| Option | Description |
|--------|-------------|
| `--icon <FILE>` | Path to a local PNG image to use as the icon. |
| `--browser <CMD>` | Specific browser command to launch (e.g., `firefox`, `chromium`). |
| `--maintainer <STR>` | Override the default maintainer string in the package metadata. |
| `--help` | Show help message and exit. |

## Examples

### 1. Create a Web App with Auto-Fetched Icon
This creates a package named `web-youtube.deb` that launches YouTube. It will automatically attempt to download the YouTube icon.

```bash
mkwebapp --url "https://youtube.com" --name "YouTube"
```

### 2. Create a Web App with a Custom Icon
If you have a specific logo file saved locally:

```bash
mkwebapp \
  --url "https://chatgpt.com" \
  --name "ChatGPT" \
  --icon "/sdcard/Download/gpt-icon.png"
```

### 3. Force a Specific Browser
Launch the site specifically using Firefox (useful if you have multiple browsers installed):

```bash
mkwebapp \
  --url "https://proton.me/mail" \
  --name "ProtonMail" \
  --browser "firefox --new-window"
```

## Installing the Generated Package

Once the script finishes, you will have a `.deb` file (e.g., `web-youtube_1.0.0_all.deb`) in your current directory.

To install it:

```bash
dpkg -i web-youtube_1.0.0_all.deb
```

**After installation:**
1.  **CLI:** You can run the app from the terminal using the sanitized name (e.g., `youtube`).
2.  **GUI (Termux:X11):** The app will appear in your desktop application menu under the "Network" category.

## How It Works

1.  **Sanitization:** The script takes your `--name` and sanitizes it to create a safe package name (e.g., "My App!" becomes `web-my-app`).
2.  **Directory Structure:** It creates a temporary directory mimicking the Termux file system (`$PREFIX/bin`, `$PREFIX/share/applications`, etc.).
3.  **Launcher Script:** Generates a bash script at `$PREFIX/bin/<name>` that checks for installed browsers (Firefox, Chromium, or falls back to `xdg-open`) and launches the URL.
4.  **Desktop Entry:** Creates a standard `.desktop` file so desktop environments can display the app.
5.  **Packaging:** Uses `dpkg-deb` to compile the directory into a `.deb` package.

## Credits

Maintained by **Alienkrishn [Anon4You]**

---

*Enjoy turning the web into apps!*
