# ufw-block-http-except-cloudflare
This is a shell script that will allow your to block all http and https traffic
from the internet and allow only cloudflares IP Block and SSH Access to Port 22 from Anywhere. It will then generate a ufw allow command
for all of Cloudflares IP Block.

This script is designed for debian based linux servers using the UFW (Uncomplicated Firewall)

Using this script properly will essentially give your server the best DDoS protection as it
will force all http/https traffic through cloudflare.

# Usage:

1. Pull up a terminal or SSH into the target server.

2. Logon as root

<code>sudo -i</code>

3. Download the installer script.

<code>wget https://github.com/bsidlify4u/ufw-block-http-except-cloudflare/blob/master/cloudflare-ufw.sh</code>

4. Make the script executable

<code>chmod +x cloudflare-ufw.sh</code>

5. Run the script.

<code>./cloudflare-ufw.sh</code>

6. Dont forget to remove your firewall rules that allows "from all http and https".

7. Test your firewall rules.

8. Setup a cronjob to run the script daily/weekly if you choose.

## create cron job with this command:

**15 3 * * * /bin/sh /home/miteldream/cloudflare-ufw.sh && /usr/bin/python3 /home/miteldream/clean_ufw.py >> /var/log/cloudflare-ufw.log 2>&1**

**What it means**
* `15 3 * * *` – run at **03:15 AM every day** (minute 15, hour 3, any day-of-month, any month, any weekday).
* First command: `/bin/sh /home/miteldream/cloudflare-ufw.sh`
  * Typically downloads the latest Cloudflare IP ranges and inserts them into UFW.
* `&&` ensures the second command only runs if the first succeeds.
* Second command: `/usr/bin/python3 /home/miteldream/clean_ufw.py --yes`
  * Executes this cleanup script in non-interactive mode, deleting any leftover `Anywhere` rules.
* `>> /var/log/cloudflare-ufw.log 2>&1` appends both stdout and stderr from the *whole pipeline* to a log file for auditing.
