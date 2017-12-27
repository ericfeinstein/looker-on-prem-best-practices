It is good hosting practice to harden your servers, meaning decrease the likelyhood of attack or vulnerability by locking down unnecessary services/ports. Here are the ports that Looker need.



Open these ports **inbound/outbound**

9999- The webserver of the machine  
19999- Used for the Looker API  
22- for SSH connection to git repository

**Outbound only:**  
 443- Looker License Server \(license.looker.com\)and Backups \(Amazon S3\)

587- Mail \(smtp.sendgrid.net\)  
3500- Looker authentication  
9000- Error communication \(sentry.looker.com\)

If you are operating a **cluster**, additional ports are needed:

1551- for node-to-node communication and cache sharing



To check a port is open:

`curl -k XXXX`

When you open a port or close it, this can be done at the following levels:

* Load Balancer
* Network/Firewall
* Server/ VM

It is best to open all the above ports at the server level, and redirect port 443\(https\) and port 80 \(http\) via URLs at the load balancer. I.e. API.looker.mycompany.com and looker.mycompany.com







