
# Resource Inventory
Just as it is when performing security testing within traditional environments, so is reconnaissance an important factor when conducting security testing within cloud environments. 

Unlike traditional network security reconnaissance, reconnaissance within a cloud environment is heavily driven by API defined service discovery, allowing testers to discover the services utilised by a target organisation. 

This step is crucial as the large and expanding number of services offered by Cloud Service Providers means that organisations may piece together many different services from multiple regions to form a customised cloud environment. 

## Performing Amazon AWS Inventory Discovery
Collecting inventory within an Amazon Web Services environment can be performed in a number of ways ranging from services offered by Amazon Web Services to custom built scripts or tools that have been created to collect the same information using SDKs provided by AWS such as botocore[https://github.com/boto/botocore] a Python AWS SDK.

### AWS Config (paid service)
AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations. With Config, you can review changes in configurations and relationships between AWS resources, dive into detailed resource configuration histories, and determine your overall compliance against the configurations specified in your internal guidelines. This enables you to simplify compliance auditing, security analysis, change management, and operational troubleshooting.

From a discovery perspective, AWS Config offers a number of operations that can be performed to discover resources used by the target organisation such as the operation `list-discovered-resources` which returns a list of resources used by a target organisation. 

#### Usage
The following command lists the EC2 instances that AWS Config has discovered:

```
aws configservice list-discovered-resources --resource-type AWS::EC2::Instance

{
    "resourceIdentifiers": [
        {
            "resourceType": "AWS::EC2::Instance",
            "resourceId": "i-1a2b3c4d"
        },
        {
            "resourceType": "AWS::EC2::Instance",
            "resourceId": "i-2a2b3c4d"
        },
        {
            "resourceType": "AWS::EC2::Instance",
            "resourceId": "i-3a2b3c4d"
        }
    ]
}
```


### boto3 (AWS Python SDK)

Boto is the Amazon Web Services (AWS) SDK for Python. It enables Python developers to create, configure, and manage AWS services, such as EC2 and S3. Boto provides an easy to use, object-oriented API, as well as low-level access to AWS services.

Using boto3 reconnaissance can be automated by using pre-configured enumeration scripts. 

#### Installing boto3

To install boto3 on your device you can use a single pip command as listed below: 

`pip3 install boto3`

> If this does not work you may need to invoke Pip directly through the command-line as follows: 

`python3 pip install boto3`

Before running boto3 you must first provide your AWS API keys which can be done using the following: XYZ

Once boto3 is installed your options are vastFull documentation of the boto3 API can be found at https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html

For example, the following script will use the boto3 SDK to retrieve a list of all ec2 resources in all regions and output the respective `instance.id`, `instance_type` and `region` that the instance resides in. 

```
#!/usr/bin/env python3
import boto3

client = boto3.client('ec2')

ec2_regions = [region['RegionName'] for region in client.describe_regions()['Regions']]

for region in ec2_regions:
    conn = boto3.resource('ec2')
    instances = conn.instances.filter()
    for instance in instances:
        if instance.state["Name"] == "running":
            print (instance.id, instance.instance_type, region)


```

> $ python ec2ls.py

```
$ python ec2ls.py 
('i-0747e71cf815d426c', 't3.micro', 'eu-north-1')
('i-0db0b54f323bcc66c', 't3.small', 'eu-north-1')
('i-0afe1327162de73fe', 't3.micro', 'eu-north-1')
('i-0747e71cf815d466c', 't3.micro', 'ap-south-1')
('i-0db0b54f323bcc76c', 't3.small', 'ap-south-1')
('i-0afe1327162de74fe', 't3.micro', 'ap-south-1')
('i-0747e71cf815d426c', 't3.micro', 'eu-west-3')
('i-0db0b54f323bcc56c', 't3.small', 'eu-west-3')
('i-0afe1327162de73fe', 't3.micro', 'eu-west-3')
('i-0747e71cf815d476c', 't3.micro', 'eu-west-2')
('i-0db0b54f323bcc96c', 't3.small', 'eu-west-2')
```


### Tools

### aws-inventory (NCCGROUP)
Aws-inventory is a tool that tries to discover all AWS resources created in an account, it uses botocore to discover AWS services and what regions they run in. It is also used in invoking the service APIs. The APIs that are invoked are those which should list or describe resources. The results can be printed to stdout in JSON format. They can also be written across several files:

- Raw responses from API endpoints can be written to a file specified on the commandline. The file format is Python pickle.
- Exceptions raised during tool execution can be written to a file specified on the commandline. The file format is Python pickle.
- gui/aws_inventory_data-<environment_name>.json - JSON format. Parsed responses structured for input to the GUI.

#### Usage

- Running with defaults will 

- List AWS services known to botocore. This is all done locally by reading service model files.

```
$ python aws_inventory.py --list-svcs
acm
apigateway
application-autoscaling
appstream
autoscaling
batch
budgets
clouddirectory
cloudformation
cloudfront
.
.
.
```

- List service operations known to botocore. This is all done locally by reading service model files.

```
$ python aws_inventory.py --list-operations
[shield]
DescribeSubscription
ListAttacks
ListProtections

[datapipeline]
ListPipelines

[firehose]
ListDeliveryStreams
.
.
.
[glacier]
# NONE

[stepfunctions]
ListActivities
ListStateMachines

Total operations to invoke: 4045
```

- Print what APIs would be called for a service. This is all done locally.

```
$ python aws_inventory.py --debug --dry-run
```

### aws-list-all (JohannesEbke)
aws-list-all is another tool that leverages the AWS Python SDK (boto3) which allows users to list all resources and services within all regions used within an AWS account.

- Restricting the region and service is optional, a simple query without arguments lists everything. It uses a thread pool to parallelize queries and randomizes the order to avoid hitting one endpoint in close succession. One run takes around two minutes for me.


#### Usage

Although aws-list-all supports output to the terminal, considering the large amount of output returned by the available AWS APIs, it is recommended to store the output of your queries to disk and analyse them using the built-in `show` command within the aws-list-all tool.

- Querying all services in the ap-southeast-2 region 

```
aws-list-all query --region ap-southeast-2 --parallel 1
```

- Querying all ec2 service information within the eu-west-1 region and storing the output in the ./data directory for further analysis

```
aws-list-all query --region eu-west-1 --service ec2 --directory ./data/
```

- Printing the output for each service or region that was collected 

```
aws-list-all show --verbose ./data/*
```

```
ec2 ap-southeast-2 DescribeAddresses Addresses 3
ec2 ap-southeast-2 DescribeImages Images 2
ec2 ap-southeast-2 DescribeInstanceCreditSpecifications InstanceCreditSpecifications > 1
ec2 ap-southeast-2 DescribeInstances Reservations 3
ec2 ap-southeast-2 DescribeInstanceStatus InstanceStatuses 3
ec2 ap-southeast-2 DescribeInstanceTypeOfferings InstanceTypeOfferings 219
ec2 ap-southeast-2 DescribeInstanceTypes InstanceTypes > 100
ec2 ap-southeast-2 DescribeInternetGateways InternetGateways 1
ec2 ap-southeast-2 DescribeKeyPairs KeyPairs 1
ec2 ap-southeast-2 DescribeNetworkInterfaces NetworkInterfaces 5
ec2 ap-southeast-2 DescribeSecurityGroups SecurityGroups 2
ec2 ap-southeast-2 DescribeSnapshots Snapshots 2
ec2 ap-southeast-2 DescribeSubnets Subnets 3
ec2 ap-southeast-2 DescribeTags Tags 16
ec2 ap-southeast-2 DescribeVolumes Volumes 3
ec2 ap-southeast-2 DescribeVolumeStatus VolumeStatuses 3
ec2 ap-southeast-2 DescribeVpcs Vpcs 1

```

