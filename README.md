# Netdata zoneminder plugin
Plugin to monitor your zoneminder cameras. Includes some alarms you can use to monitor your cameras and zoneminder's availability. 


## Requirements 
1. netdata: https://github.com/netdata/netdata 
    To install netdata on Linux systems:
    ```sh
    # make sure you run `bash` for your shell
    bash

    # install Netdata directly from GitHub source
    bash <(curl -Ss https://my-netdata.io/kickstart.sh)
    ```
1. requests python library: https://requests.readthedocs.io/en/master/
    To install requests library run:
    ```
    sudo -H python -m pip install requests
    ```
1. PyJWT python library: https://pyjwt.readthedocs.io/en/latest/
    To install PyJWT library run:
    ```
    sudo -H python -m pip install pyjwt
    ```
1. zoneminder: https://zoneminder.com/
    You need to make sure zoneminder's api is enabled: https://zoneminder.readthedocs.io/en/stable/api.html#enabling-api

## Installation
1. Clone repository and cd to repo directory:
    ```
    git clone https://github.com/pepe386/netdata-zoneminder
    cd netdata-zoneminder
    ```
1. Edit zoneminder.conf with your zoneminder url, user and password.
1. Run installation script:
    ```
    chmod +x install.sh
    ./install.sh
    ```
1. restart netdata
    ```
    service netdata restart
    ```
    If the above command fails try:
    ```
    systemctl restart netdata
    ```
    or:
    ```
    /etc/init.d/netdata restart
    ```

For other installation options run:
```
./install.sh -h
```
Installation script attempts to install python plugin to /usr/libexec/netdata/python.d directory. It also assumes netdata configuration directory is /etc/netdata. This paths might be different depending on your netdata installation, refer to https://learn.netdata.cloud/docs/agent/daemon/config
To specify different plugin and configuration directories check "./install.sh -h" output for other installation options. 

## Updates
To update just cd to repo directory and run:
```
git pull
./install.sh
```

## Charts
This module generates 4 netdata charts:
1. FPS of each camera 
1. Bandwidth of each camera
1. Events of each camera
1. Disk usage of all saved events 

This is how charts look in netdata dashboard:
![alt text](https://raw.githubusercontent.com/pepe386/netdata-zoneminder/master/images/zm_netdata_1.png)
![alt text](https://raw.githubusercontent.com/pepe386/netdata-zoneminder/master/images/zm_netdata_2.png)

## Alarms
Installation script by default installs 2 netdata alarms:
1. Average FPS of last 5 minutes: by default this alarm sets "warn" level when average fps is less than 5 in the last 5 minutes, and "critical" level when average fps is less than 2 in the last 5 minutes. 
1. Time since last successful data collection: "warn" when no data is collected for last 5 minutes, and "critical" when no data is collected for last 15 minutes. 

To restart netdata alarms you can run:
```
killall -USR2 netdata
```

For instructions on setting up netdata email alerts see: https://learn.netdata.cloud/docs/agent/health/notifications/email