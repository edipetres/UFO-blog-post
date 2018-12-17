Written by Emil Klausen, Plamen Getsov and Edmond Petres

# Cloud Computing: How it can help creating business value

*Developer teams spend significant amount of time setting up and maintaining their IT systems [1]. Instead of focusing solely on the solution for their IT problems, they spend resources on managing their tools and infrastructure. Using cloud providers like AWS you could dramatically reduce the work on the setup of your infrastructure and hand over responsibilities to managed services that do things right.*

In today’s DevOps culture, developers focus not only on creating software but also handling operations. A robust infrastructure is needed to comply with the operating requirements but building one requires developer time and expertise in that field. Teams have the option to use Cloud Service Providers like AWS or Microsoft Azure who can help them deploy and manage infrastructure and software. The alternative of setting up one’s own software infrastructure allows for more customization to the company’s specific needs but can be a source of mistakes.

For developing the Hackernews Project we wanted to integrate and deploy changes multiple times a day. Users are constantly accessing our system so deploying new versions have to happen with minimal or preferably no downtime. Building our own infrastructure for this purpose would have taken a lot of resources. Read further to learn how we incorporated cloud services such as Amazon’s Elastic Beanstalk and what are the our prouds and pains.


## What is a CSP?
A cloud service provider or CSP is a company offering Infrastructure as a Service (IaaS), Software as a Service (SaaS) or Platform as a Service (PaaS). There are a handful of companies offering cloud services. Some of the big players are Amazon Web Services, Microsoft Azure, Google Cloud Platform and IBM. It is worth mentioning Digital Ocean here, but their main focus is on servers aka droplets.

![alt text](https://cdn.ttgtmedia.com/rms/onlineImages/cloud_computing-iaas_market_share_desktop.jpg)
[2]

### One-stop shop
Cloud providers are in a race to cover the developers’ needs. Whether you pick Microsoft’s Azure, AWS or any other provider, you will likely find that they are offering a service you need. Such as server instances, databases, file storage, domain management, logging systems and a lot more. This allows you to focus on one platform only.

### Simplicity of the setup
Deploying a piece of software or an infrastructure can be as easy as clicking through a few pages of setup in the GUI and voila! In a matter of minutes your development environment is ready to use. This manual setup is useful when it needs to be done a single time, but having to repeat it may be prone to errors. So what do we do?

### Programmable and Immutable
Most CSPs provide a programmable configuration - usually through a Command Line Interface or CLI, allowing for an immutable and executable infrastructure as code. Take for example the AWS CLI. You can write a few lines of bash script to create a new Ubuntu server with a preconfigured amount of resources that is defined in your code. Each time this script is executed it will produce the exact same results and is not prone to manual errors.

### Scaling
Whether your setup is manual or programmable, cloud services allow for configuring the environment to exactly the amount of hardware resources and software stack you project needs. Do you need more RAM, CPU power or a different version of Ubuntu? Maybe multiple instances of the same type? Cloud providers allow you to scale both vertically and horizontally in just a few clicks.

*How to set up auto-scaling on AWS [3]*

### The Bright Future of Cloud Technologies
Businesses today make more and more use of innovative technologies. As our internet bandwidth and quality increases, so does the storage capacity and computing performances. The market is predicted to continue its growth as more companies choose to benefit from software, platform and infrastructure as a service. [4]

![alt text](https://i.imgur.com/2tDEtiT.png)
[5]

## The Cost of Cloud Computing
Susan Ward, a freelance IT writer who runs a successful IT consulting company, warns about use cases where cloud computing is not the best alternative:

*“Cloud computing is the best thing for small business since the invention of the stapler. But that doesn't mean that there are no cloud computing disadvantages and that every small business should immediately throw out all their servers and desktop software and conduct all their business operations in the cloud.”  [6]*

### Possible downtime
Cloud computing makes you dependent on an internet connection. “When it's offline, you're offline”. Your business may suffer from a slow internet connection and from frequent outages.

Your provider will guarantee a certain uptime of the services, usually above 99% in the Service-level Agreement (SLA). But mistakes can and will happen even to the best.

On Prime Day in 2018 Amazon suffered an outage that affected their website and warehouse operations for hours. [7]

In 2017 the majority of the internet was grounded when Amazon’s S3 service crashed taking down businesses relying on that service. [8]

### Security Issues
Your data is in the cloud and that means you have to place your trust in your provider for secure handling and protection of the data.

### Cost
Performance-intensive applications such as video editing are not suitable for cloud computing and can wind up with very high running costs. Also, a sudden spike in your traffic can unexpectedly raise your bills if you did not set the limits right in you scaling configuration.

### Inflexibility
Be careful when choosing a vendor to make sure you don’t become a “forever” customer. Transferring your applications to other providers is not an easy process, and some will deliberately try to lock you in.

### Learning curve
Being such a large platform, one will spend quite a bit of time learning how to use their services. While this will have an initial learning cost, it is probably not higher than having to learn how to set up our own infrastructure in such a robust way that these specialized companies have done it for us.

### Customization
While most of the services usually work out of the box, we can run into difficulties trying to customize them. For example, when using a self-managed deployment platform like Elastic Beanstalk, you can only add software to the OS from supported repositories. This meant we could not install Prometheus to log the network requests as it was not available in such a repository.

## Alternative - Own hardware
Having your own hardware in house is more secure in the sense that you don’t have to trust a third party with your sensitive information, such as sensitive user data. Having a physical server also means you don’t need internet access for accessing the data.
Also you control exactly how you want to setup the server, so that it can be specifically tailored towards your exact needs. 

### Cost
However it’s quite expensive. Not only in the sense that you need to buy a physical server, but you also need to buy software, electricity, maintain it and update it with other physical hardware to improve the performance or capacity of it. In addition to that you also need to store it a safe place, to ensure that the physical hardware will take no damage from water, dust, heat etc. 
There are of course different ways of working with a physical server, you could go for bare minimum and get something that just works for a small website and not having to spend too much money and space, or scale it up, but then having to take all of the security of both physical and software into account.

### Upgrading hardware
A good thing about a physical server is that you decide what you want to install on it, and can therefore go for cheaper solutions than compared to what other services may provide. It can for example be with memory or storage, here you could buy a cheaper product that offers the same as a more expensive version, and thereby reduce the cost.
However it’s suggested that every third year you completely replace your server. In the chart below it’s shown how the cost of an older server will quickly increase after third year, in both weaker performance, but also with more time needed for support.

![alt text](https://blog.juriba.com/hs-fs/hubfs/ServerPerformanceCostIDC.png?width=640&name=ServerPerformanceCostIDC.png) [9]

A big downside to having only 1 server and needing to upgrade hardware, then you will have to shut it down, and thereby having downtime.

## The Chosen Toolstack
We settled with AWS as it is the biggest vendor out there with a large community, good learning resources and we had the most experience in the team with this cloud provider.

### Elastic Beanstalk
The service that brought the most value for our project was arguably Elastic Beanstalk. Let’s dive under the surface to learn how it helped our Hackernews project achieve the requirements of our operations team.

#### Zero-downtime Updates
Using Elastic Beanstalk (EB for short) with a Continuous Integration pipeline - in our case CircleCI - when pushing some new code to the VCS (Version Control System like Github or BitBucket), the system is built and deployed automatically without any downtime for the end user. 

*But how is this achieved?* When deploying a new version of your system, a new server instance is spawned and while the installation is in progress, all the requests to the website are routed to the previous running instance. Once the new version is ready to serve users, the load balancer reroutes traffic from the previous server to the new one and therefore causing no downtime. 

With a proper configuration in place (read more here - link eb how-to) the pipeline triggers a deployment attempt on Elastic Beanstalk with one line of code: *eb deploy*

What happens, however, if the server crashes? EB takes care of that automatically. A non-zero exit code that shuts down the server completely will automatically trigger a restart of this server. This has worked well for us and made sure our downtime was just a matter of seconds when a crash occurred.

#### Take-aways
This is a robust setup as it guarantees that our service (the API) is responding before publishing an updated version. In case anything goes wrong an alert will go off and the new (broken) version will not sabotage the running system. 

We have also saved a lot of time on configuring these servers where the app is deployed. By using the EB service, most if not all of the deployment steps are taken care of. It handles server creation, security, app deployment, zero-time updates, logging, alerts, statistics and many more.

## Conclusion
Overall, we are satisfied with choosing Amazon Web Services for hosting our static website and API. However, there are a number of considerations before we select a cloud computing platform instead of setting up our own hardware. Most CSPs will offer services under different names that achieve the same if not similar results. Prices can vouch for one vendor over the other, but personal preference and experience can influence the decision. We should expect that one day we might need to transfer our system to another vendor and no become too dependent on any of them. 

Taking the above things into consideration, when we chose a cloud service provider like AWS it is likely going to boost our team’s performance and take some responsibility off of our shoulders by managing the infrastructure for us. For a small website like ours that needs to scale with the users, has only a number of developers and needs to be secure and debuggable it makes sense to rely on cloud computing services.

After all, most new companies are not going to build an office for the employees to work in. They will instead rent out the amount of space they need from someone who is specialized in building offices.




## References
1. As described in the Software Development Life Cycle section - https://www.owasp.org/index.php/Testing_Guide_Introduction 
2. https://searchitchannel.techtarget.com/definition/cloud-service-provider-cloud-provider
3. https://docs.aws.amazon.com/autoscaling/ec2/userguide/GettingStartedTutorial.html
4. https://www.techfunnel.com/information-technology/5-future-trends-in-cloud-computing/
5. https://www.statista.com/statistics/500541/worldwide-hosting-and-cloud-computing-market/
6. https://www.thebalancesmb.com/disadvantages-of-cloud-computing-4067218
7. https://nordic.businessinsider.com/amazon-prime-day-outage-caused-by-moving-off-oracles-databases-2018-10?r=US&IR=T
8. https://www.theverge.com/2017/3/2/14792442/amazon-s3-outage-cause-typo-internet-server
9. https://blog.juriba.com/the-hidden-costs-of-an-aging-it-infrastructure



