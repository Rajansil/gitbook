# Setup Git for CICD

**Step 1:** Create a account in Gitlab using email address. We can use free as well. Provide a repository name with permission.&#x20;

<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-05 at 5.19.13 PM.png" alt=""><figcaption></figcaption></figure>

**Step 2**: Clone the repository with SSH which helps to edit the code easily using different IDE. \


```
git clone git@gitlab.com......
```

**Step 3:**  Open the folder in VS code and starting writng a script in .gitlab-ci.yml.\
In the following script I am using docker to build a CI/CD pipeline. In order to use a docker we need to have docker in docker hub and create a Dockerfile folder. Include image with version and mention port number in dockerfile.

<pre><code><strong>Stages:
</strong><strong>    - test
</strong><strong>    - build
</strong><strong>    - stg
</strong><strong>    - production
</strong><strong>    
</strong><strong>test artifacts:
</strong>    stage: test
    image: python:3.9-slim-buster
    before_script:
        - apt-get update &#x26;&#x26; apt-get install make
    script:
        - mkdir /data
        - cd /data
        - touch rajan.txt
        -  echo "Rajan Silwal" > rajan.txt


build the car:
    stage: build
    image: docker:20.10
    services:
        - docker::20.10-dind
    variables:
        DOCKER_TLS_CERTFIR: "/certs"
    before_script:
        - docker login -u $REGISTRY_USER -p $REGISTRY_PASS
    script:
        - docker build -f Dockerfile -t rajansilwal04/cicd:python-app-1.0 . 
        - docker push rajansilwal04/cicd:python-app-1.0 

testing staging:
    stage: stg
    image: python:3.9-slim-buster
    before_script:
        - apt-get update &#x26;&#x26; apt-get install make
    script:
        - mkdir /data
        - cd /data
        - touch rajan.txt
        -  echo "Rajan Silwal" > rajan.txt

Production:
    stage: production
    image: python:3.9-slim-buster
    before_script:
        - apt-get update &#x26;&#x26; apt-get install make
    script:
        - mkdir /data
        - cd /data
        - touch rajan.txt
        -  echo "Rajan Silwal" > rajan.txt</code></pre>

**Step 4:** Once you done with your code you can commit and push into a git. It will automatically start running CICD pipeline in gitlab.\
\
We can see the pipeline under CICD menu.\


<figure><img src="../../.gitbook/assets/Screen Shot 2022-11-05 at 5.17.02 PM.png" alt=""><figcaption></figcaption></figure>

If you want to see the log of pipeline, We can see that logs under jobs.
