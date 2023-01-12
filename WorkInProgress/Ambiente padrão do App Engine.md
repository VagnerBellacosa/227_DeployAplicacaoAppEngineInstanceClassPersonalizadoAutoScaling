- [App Engine ](https://cloud.google.com/appengine)
- [Documentação ](https://cloud.google.com/appengine/docs)
- [Ambiente padrão](https://cloud.google.com/appengine/docs/standard)

Isso foi útil?



Envie comentários

# Ambiente padrão do App Engine

bookmark_border



O ambiente padrão do App Engine tem como base instâncias de contêiner em execução na infraestrutura do Google. Os contêineres são pré-configurados com um dos diversos ambientes de execução disponíveis.

Com o ambiente padrão, fica mais fácil criar e implantar um aplicativo que será executado de maneira confiável, sob grande carga e com grandes quantidades de dados.

Os aplicativos são executados em um ambiente seguro e colocado no sandbox, permitindo que o ambiente padrão distribua solicitações em vários servidores e escalone servidores para atender às demandas de tráfego. O aplicativo é executado em seu próprio ambiente seguro e confiável, independentemente do hardware, sistema operacional ou localização física do servidor.

## Ambientes de execução e linguagens do ambiente padrão

O ambiente padrão é compatível com as seguintes linguagens:

[Go](https://cloud.google.com/appengine/docs/standard/go/runtime)

[Java](https://cloud.google.com/appengine/docs/standard/java-gen2/runtime)

[Node.js](https://cloud.google.com/appengine/docs/standard/nodejs/runtime)

[PHP](https://cloud.google.com/appengine/docs/standard/php-gen2/runtime)

[Python](https://cloud.google.com/appengine/docs/standard/python3/runtime)

[Ruby](https://cloud.google.com/appengine/docs/standard/ruby/runtime)

## Classes de instância

A *classe de instância* determina o volume de memória e CPU disponíveis para cada instância, a [cota gratuita](https://cloud.google.com/appengine/docs/standard/quotas#Instances) e o [custo por hora](https://cloud.google.com/appengine/pricing#standard_instance_pricing) depois que o app excede a cota gratuita.

Os limites de memória variam de acordo com a [geração do ambiente de execução](https://cloud.google.com/appengine/docs/standard/runtimes). Para todas as gerações do ambiente de execução, o limite de memória inclui a memória que seu app usa com a memória que o próprio ambiente de execução precisa para executar seu app. Os ambientes de execução do Java usam mais memória para executar o app do que outros.

Para modificar a classe de instância padrão, use a [configuração `instance_class`](https://cloud.google.com/appengine/docs/standard/python3/config/appref#instance_class) no arquivo `app.yaml` do app.

[Ambientes de execução de segunda geração](https://cloud.google.com/appengine/docs/standard/#ambientes-de-execução-de-segunda-geração)[Ambientes de execução de primeira geração](https://cloud.google.com/appengine/docs/standard/#ambientes-de-execução-de-primeira-geração)

Os ambientes de execução de segunda geração que usam essa especificação são: [Python 3](https://cloud.google.com/appengine/docs/standard/python3/runtime), [Java 11](https://cloud.google.com/appengine/docs/standard/java-gen2/runtime), [Node.js](https://cloud.google.com/appengine/docs/standard/nodejs/runtime), [PHP 7](https://cloud.google.com/appengine/docs/standard/php-gen2/runtime), [Ruby](https://cloud.google.com/appengine/docs/standard/ruby/runtime) e [Go 1.12+](https://cloud.google.com/appengine/docs/standard/go/runtime).

**Observação**: o ambiente de execução do [Go 1.11](https://cloud.google.com/appengine/docs/legacy/standard/go111) tem as mesmas especificações de classe de instância que os ambientes de execução de segunda geração.

| Classe da instância | Limite de memória | Limite de CPU | Tipos de dimensionamento compatíveis |
| :------------------ | :---------------- | :------------ | :----------------------------------- |
| F1 (padrão)         | 256 MB            | 600 MHz       | automático                           |
| F2                  | 512 MB            | 1,2 GHz       | automático                           |
| F4                  | 1.024 MB          | 2,4 GHz       | automático                           |
| F4_1G               | 2.048 MB          | 2,4 GHz       | automático                           |
| B1                  | 256 MB            | 600 MHz       | manual, básico                       |
| B2 (padrão)         | 512 MB            | 1,2 GHz       | manual, básico                       |
| B4                  | 1.024 MB          | 2,4 GHz       | manual, básico                       |
| B4_1G               | 2.048 MB          | 2,4 GHz       | manual, básico                       |
| B8                  | 2.048 MB          | 4,8 GHz       | manual, básico                       |

**Importante:** ao [visualizar seu faturamento](https://cloud.google.com/billing/docs/how-to/reports), você não verá os nomes das classes de instâncias individuais nos itens de linha de faturamento. Em vez disso, você vê as instâncias/hora das classes "B" informadas como "instâncias de back-end" e as instâncias/hora das classes "F" informadas como "instâncias de front-end". O faturamento se aplicará ao múltiplo apropriado de instâncias/hora para cada classe de instância que você tiver usado. Por exemplo, se você utilizar uma instância F4 por uma hora, verá o faturamento da "instância de front-end" referente a quatro instâncias/hora com a taxa F1.

## Cotas e limites

O ambiente padrão oferece 1 GiB de armazenamento de dados e tráfego gratuitos, que podem ser aumentados com a ativação de aplicativos pagos. No entanto, alguns recursos impõem limites não relacionados às cotas para proteger a estabilidade do sistema. Para mais detalhes sobre cotas, incluindo como editá-las para atender às suas necessidades, consulte [Cotas](https://cloud.google.com/appengine/docs/standard/quotas).

## Faça um teste 

Se você começou a usar o Google Cloud agora, crie uma conta para avaliar o desempenho do App Engine em situações reais. Clientes novos também recebem US$ 300 em créditos para executar, testar e implantar cargas de trabalho.

[Faça um teste gratuito do App Engine](https://console.cloud.google.com/freetrial)



Isso foi útil?



Envie comentários