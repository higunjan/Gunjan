# Installing Multiple Node Apps on VPS Forever

###Install Forever (Globally)

Forever is to ensure your node app gets run forever. That means if it crashes, it will restart for you.
```
npm install forever -g
```

###Start with Forever

```
cd /var/www/nodeapp.com/       // Goto Your NodeApp path

# Install the packages, if you haven't
npm install

# Start forever, with minimum uptime 10 sec between launches if restart
# Assuming app.js is your application main script
forever start --spinSleepTime 10000 app.js
```
###Start with Forever, truly forever

Your app is not truly running forever.
If your VPS crash, then forever is not being started..
Hence, you need a ```cronjob``` and this ```starter.sh``` in your home folder. You can ```nano ~/starter.sh``` and copy the following:

```
#!/bin/sh

if [ $(ps -e -o uid,cmd | grep $UID | grep node | grep -v grep | wc -l | tr -s "\n") -eq 0 ]
then
        export PATH=/usr/local/bin:$PATH
        forever start --spinSleepTime 10000 --sourceDir /var/www/nodeapp.com app.js >> /var/www/nodeapp.com/log.txt 2>&1
fi
```

Then ```crontab -e```, and add the following:

```@reboot /path/to/starter.sh```

To know where your path to ```starter.sh``` is, use pwd at the directory.
That’s it!
To test your script is correct, you can:

```
forever stopall
# Make sure the script can be executed
chmod +x starter.sh
~/starter.sh
forever list
# Should see 1 running process
```

Or reboot your VPS to see that it gets truly run forever and ever :)


##Multiple Node Apps

To deploy multiple node apps, repeat (except for installing node, git, forever, crontab).

Assuming I have another app call nodeapp2, these are the changes:

```/var/www/nodeapp2.com/``` – contains the node app

```nodeapp2.com.conf``` – for nginx configuration (with a different port)

```starter.sh``` – add another line for the new node app to forever start

