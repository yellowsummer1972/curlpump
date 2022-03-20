## curlpump
pumping curl requests to stress test number of sites and find the breaking point
I've included a list known Russian gov and propaganda websites for you to "test"


### Requirements
- You need a linux host with docker 
- Have some understanding with bash
- Have VPN while running this to attack websites. (This is important: Don't run it if you don't have vpn)


### How to build and run 

#### Initialize a local repo
```
create a directory on local machine
mkdir curlpump
git clone  https://github.com/yellowsummer1972/curlpump.git curlpump
```

# build it
```
cd curlpump
./build.sh
```

# make sure the image is created
```
docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
curlpump     latest    111eb0b5e187   21 hours ago   16.2MB
alpine       latest    c059bfaa849c   3 months ago   5.59MB
```

# run it
```
create a local source so that you make change while it is running
mkdir ~/pump
docker run --name pumpit -v ~/pump:/pump -td curlpump
```

# view log
```
docker logs -f pumpit
```

# How it works
deliver.sh
```
For each url in sites.lst, deliver.sh creates a thread to continuously attack the website. 
Loop:
  check if site is responding (http code 200 - 399). 
    if yes, create 50 request to the url (burst_count value in pump.conf)
    if not, wait 5 seconds and check again
end loop
```

sites.lst
```
File ~/pump/sites.lst contains list of websites to pump, one per line.  You can remove a site simply by comment it out with a #, or delete it.  
Once the site is deleted or commented out, the script detects the change and kill the thread for that.  
Vice-versa, if you add a new site, deliver.sh detects the change on-the-fly and create a thread for it. 
```
pump.conf
```
File ~/pump/pump.conf contains run time values.  Changes to this conf will be applied on-the-fly
```
