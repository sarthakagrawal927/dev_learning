# intermediate_web

Has a lot of interesting submodules. Keep them updated

```bash
git pull --recurse-submodules
git submodule update --remote
```

## Performance 1 - Network Requests

* HTML, CSS, JS - Compressed via webpack (auto usually)
* Images - SVG op, JPG - most colors, PNG -> Transparency, GIF- Animations (always compress), MEDIA-QUERY must
* Limit number of files by combining (auto usually - webpack)
* Script after CSS & make sure only necessary (visible page) style sheets are imported
* Conditional loading based on Media query, less specificity means less work for browser to make sense of the tag
* Inline styles much faster, scripts are blockage.
  * Async - loads JS in parallel w another thread - followed by immediate execution - Core functionality needs JS
  * Defer - loads JS in parallel w another thread - executed after page loads -  Not important to core functionality
  * NILL - loads JS (blocking main) - executes it - Core scripts

## Performance 2 - React Mode

* Code splitting via Lazy loading.
* Performance optimizing via memoization, rerendering only necessary files.

## Cloud Devops ( AWS )

* EC2 - Server in cloud
* Elastic Block Store - to increase capacity of instance
* VPC - Secure EC2
* Lambda - Run functions on need basis
* Elastic Bean Stalk - Manages EC2, Autoscaling, Deployment, load balancers, etc out of the box.
* S3 - Storage
* S3-Glacier - Archive
* DynamoDB - NoSQL DB
* Cloudfront - CDN
* Redshift - Big data - analyzing, fast access data
* Shield - Secure EC2 from attacks
* WAF - Firewall
* IAM - Identity & Access Management - Roles, User, Policies etc
* Route53 - DNS, resource checkup
* Elasticity - Automatically scale up/down
* Load Balancer - Load balancing between servers
* Simple Notification Service, Simple Queue Service
* Elastic Container Service - Managing Docker Containers
* CloudWatch - Collect & Track Metrics
* CloudTrail - Audit all activity in AWS
* CloudFormation - Set up everything via scripts

## Top qualities

1. Analytical Problem solving with code
2. Technical Communication
3. Engineering Best practices & approaches.
4. Non-tech Communication
5. Computer & Language experiences.
