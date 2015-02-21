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
