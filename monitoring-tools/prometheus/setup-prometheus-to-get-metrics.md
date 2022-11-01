# Setup Prometheus to get metrics

1.  Update a node file in prometheus repo with a port number in this format:\


    ```
    - targets:
        - HostName:portNumber
      labels:
        service: name
    ```
2. Once you add your host name we need to git push to a gitlab and git pull in prometheus server
3. To check login with prometheus IP adress and its port number to see in Graphic mode where you can see your node.

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-31 at 6.04.12 PM.png" alt=""><figcaption></figcaption></figure>
