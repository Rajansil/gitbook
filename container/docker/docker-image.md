# Docker image

Here I am using AWS Virtual Machine to run docker. Following are the steps that I am doing in order to run a docker in VM:\


&#x20;In the development process I will build image in my local machine and push that image to Docker Hub. Now from docker hub We will clone that image to AWS VM.

&#x20;                       **Running Docker in Local Machine**

&#x20;**Step 1:**  Download and install Docker hub in your computer, signup using your email and sign in

**Step 2:**  Get the web app code from a developer and starting writing Dockerfile\
&#x20;    &#x20;

```
FROM node:14-alpine

WORKDIR /app

COPY package.json . 

RUN npm install

COPY . . 

EXPOSE 80

CMD ["node", "app.js"]
```

**Step 3:** Build this dockerfile and run it using following command:\
&#x20;       **docker build -t test .**

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-27 at 4.44.53 PM.png" alt=""><figcaption></figcaption></figure>

**Step 4:** Run that build image using folloeing command:\
&#x20;    **docker run -d --rm --name test1 -p 80:80 test,** \
****\
****Here **test** image is running in port 80 with the name **test1**

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-27 at 4.49.03 PM.png" alt=""><figcaption></figcaption></figure>

Here is the final output in local machine with **localhost:80**

&#x20;        &#x20;

<figure><img src="../../.gitbook/assets/Screen Shot 2022-12-28 at 9.54.10 PM.png" alt=""><figcaption></figcaption></figure>



****

****

****

****
