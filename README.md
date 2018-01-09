# Mining

Mining rigs have most of the time stability issues and GPUs hang in the middle of process
without further computational process. Therefore it is important for the user to facilitate
the machine up-and-running for continuous and sustainable mining operation. Most of the 
solutions one can find is based on hardware [SimpleOS, RaspiReseter]. Nonetheless, one 
utilize Linux scripting magic for watching the failure and make the restart happen in a 
separate thread.

Create a shell script in SimpleMining OS:
$>cd /root/utils/
$>sudo nano watchdog_error.sh 
// You can add more error "Binary file (standard input) matches|{ADD MORE}"

'''
#!/bin/bash

export DISPLAY=:0

WDES='screen -x mine -X hardcopy /tmp/miner && cat /tmp/miner | grep . | tail -n 1 | grep "Binary file (standard input) matches" | wc -l'

if [ $WDES -ge 1 ]
then
 echo "ERROR, miner forced reboot"
 sudo shutdown -r now
fi
'''

- Activate Crontab
$> sudo crontab -e

- Add the line below (without quotes) to run every 15 mins to make sure the error is not existing.
"*/15 * * * * bash /root/utils/watchdog_error.sh"

- Start the crontab
/etc/init.d/cron start

- Add one line to make sure restart the cron after reboot and add this line before exit 0
/etc/init.d/cron start
