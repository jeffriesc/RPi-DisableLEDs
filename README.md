# RPi-DisableLEDs
This repo contains a service file to disable the LEDs on the Raspberry Pi

Here's how you can disable the 'active'-LED and the power-LED on a rasbperry pi (and probably on other boards too):

Manual

Initially you have to find the correct number of the desired LED.

Run: echo 0 | sudo tee /sys/class/leds/led1/brightness > /dev/null

where you can try different led1s
On a RPI 4 led1 is the power-LED and led0 is the 'active' LED.

systemd service (to disable the LEDs at startup)
Create a service file at: /etc/systemd/system/disable-led.service with the following content:

'''[Unit]
Description=Disables the power-LED and active-LED

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=sh -c "echo 0 | sudo tee /sys/class/leds/led1/brightness > /dev/null && echo 0 | sudo tee /sys/class/leds/led0/brightness"
ExecStop=sh -c "echo 1 | sudo tee /sys/class/leds/led1/brightness > /dev/null && echo 1 | sudo tee /sys/class/leds/led0/brightness"

[Install]
WantedBy=multi-user.target'''

you can also remove && echo 0 | sudo tee /sys/class/leds/led0/brightness to disable only the power-LED (or vice versa)
enable it at startup sudo systemctl enable disable-led.service
to start immidiately: sudo systemctl start disable-led.service
to enable the LEDs again: sudo systemctl stop disable-led.service
