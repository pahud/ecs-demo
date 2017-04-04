# ecs-demo

// set “default” as ECScluster name

**$ ecs-cli configure -c default**

 

// create ECS cluster

**$ ecs-cli up --capability-iam --keypair {KEYNAME} --vpc {VPCID} --subnets subnet-xxxxxxxx,subnet-xxxxxxxx**



// create a SG for ALB with80 443 public to all

**aws ec2 create-security-group --vpc-id {VPCID} --group-name ECSALB --description ECSALB**

**aws ec2 authorize-security-group-ingress --group-id {SG_ID}  --protocol tcp --port 80 --cidr 0.0.0.0/0**

**aws ec2 authorize-security-group-ingress --group-id  {SG_ID} --protocol tcp --port 443 --cidr 0.0.0.0/0**



// create ALB

**$ aws elbv2 create-load-balancer --name ecsalb --subnets subnet-xxxxxxxx subnet-xxxxxxxx**



// update security group ofecsalb, attach the security group “*ECSALB”*previously created to this ALB



// create ALB target group

**$ aws elbv2 create-target-group --name ecs-detault-tg--port 80 --vpc-id  {VPCID} --protocolHTTP --health-check-protocol HTTP --healthy-threshold-count 2--unhealthy-threshold-count 2 --health-check-interval-seconds 30**



// create ALB listener

**$ aws elbv2 create-listener --port 80 --protocol HTTP--load-balancer-arn {ARN} --default-actions Type=forward,TargetGroupArn={ARN}**



// create a ECS Service Roleas “ecsServiceRole”

 

// create service andregister to ALB/targetGroup

**$ ecs-cli compose service create myweb --container-namemyweb --container-port 80 --role ecsServiceRole --target-group-arn {ARN}**



// scale service count to 1 

**$ ecs-cli compose service scale 1**

