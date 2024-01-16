---
title: How to setup and use un/authenticated proxy with apt and pip
date: 2024-01-11 14:00:00 +1000
categories: [tips,tricks,network]
tags: [hacking, tips, tricks,proxy,pip,apt,kali,parrot, linux]  
comments: true
image:
  path: /assets/img/post/how-to-setup-proxy-apt-pip.jpg
---

# Setting Up Un/Authenticated Proxy for APT and PIP

If you're working in an environment that requires a proxy, setting it up for package managers like APT (Advanced Package Tool) and PIP (Python Package Installer) is essential. Here's a step-by-step guide:

## APT Configuration
### Unauthenticated
1. Open or create if the APT configuration file doesn't exist using your preferred text editor. For example:

    ```bash
    sudo nano /etc/apt/apt.conf
    ```

2. Add the following lines to specify the proxy:

    ```bash
    Acquire {
    HTTP::proxy "http://proxy_server:port/";
    HTTPS::proxy "http://proxy_server:port/";
    }
    ```
### Authenticated
**If your proxy supports authentication** and requires a username/password for login, use: {: .prompt-info }

```bash
Acquire {
http::Proxy "http://user:password@proxy_server:port/";
https::Proxy "http://user:password@proxy_server:port/";
    }
```

**Note**: Replace `your_proxy_server` and `proxy_port` with your actual proxy server address and port as well as `user` and `password` if authetication is needed. {: .prompt-info }

Save and exit the editor and test the APT connection:

 ```bash
 sudo apt update
 ```

## PIP Configuration
### Unauthenticated

you can use the following commands in the terminal to set temporal proxy configuration:

```bash
set HTTP_PROXY=http://proxy_server:port
set HTTPS_PROXY=https://proxy_server:port
```
used `unset` to remove temporal proxy  configuration.

or to make the confiuration permanent create a file `pip.conf`

```bash
 nano /etc/pip.conf
```
and add the following lines, replacing `proxy_url` and `port` with your actual proxy URL and port:

```bash
[global]
proxy = http://proxy_url:port/
```
Finally, Save the changes and exit the text editor.

### Authenticated
You can use any of these commands for HTTP or HTTPS with your proxy details:

```bash
pip install --proxy http://username:password@proxy_server:port package_name

pip install --proxy https://username:password@proxy_server:port package_name
```
or Add below text at the end of `~/.bashrc` or `~/.zshrc`:

```bash
# PIP proxy configuration
HTTP_PROXY=http://username:password@proxyserver:port
HTTP_PROXY=https://username:password@proxyserver:port
```
**Note:** Don't forget to export the variables after setting them, to make them available to the shell session. {: .prompt-info }

```bash
Export HTTP_PROXY
```

Again, replace `your_proxy_server` and `proxy_port` with your actual proxy server address and port. {: .prompt-info }

Test the PIP connection:

```bash
pip install package_name
```

Replace `package_name` with any package you want to install.

Now, you've successfully set up a proxy for both APT and PIP, allowing you to work seamlessly in environments with proxy requirements.
