# CloudFormation - Desafio

Este repositório contém um template YAML para ser utilizado no AWS CloudFormation que cria:

- Uma instância EC2 do tipo `t2.nano` utilizando a AMI mais recente do Amazon Linux 2023 (via SSM Parameter Store).
- Um usuário IAM chamado `joao.silva`.
- Um bucket S3 com nome padrão `desafio-andrea-silva-dio`.

## Como implantar

Você pode implantar com o AWS CLI:

1. Certifique-se de ter credenciais configuradas (perfil com permissões de CloudFormation, EC2, IAM e S3).
2. Execute:

```
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name desafio-cfn-stack \
  --capabilities CAPABILITY_NAMED_IAM
```

- Caso deseje alterar parâmetros (por exemplo, nome do bucket ou tipo de instância):

```
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name desafio-cfn-stack \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameter-overrides BucketName=SEU_BUCKET_UNICO InstanceType=t2.nano
```

## Insights e observações

- EC2: o template usa a AMI mais recente do Amazon Linux 2023 por meio de SSM (`LatestAmiId`). Isso evita hardcode de AMI por região e mantém a instância atualizada.
- IAM: o usuário `joao.silva` foi criado sem políticas anexadas por padrão. Caso seja necessário permissões específicas, podemos anexar políticas gerenciadas ou políticas inline posteriormente.
- Capacidades: por criar recursos IAM com nome explícito, é necessário passar `--capabilities CAPABILITY_NAMED_IAM` no deploy.
- Custos e limpeza: recursos EC2 e S3 podem gerar custos. Para evitar cobrança contínua, sempres remova a stack quando terminar:

```
aws cloudformation delete-stack --stack-name desafio-cfn-stack
```

## Saídas (Outputs)

Após a implantação, o CloudFormation fornecerá:

- `EC2InstanceId`: ID da instância EC2 criada.
- `IAMUserArn`: ARN do usuário IAM.
- `S3BucketName`: nome do bucket S3.
