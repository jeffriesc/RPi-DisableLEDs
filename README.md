# RPi-DisableLEDs
This repo contains a service file to disable the LEDs on the Raspberry Pi

Here's how you can disable the 'active'-LED and the power-LED on a rasbperry pi on your own:

If you want to know which LED is which, run: `echo 0 | sudo tee /sys/class/leds/led1/brightness > /dev/null`. Which ever LED is still on is `led0`. In my case the `led1` is the red powered LED and `led0` is the green activity LED. This will be for most, if not all running a Raspberry Pi 3B+ or newer. 

Now to automate this on start up

Create the service file with: `sudo nano /etc/systemd/system/disable-led.service` paste the following:

```
[Unit]
Description=Disables the power-LED and active-LED

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=sh -c "echo 0 | sudo tee /sys/class/leds/led1/brightness > /dev/null && echo 0 | sudo tee /sys/class/leds/led0/brightness"
ExecStop=sh -c "echo 1 | sudo tee /sys/class/leds/led1/brightness > /dev/null && echo 1 | sudo tee /sys/class/leds/led0/brightness"

[Install]
WantedBy=multi-user.target
```
  
Use ctrl+O and then press enter to write the file and ctrl+X to close out of nano

Note: you can also remove `&& echo 0 | sudo tee /sys/class/leds/led0/brightness` to disable only the active-LED (or vice versa)

Test the service with: `sudo systemctl start disable-led.service`
Stop the the service with: `sudo systemctl stop disable-led.service`
Use `sudo systemctl enable disable-led.service` to enable on startup
