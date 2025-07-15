# DevOps Fundamentals

## Overview of DevOps Principles and Practices

DevOps is a set of practices that aim to automate and integrate the processes between software development (Dev) and IT operations (Ops). By promoting a culture of collaboration, communication, and integration, DevOps shortens the system development life cycle and delivers high-quality software faster.

### Key Principles of DevOps

#### Automation

Automating repetitive tasks such as testing, building, and deployment. This allows teams to focus on more important tasks. Tools like Jenkins, GitLab CI, and CircleCI are widely used to automate these tasks.

Example: Automating the build and deployment process using Jenkins Pipeline:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
```

#### Continuous Integration/Continuous Deployment (CI/CD)

CI: Developers frequently integrate code into a shared repository, where automated builds and tests are run.  
CD: Automates the release of code into production, ensuring deployments are fast and reliable.

Example of a CI/CD pipeline (GitHub Actions YAML):

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Build
      run: npm run build

    - name: Deploy
      run: |
        scp -r ./build/ user@server:/var/www/html/
```

#### Infrastructure as Code (IaC)

Managing and provisioning infrastructure through code rather than manual processes. Tools like Terraform, Ansible, and AWS CloudFormation help manage infrastructure changes in a version-controlled and reproducible way.

Terraform Example: Creating an AWS EC2 instance:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "DevOpsInstance"
  }
}
```

#### Monitoring and Logging

Continuous monitoring and logging ensure the software and infrastructure are operating as expected. Tools like Prometheus, Grafana, and the ELK stack (Elasticsearch, Logstash, Kibana) help in tracking system health and performance.

Example: Prometheus configuration for system monitoring:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

## DevOps Culture, Collaboration, and Key Benefits

DevOps promotes a culture of collaboration between development and operations teams, breaking the traditional silos. This shared responsibility allows faster, more reliable software delivery.

### Key Aspects of DevOps Culture

#### Collaboration

In traditional software development models, development, operations, and testing teams work in isolation. DevOps brings these teams together to work collaboratively. Encourages transparency and a sense of shared responsibility for the product's success.

Example of collaboration using a shared CI/CD pipeline:  
Teams use tools like Jenkins or GitLab CI to work on the same pipeline for building, testing, and deploying software. Developers push their changes, which trigger the CI/CD process; the operations team can monitor deployments and system health.

#### Shared Responsibility

Both development and operations teams share accountability for delivering high-quality, reliable software. This includes managing everything from feature development to system stability and user experience.

#### Continuous Feedback

DevOps emphasizes rapid and continuous feedback from all stakeholders (developers, testers, operations, and users). This feedback loop ensures that issues are addressed quickly, leading to better product quality.

### Key Benefits of DevOps

#### Faster Time to Market

By automating processes and enabling teams to work more closely, DevOps practices help release features and fixes faster.

CI/CD Example: Automated deployment after code is pushed:

```yaml
name: Auto-Deploy on Push

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Deploy to server
      run: scp -r ./build/ user@server:/var/www/html/
```

#### Improved Collaboration and Communication

Breaking down silos encourages better communication between teams, resulting in fewer misunderstandings and faster problem resolution.

#### Increased Efficiency through Automation

Automated testing, deployment, and monitoring reduce manual intervention, minimizing human errors and speeding up processes.

#### Higher Quality Products

Continuous testing and integration lead to fewer bugs, improved system reliability, and better overall software quality.

#### Better Customer Experience

Faster delivery and higher quality ensure users get updates and fixes quickly, leading to a better overall user experience.



### DevOps Lifecycle

![DevOps Lifecycle](../../assets/devops-lifecycle-diagram.png)
#### 1. Continuous Development

In Continuous Development, code is written in small, continuous bits rather than all at once. Continuous Development is important in DevOps because this improves efficiency every time a piece of code is created, it is tested, built, and deployed into production. Continuous Development raises the standard of the code and streamlines the process of repairing flaws, vulnerabilities, and defects. It facilitates developers’ ability to concentrate on creating high-quality code.

#### 2. Continuous Integration

Continuous Integration can be explained mainly in 4 stages in DevOps:

- Getting the SourceCode from SCM
- Building the code
- Code quality review
- Storing the build artifacts

The stages mentioned above are the flow of Continuous Integration and we can use any of the tools that suit our requirement in each stage. Some of the most popular tools are GitHub for source code management (SCM), Maven for building, SonarQube for code quality reviews, and Nexus for storing build artifacts. This whole process is achieved by using a Continuous Integration tool like Jenkins.

#### 3. Continuous Testing

Any firm can deploy continuous testing with the use of agile and DevOps methodologies. Depending on our needs, we can perform continuous testing using automation testing tools such as Testsigma, Selenium, LambdaTest, etc. With these tools, we can test our code and prevent problems and code smells, as well as test more quickly and intelligently. With the aid of a continuous integration platform like Jenkins, the entire process can be automated, which is another added benefit.

#### 4. Continuous Deployment/Continuous Delivery

- **Continuous Deployment**: The process of automatically deploying an application into the production environment when it has completed testing and the build stages. Here, we’ll automate everything from obtaining the application’s source code to deploying it.
- **Continuous Delivery**: The process of deploying an application into production servers manually when it has completed testing and the build stages. Here, we’ll automate the continuous integration processes, however, manual involvement is still required for deploying it to the production environment.

#### 5. Continuous Monitoring

DevOps lifecycle is incomplete if there was no Continuous Monitoring. Continuous Monitoring can be achieved with the help of Prometheus and Grafana. We can continuously monitor and get notified before anything goes wrong. Prometheus gathers many performance measures, including CPU and memory utilization, network traffic, application response times, error rates, and others. Grafana makes it possible to visually represent and keep track of data from time series, such as CPU and memory utilization.

#### 6. Continuous Feedback

Once the application is released into the market, end users will give feedback about the performance of the application and any glitches affecting the user experience. After getting multiple feedbacks from end users, the DevOps team will analyze the feedback and reach out to the developer team to rectify the mistakes in the code. This reduces errors or bugs in the current development and produces more effective results for end users, also reducing unnecessary steps to deploy the application.

#### 7. Continuous Operations

We will sustain higher application uptime by implementing continuous operation, which will assist us to cut down on the maintenance downtime that negatively impacts end users’ experiences. More output, lower manufacturing costs, and better quality control are benefits of continuous operations.

### Different Phases of the DevOps Lifecycle

1. **Plan**: Professionals determine the commercial need and gather end-user opinions throughout this level. In this step, they design a project plan to optimize business impact and produce the intended result.
2. **Code**: During this point, the code is being developed. To simplify the design process, the developer team employs lifecycle DevOps tools and extensions like Git that assist them in preventing safety problems and bad coding standards.
3. **Build**: After programmers have completed their tasks, they use tools such as Maven and Gradle to submit the code to the common code source.
4. **Test**: To assure software integrity, the product is first delivered to the test platform to execute various sorts of screening such as user acceptability testing, safety testing, integration checking, speed testing, and so on, utilizing tools such as JUnit, Selenium, etc.
5. **Release**: At this point, the build is prepared to be deployed in the operational environment. The DevOps department prepares updates or sends several versions to production when the build satisfies all checks based on the organizational demands.
6. **Deploy**: At this point, Infrastructure-as-Code assists in creating the operational infrastructure and subsequently publishes the build using various DevOps lifecycle tools.
7. **Operate**: This version is now convenient for users to utilize. With tools including Chef, the management department takes care of server configuration and deployment at this point.
8. **Monitor**: The DevOps workflow is observed at this level depending on data gathered from consumer behavior, application efficiency, and other sources. The ability to observe the complete surroundings aids teams in identifying bottlenecks affecting the production and operations teams’ performance.

### CALMS Framework
CALMS is a framework in DevOps that is used to access and measure the company’s abilities to adopt DevOps and its success. CALMS stands for **Culture**,**Automation**, **Lean**, **Measurement** and **Sharing**.

1. **Culture:**
2. **Automation:**
3. **Lean:**
4. **Measurement:**
5. **Sharing:**