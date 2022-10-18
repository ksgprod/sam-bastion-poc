# Visão geral

Este projeto contém código-fonte escrito em [NodeJS](https://nodejs.org/en/docs/) e arquivos de suporte para um aplicativo serverless que você pode implantar com o [SAM CLI](https://docs.aws.amazon.com/pt_br/serverless-application-model/latest/developerguide/what-is-sam.html). Inclui os seguintes arquivos:

- src/handlers/bastion.js - Código para funções [Lambda](https://aws.amazon.com/pt/lambda) da aplicação.
- template.yaml - Template que define os recursos da AWS da aplicação.

O aplicativo usa vários recursos da AWS, incluindo funções do Lambda. Esses recursos são definidos no arquivo `template.yaml` do projeto. Você pode atualizar o modelo para adicionar recursos da AWS por meio do mesmo processo de implantação que atualiza o código da aplicação.

Esta aplicação contemplam scripts para execução e interrupção de determinadas instâncias EC2 parametrizadas através de seus IDs, automatizadas através de funções Lambda, agendadas por meio da função cron().

## Deploy da aplicação

A Serverless Application Model Command Line Interface (SAM CLI) é uma extensão da [AWS CLI](https://aws.amazon.com/pt/cli/) que adiciona funcionalidade para criar e testar aplicativos Lambda. Ele usa o [Docker](https://docs.docker.com/engine/reference/builder/) para executar suas funções em um ambiente Amazon Linux que corresponda ao Lambda.

Para usar o SAM CLI, você precisa das seguintes ferramentas:

* SAM CLI - [Guia de instalação do SAM CLI](https://docs.aws.amazon.com/pt_br/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html);
* Node.js - [Guia de instalação do Node.js 16](https://nodejs.org/pt-br/);
* Docker - [Guia de instalação do Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

Para compilar e implantar seu aplicativo pela primeira vez, execute os seguintes comandos shell:

```bash
sam build
sam deploy --guided
```

* **Stack Name**: O nome da stack a ser implantada no CloudFormation (exclusivo para sua conta e região).
* **AWS Region**: A região da AWS na qual você deseja implantar sua aplicação.
* **Confirm changes before deploy**: Define a necessidade de confirmação das alterações a serem implantadas. 
* **Allow SAM CLI IAM role creation**: Permite criação de novas roles IAM, necessárias para criação das funções Lambda.
* **Save arguments to samconfig.toml**: Permite salvar suas opções em um arquivo de configuração dentro do projeto. Tal arquivo permite que você possa apenas executar novamente o `sam deploy` sem parâmetros para novas implantações.

É necessário atribuir as seguintes permissões ao usuário IAM, para realização do deploy:

* AmazonEC2FullAccess
* IAMFullAccess
* CloudWatchEventsFullAccess
* AmazonS3FullAccess
* AWSCloudFormationFullAccess
* AWSLambda_FullAccess

Você pode entender melhor as políticas aplicadas e limitá-las criando suas próprias políticas, caso necessário.

## Execução local via SAM CLI

Build da aplicação com o comando: `sam build`.

```bash
sam build
```

Execute as funções localmente através do comando: `sam local invoke`.

```bash
sam local invoke StartBastion
```

```bash
sam local invoke ShutDownBastion
```

PS.: Passe uma lista de parâmetros "InstanceIds" contemplando os IDs das instâncias EC2, nas quais se deseja executar, substituindo onde se lê "YOUR_EC2_INSTANCE_ID", no seu arquivo `src/handlers/bastion.js`.
