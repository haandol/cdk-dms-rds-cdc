# CDK DMS CDC Example

From Aurora MySQL to kafka CDC(Capture Data Change) example using CDK.

![architecture](/docs/architecture.png)

# Prerequisites

- setup awscli
- node 16.x
- cdk 2.x

## Enable Aurora MySQL CDC

follow the Resolution at the [link](https://aws.amazon.com/premiumsupport/knowledge-center/aurora-mysql-increase-binlog-retention/?nc1=h_ls) and set db parameter group.

and then, run following sql on Aurora MySQL to increase binlog retention.

```sql
call mysql.rds_set_configuration('binlog retention hours', 168);
```

# Installation

open [**infra/env/dev.env**](/infra/env/dev.env) and fill the blow fields

> Remove all optional fields for empty value (empty value will be failed on validation)

- `AWS_ACCOUNT_ID`: 12 digit account id. e.g. `123456789012`
- `AWS_REGION`: e.g. `ap-northeast-2`
- `VPC_ID`: e.g. `vpc-xxxxxxxx`
- `SECURITY_GROUP_IDS`: comma separated security group ids. e.g. `sg-xxxxxxxx,sg-yyyyyyyy`
- `SECRETS_MANAGER_ARN`: secretsmanager secretarn that contains RDS connection informations. e.g. `arn:aws:secretsmanager:ap-northeast-2:xxxxx:secret:yyyyy`
- `KAFKA_BROKER`: TLS kafka broker address including port. e.g. kafka-broker1:9094,kafka-broker2:9094
- `KAFKA_TOPIC`: outgoing kafka topic name. e.g. "cdc"

and copy `env/dev.env` file to project root as `.env`

```bash
$ cd infra
$ cp env/dev.env .env
```

```bash
$ npm i
```

bootstrap cdk if no one has run it on the target region

```bash
$ cdk bootstrap
```

deploy infra

```
$ cdk deploy "*" --require-approval never
```
