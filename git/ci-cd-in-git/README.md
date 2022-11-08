# 🐇 CI/CD in Git

CI/CD is the top-level component of continuous integration, delivery, and deployment. Pipelines are a collection of jobs that are divided by stages.\
For more details:  [https://about.gitlab.com/topics/ci-cd/](https://about.gitlab.com/topics/ci-cd/)

<figure><img src="../../.gitbook/assets/Screen Shot 2022-10-31 at 4.02.51 PM.png" alt=""><figcaption></figcaption></figure>

**CI/CD Pipeline Stages:**

1. **Source/Code**: Triggered by a souce code repo.&#x20;
2. **Build:** Engineer will share a code via git repo to build products.&#x20;
3. **Unit Test**: Validate the code that we get from engineer
4. **Deploy** **Staging**: Once build and test is done we are ready to deploy that code to staging. Its done before deploying on production.
5. **Deploy Production**: Once we get a green sign from staging we are ready to depoy our code to a production.
