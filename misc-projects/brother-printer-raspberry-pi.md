# Make Brother Label Printer Wireless using Raspberry Pi

## 1. Prepare the Raspberry Pi
Burn Raspbian Lite on an SD card using RPi imager
- ssh access, public keys
- wifi username/password

Boot raspberry pi
- ssh into it, `apt update`, `apt upgrade`
- `lsusb -vvv` to confirm printer shows up
- also `ls /dev/usb/` to see which port - usually `lp0`

To access printer ports, add the current user to `lp` group
```
sudo usermod -G lp -a $USER
```

## 3. Install dependencies
`sudo apt install` these packages 
```
python3-setuptools
python3-pip
libopenjp2-7-dev
git
fontconfig
# libtiff5 is not available anymore
```

libtiff6 or libtiff5-dev are the alternatives, worked for me with libtiff5-dev

## 4. Prepare venv
Create new venv at `$HOME/venv` and binaries will be in `$HOME/venv/bin`
```
python3 -m venv ~/venv
```
Add the venv path to .profile file so that `brother_ql` can be run directly
```
if [ -d "$HOME/venv/bin" ] ; then
    PATH="$HOME/venv/bin:$PATH"
fi
```
Load the profile with
```
export .profile
```

## 6. Install brother_ql
Github page: https://github.com/pklaus/brother_ql
```
sudo ~/venv/bin/pip3 install brother_ql
```
brother_ql can be used standalone via cli, but web service makes it much better. 

Some useful commands
```
brother_ql_info list-models
brother_ql_info list-label-sizes
```

## 7. Setup the web service
Clone the repository to $HOME
```
git clone https://github.com/pklaus/brother_ql_web.git
```

Install requirements, they're in `requirements.txt` file and can be installed via
```
sudo ~/venv/bin/pip3 install -r requirements.txt
```
But this doesn't work with newer version of bottle, so install the requirements individually, choosing older version of bottle. 
```
sudo ~/venv/bin/pip3 install bottle==8.1.2
sudo ~/venv/bin/pip3 install jinja2
```

Edit `config.json` file 
```
{
  "SERVER": {
    "PORT": 80,
    "HOST": "",
    "LOGLEVEL": "WARNING",
    "ADDITIONAL_FONT_FOLDER": false
  },
  "PRINTER": {
    "MODEL": "QL-800",
    "PRINTER": "file:///dev/usb/lp0"
  },
  "LABEL": {
    "DEFAULT_SIZE": "62",
    "DEFAULT_ORIENTATION": "standard",
    "DEFAULT_FONT_SIZE": 70,
    "DEFAULT_FONTS": [
      {"family": "Minion Pro",      "style": "Semibold"},
      {"family": "Linux Libertine", "style": "Regular"},
      {"family": "DejaVu Serif",    "style": "Book"}
    ]
  },
  "WEBSITE": {
    "HTML_TITLE": "Label Designer",
    "PAGE_TITLE": "Brother QL-800 Label Designer",
    "PAGE_HEADLINE": "Design your label and print it…"
  }
}
```

Trial run, looks like you have to be inside that folder to be able to run it
```
cd ~/brother_ql_web
sudo ~/venv/bin/python3 brother_ql_web.py
```

Login from web browser http://10.10.12.34/ and confirm everythig works. 

## 8. Autostart
Add task to crontab to run as root using `sudo nano /etc/crontab`
```
@reboot    root    cd /home/shree/web_app/brother_ql_web; /home/shree/venv/bin/python3 brother_ql_web.py >> /home/shree/web_app/brother_ql_web/logfile.log 2>&1 &
```
This also saves a log. 

```
@reboot    root    cd /home/shree/web_app/brother_ql_web; /home/shree/venv/bin/python3 brother_ql_web.py > /dev/null &
```
If log is not needed. 

## 9. Reverse Proxy
Add to `dynamic.yml`
```
http:
  routers:
    labelprinter:
      rule: 'Host(`labelprinter.{{ MY_DOMAIN }}`)'
      service: labelprinter
      entryPoints:
        - websecure
      tls:
        certresolver: cloudflare

  services:
    labelprinter:
      loadBalancer:
        servers:
          - url: "http://10.10.100.3"
```
It's now reachable at https://labelprinter.mydomain/

## 10. Add printer to Grocy
