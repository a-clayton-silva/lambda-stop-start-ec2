# lambda-stop-start-ec2

1 - criar uma nova politica e atachar em uma nova role no iam


{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "logs:CreateLogGroup",
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ],
        "Resource": "arn:aws:logs:*:*:*"
      },
      {
        "Effect": "Allow",
        "Action": [
          "ec2:Start*",
          "ec2:Stop*"
        ],
        "Resource": "*"
      }
    ]
  }


2 - criar a lambda, atachar a role criada e incluir o codigo abaixo, um para start e outro para stop, usar timeout 15 segundos.


import boto3
region = 'us-east-1'
instances = ['i-0daadbc51e675566d']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))


##################################

import boto3
region = 'us-east-1'
instances = ['i-0daadbc51e675566d']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('starting your instances: ' + str(instances))
    
    
    

3 - criar gatilho eventbridge cloudwatch events


criar uma nova regra
#obs, lembrando que na cron, tem que somar 3 horas, se o caso abaixo seria para iniciar as 19h, colocar as 22h. minute, hour, day month, month, day week. https://crontab.guru/

start
cron(20 22 ? * MON,WED *)
stop
cron(0 1 ? * TUE,THU *)


4 - criar uma retençao de expiraçao de logs no cloudwatch para nao gerar custo


cloudwatch / logs / logs groups
clicar em never expire em terention e alterar para 1 dia ou 7 dias.


#####

FIM


#####





####### exemplo para stop start para rds

import boto3
region = 'us-east-1'
rds = boto3.client('rds', region_name=region)
my_instances = ['rdsclaudio']
def lambda_handler(event, context):
    for instance in my_instances:
	    rds.start_db_instance(DBInstanceIdentifier=instance)
	    print('starting your RDS: ' + str(instance))



#####


import boto3
region = 'us-east-1'
rds = boto3.client('rds', region_name=region)
my_instances = ['rdsclaudio']
def lambda_handler(event, context):
    for instance in my_instances:
	    rds.stop_db_instance(DBInstanceIdentifier=instance)
	    print('stopped your instances: ' + str(instance))
