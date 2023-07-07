
# Install AWS parallel cluster and Run using the following commands

### Reference link - https://docs.aws.amazon.com/parallelcluster/latest/ug/install-linux.html#install-linux-path

### 1. Install anaconda or virtualenv With python version 3.7 or higher 
### 2. Install AWS Parallel Cluster
```bash
pip install --upgrade "aws-parallelcluster<3.0"
```

### 3. Check the version of AWS Parallel Cluster
```bash
pcluster version
```

### 4. Download AWS CLI
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
```

### 5. Install AWS CLI
```bash
./aws/install --update -i ~/aws-cli -b ~/bin     # Change the path as per your requirement
```

### 6. Configure AWS CLI
```bash
aws configure                 # You will be asked to enter your AWS access key and secret key. Ask your admin for the same. You also need to have IAM access.
```

### 7. Create a cluster configuration file

* File at this path - .parallelcluster/config
* You need to create vpc_id, master_subnet_id, Key Pair.
* Example configration file is given below
```bash
[cluster default]
key_name = <key_name>
vpc_settings = public
ebs_settings = myebs
compute_instance_type = c6i.metal
compute_root_volume_size = 500
master_instance_type = c6i.metal
master_root_volume_size = 1023
maintain_initial_size = false
initial_queue_size = 0
max_queue_size = 4
placement_group = DYNAMIC
placement = cluster
scaling_settings = custom
tags = {"name": "pcl"}
base_os = centos7
scheduler = slurm
enable_intel_hpc_platform = true

[scaling custom]
scaledown_idletime=10

[vpc public]
vpc_id = vpc-0ae8ab2ecbee42daa
master_subnet_id = subnet-022a836e87667accc

[ebs myebs]
shared_dir = /shared
volume_type = gp2
volume_size = 2023

[aliases]
ssh = ssh {CFN_USER}@{MASTER_IP} {ARGS}

[aws]
aws_region_name = us-east-2
```

### 8. Create a cluster
```bash
pcluster create <cluster_name>      # it will report the ip address of the master node. ssh to the master node using the ip address.
```

### 9. Check the status of the cluster
```bash
pcluster status <cluster_name>
```

