# Como implantar no App Engine

bookmark_border



Nesta página, explicamos como implantar aplicativos no App Engine usando o Cloud Build. Se você não estiver familiarizado com o Cloud Build, leia os [guias de início rápido](https://cloud.google.com/build/docs/quickstarts) e a [visão geral da configuração de build](https://cloud.google.com/build/docs/build-config) primeiro.

O App Engine é uma plataforma sem servidor totalmente gerenciada para desenvolver e hospedar aplicativos da Web em escala. Para mais informações sobre o App Engine, leia a [documentação do App Engine](https://cloud.google.com/appengine/docs).

## Antes de começar

- Ative a API App Engine.

  [Ativar a API App Engine](https://console.cloud.google.com/apis/library/appengine.googleapis.com)

- Para executar os comandos do `gcloud` nesta página, instale a [Google Cloud CLI](https://cloud.google.com/sdk).

- Tenha em mãos o código-fonte do aplicativo que você quer criar e implantar no App Engine. O código-fonte precisa ser armazenado em um repositório, como o Cloud Source Repositories, o GitHub ou o Bitbucket.

### Permissões do IAM obrigatórias

Conceda o papel **Administrador do App Engine** e **Usuário da conta de serviço** à conta de serviço do Cloud Build:

1. Abra a página **Configurações do Cloud Build**:

   

   [Abrir a página "Configurações do Cloud Build"](https://console.cloud.google.com/cloud-build/settings)

   

2. Defina o status do papel **Administrador do App Engine** e do papel **Usuário da conta de serviço** como **Ativado**.

## Como configurar a implantação

O Cloud Build permite que você use qualquer imagem de contêiner disponível publicamente para executar suas tarefas. Para fazer isso, especifique a imagem em uma `step` de build no arquivo de configuração do Cloud Build.

O App Engine fornece o comando `gcloud app deploy`, que cria uma imagem com o código-fonte e implanta essa imagem no App Engine. Use a [imagem `cloud-sdk`](https://github.com/GoogleCloudPlatform/cloud-sdk-docker) como uma etapa de compilação no arquivo de configuração para invocar os comandos `gcloud` na imagem. Os argumentos passados para essa etapa de versão são passados diretamente para a CLI gcloud, permitindo que você execute qualquer comando `gcloud` nesta imagem.

Para implantar um aplicativo no App Engine, siga estas etapas:

1. Crie um [arquivo de configuração do Cloud Build](https://cloud.google.com/build/docs/build-config) com o nome `cloudbuild.yaml` ou `cloudbuild.json`.

   **Observação:** não armazene o arquivo de configuração no mesmo diretório que o código-fonte, porque a CLI gcloud pode interpretar que você quer implantar o aplicativo usando o Docker por meio do Cloud Build. Em vez disso, armazene a origem em um diretório separado e crie o arquivo de configuração no diretório pai.

2. No arquivo de configuração:

   - Adicione um campo `name` para especificar a etapa de build `cloud-sdk`.

   - Adicione um campo `entrypoint` para usar a ferramenta `bash` quando `cloud-sdk` for invocado.

   - No campo `args`, invoque o comando `gcloud app deploy` e defina um `timeout` para o [App Engine usar quando invocar o Cloud Build](https://cloud.google.com/appengine/docs/standard/nodejs/testing-and-deploying-your-app#deploying_your_application). Isso é necessário porque as etapas de build do Cloud Build e os builds têm um tempo limite padrão de 10 minutos, enquanto as implantações do App Engine podem levar mais tempo para concluírem. Especificar um tempo limite maior garante que o tempo limite da build não seja atingido se `gcloud app deploy` demorar mais de 10 minutos para ser concluído.

     **Erros de tempo limite ao usar o ambiente padrão do App Engine**: é possível configurar tempos limite conforme descrito aqui ao usar o ambiente flexível do App Engine. O ambiente padrão do App Engine não permite que o tempo limite da versão seja configurado. Se você estiver usando o Cloud Build para implantar no ambiente padrão do App Engine e o build falhar com um erro de tempo limite, considere o uso do ambiente flexível do App Engine ou[Cloud Run](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run) em vez do ambiente padrão do App Engine.

   - Adicione um valor [build `timeout`](https://cloud.google.com/build/docs/build-config#build_steps) de mais de 10 minutos.

   [YAML](https://cloud.google.com/build/docs/deploying-builds/deploy-appengine#yaml)[JSON](https://cloud.google.com/build/docs/deploying-builds/deploy-appengine#json)

   ```
   steps:
   - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
     entrypoint: 'bash'
     args: ['-c', 'gcloud config set app/cloud_build_timeout 1600 && gcloud app deploy']
   timeout: '1600s'
   ```

3. Inicie o build, em que `SOURCE_DIRECTORY` é o caminho ou URL para o código-fonte e `REGION` é uma das [regiões de build compatíveis](https://cloud.google.com/build/docs/locations) para iniciar o build:

   ```
    gcloud builds submit --region=REGION SOURCE_DIRECTORY
   ```

## Implantação contínua

É possível automatizar a implantação de seu software no Google App Engine criando gatilhos do Cloud Build. Configure os acionadores para criar e implantar imagens sempre que atualizar o código-fonte.

Para automatizar sua implantação no App Engine:

1. No repositório, adicione um arquivo de configuração com etapas para invocar o comando `gcloud app deploy`:

   [YAML](https://cloud.google.com/build/docs/deploying-builds/deploy-appengine#yaml)[JSON](https://cloud.google.com/build/docs/deploying-builds/deploy-appengine#json)

   ```
   steps:
   - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
     entrypoint: 'bash'
     args: ['-c', 'gcloud config set app/cloud_build_timeout 1600 && gcloud app deploy']
   timeout: '1600s'
   ```

2. Crie um gatilho de compilação com o arquivo de configuração criado na etapa anterior:

   1. Abra a página **Gatilhos** no Console do Google Cloud:

      [Abrir a página "Gatilhos"](https://console.cloud.google.com/cloud-build/triggers)

   2. Selecione o projeto no menu suspenso do seletor de projetos na parte superior da página.

   3. Clique em **Abrir**.

   4. Clique em **Criar gatilho**.

      Na página **Criar gatilho**, especifique as seguintes configurações:

      1. Insira um nome para o gatilho.
      2. Selecione o evento de repositório para iniciar o gatilho.
      3. Selecione o repositório que contém o código-fonte e o arquivo de configuração de build.
      4. Especifique o regex do nome da ramificação ou da tag que iniciará o gatilho.
      5. **Configuração**: escolha o arquivo de configuração do build que você criou anteriormente.

   5. Clique em **Criar** para salvar o gatilho de compilação.

Sempre que enviar um novo código para seu repositório, você iniciará automaticamente um build e implantará seu serviço do App Engine.

Para mais informações sobre como criar gatilhos do Cloud Build, consulte [Como criar e gerenciar gatilhos de build](https://cloud.google.com/build/docs/automating-builds/create-manage-triggers).

## A seguir

- Saiba como [implantar no Cloud Run](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run).
- Saiba como [implantar no GKE](https://cloud.google.com/build/docs/deploying-builds/deploy-gke).
- Saiba como [implantar no Cloud Functions](https://cloud.google.com/build/docs/deploying-builds/deploy-functions).
- Saiba como [implantar no Firebase](https://cloud.google.com/build/docs/deploying-builds/deploy-firebase).
- Saiba como [resolver erros de build](https://cloud.google.com/build/docs/troubleshooting).



Isso foi útil?



Envie comentários