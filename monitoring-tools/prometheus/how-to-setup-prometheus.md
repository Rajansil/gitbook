# How to Setup Prometheus

Step by Step process to install Prometheus:

1. **Create a VM using terraform or from other ways,**
2. Create a gitlab reposirepository which include: Alert Manager script, Black box, nginx and prometheus including docker which has port number and so on.&#x20;
3. Once you create a VM we need to install a required software for prometheus, I am using ansible script to create a Prometheus. \
   I. Git clone the repo\
   II. ssl create\
   III. Install docker\
   IV. Docker-compose up
4. Now prometheus server is ready, Once we docker is up we will have following container running in our server:\
   &#x20;I. Alert Manager\
   &#x20;II. Prometheus\
   &#x20;III. Grafana\
   &#x20;IV. nginx\
   Now prometheus and grafana is up and running&#x20;
