- [Cloud Shell ](https://cloud.google.com/shell)
- [Documentação ](https://cloud.google.com/shell/docs)
- [Guias](https://cloud.google.com/shell/docs/quickstart)

Isso foi útil?



Envie comentários

Guia de início rápido: implantar um aplicativo do App Engine usando o Cloud Shell

# Implantar um app do App Engine usando o Cloud Shell

bookmark_border

Nesta página, descrevemos como implantar rapidamente um app do App Engine usando um aplicativo de amostra usando o Cloud Shell.

## Antes de começar

1. 

2. 

3. 

4. Se você começou a usar o Google Cloud agora, [crie uma conta](https://console.cloud.google.com/freetrial) para avaliar o desempenho dos nossos produtos em situações reais. Clientes novos também recebem US$ 300 em créditos para executar, testar e implantar cargas de trabalho.

5. 

6. No console do Google Cloud, na página do seletor de projetos, selecione ou [crie um projeto do Google Cloud](https://cloud.google.com/resource-manager/docs/creating-managing-projects).

   **Observação**: se você não pretende manter os recursos criados neste procedimento, crie um projeto novo em vez de selecionar um que já existe. Depois de concluir essas etapas, é possível excluir o projeto. Para fazer isso, basta remover todos os recursos associados a ele.

   [Acessar o seletor de projetos](https://console.cloud.google.com/projectselector2/home/dashboard)

7. 

8. Verifique se o faturamento está ativado para seu projeto na nuvem. Saiba como [verificar se o faturamento está ativado em um projeto](https://cloud.google.com/billing/docs/how-to/verify-billing-enabled).

9. 

10. 

## Implementar um aplicativo

1. Na parte superior da janela do console do Google Cloud, clique em ![Ativar shell](https://cloud.google.com/static/shell/docs/images/activate_cloud_shell.svg) **Ativar o Cloud Shell**:

   Isso inicia a sessão do Cloud Shell em um frame na parte inferior do [Console do Google Cloud](http://console.cloud.google.com/).

2. Clone um aplicativo de amostra e execute-o localmente na sessão do Cloud Shell usando o servidor de desenvolvimento do App Engine:

   ```
   git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python \
   && cd appengine-guestbook-python \
   && dev_appserver.py ./app.yaml
   ```

3. Para se conectar ao servidor de desenvolvimento, clique em ![Visualização na Web](https://cloud.google.com/static/shell/docs/images/web_preview.svg) **Visualização na Web** e escolha **Visualizar na porta 8080**.

   O Cloud Shell abre o URL de visualização no serviço de proxy dele em uma nova janela do navegador.

4. Para abrir o editor de código, clique em ![Botão "Editor de código"](https://cloud.google.com/static/shell/docs/images/code_editor.svg) no menu do Cloud Shell para editar o app clonado.

5. Mude o texto em `index.html`:

   No editor de código, clique duas vezes em `index.html` para abrir o arquivo para edição e mudar o texto em `index.html` de *Uma pessoa anônima escreveu:* para *Um estranho misterioso disse:*

   Você verá a alteração na saída do Cloud Shell. Para ver as mudanças, atualize o app visualizado.

6. Parar o servidor de desenvolvimento:

   Após a visualização do aplicativo do App Engine, para interromper o servidor de desenvolvimento, pressione `Ctrl` + `C` na sessão do Cloud Shell.

7. Inicialize seu aplicativo do App Engine:

   Crie um aplicativo do App Engine vinculado ao seu projeto, caso ainda não tenha feito isso, e escolha a região:

   ```
   gcloud app create --project=[YOUR_PROJECT_NAME]
   ```

8. Implante o aplicativo no App Engine.

   ```
   gcloud app deploy ./index.yaml ./app.yaml
   ```

9. Abra o aplicativo no seu navegador da Web. O URL é `https://<PROJECT_ID>.<REGION-ID>.r.appspot.com/`.

   A implantação pode levar alguns minutos para ser concluída. Se o aplicativo não estiver totalmente implantado, uma mensagem de erro será exibida no navegador da Web. Atualize o navegador para ver o aplicativo implantado.

10. Para evitar o faturamento desnecessário, desative o aplicativo:

    Para desativar o aplicativo que você acabou de implantar, acesse o App Engine no console do Google Cloud e selecione **Configurações** > **Configurações do aplicativo** > **Desativar aplicativo**.

## A seguir

- [Escolha um ambiente do App Engine](https://cloud.google.com/appengine/docs/the-appengine-environments)