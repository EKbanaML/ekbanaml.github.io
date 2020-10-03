---
title: "Using Pm2 To Serve Python Application"
excerpt_separator: "<!--more-->"
last_modified_at: 2020-10-02T16:20:02-05:00
categories:
  - Python
tags:
  - python
header:
  image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
author: vaghawan
---
## Using Pm2 To Serve Python Application 

PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks. [1]  It makes the deployment of an application/script simple and easy, also the development process by giving us an easy access of status and logs.

Normally, we were using it for serving the node js script. Normally when we serve an application with pm2 e.g `pm2 start app.js`  we will be able to see the application being running in daemon mode.  

![start pm2 app]({{ site.url }}{{ site.baseurl }}/assets/images/using-pm2/pm2-start-demo.png)


In the above screenshot, the file names are different, I hit two commands : 

``` 
pm2 start server.js 
```

Let us ignore the first process as of now, because that’s the python process which we’re going to discuss later. 
Now to see the logs of the ‘server.js’ application, we can simply do

```
pm2 logs server
```
Or `pm2 logs` to watch the log of all the processes currently running under pm2. 


### Now let us revisit few basic commands: 

1. To restart : `pm2 restart appname` to restart a single application or `pm2 restart` to restart all the application at once. 
2. To Reload : `pm2 reload appname` to reload a single application or `pm2 reload` to reload all the application at once. 
3. To delete : `pm2 delete appname` to delete a single application or `pm2 delete all` to delete all application. 

## For Python processes: 
For python processes as well, you don’t have to do anything extra, you can simply do : 

```
pm2 start pythonfilename.py
```

Replace pythonfilename with your desired python filename. Make sure you’re in the same directory where the file is available. But this way, pm2 seems to pick the default python of the system which is mostly python2.7, but we can direct pm2 to use python3 by explicitly providing an interpretar argument like this: 

```
 
pm2 start detect_and_send_to_pi.py --interpreter /usr/bin/python3

```

This way, pm2 will pick the python3 to run the process. 

### System Service deployment/ Keeping Processes Alive at Server Reboot
Normally, there is the hassle of creating systemd service when it comes to creating a stable deployment that is untouched by the frequent system reboots. But pm2 makes it really easy to create services of the pm2 processes such that those processes are automatically up when the system reboots, or automatically reloaded when the application crashes.

```
pm2 ls 
```

This will list the processes, if you do not want some processes in the startup, you can delete them using   `pm2 delete processname`

![pm2 list app]({{ site.url }}{{ site.baseurl }}/assets/images/using-pm2/pm2-list-app.png)



Now hit: 

```
pm2 startup # This will output some commands like this: 
```
![pm2 list app]({{ site.url }}{{ site.baseurl }}/assets/images/using-pm2/pm2-startup.png)


Now copy and paste the command in the terminal. Now pm2 initiates the auto-initialization for us in the server restart. 
Optionally we can also choose which processes/application to choose for auto restart: 
We can do `pm2 save` to save all the currently initiated processes with 

```
pm2 save 
```

This will create a dump file and PM2 will automatically spawn this application at reboot. [2] Now even if you reboot your system, the pm2 processes will be up and running nicely. 

The article is inspired by [2]  the only new thing was the --interpreter command for python process. 


## Reference:
1. [https://github.com/Unitech/pm2 ](https://github.com/Unitech/pm2 )
2. [https://blog.pm2.io/2018-09-19/Manage-Python-Processes/](https://blog.pm2.io/2018-09-19/Manage-Python-Processes/)
3. [https://pm2.io/runtime/](https://pm2.io/runtime/)
