# Laboratorio-4-Projeto:
***
# Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados.
***
Este projeto é um dos laboratório do Bootcamp Microsoft Azure AI Fundamentals, promovido através da parceria entre a Microsoft e a DIO.

# Explore um índice do Azure AI Search (UI).
***

Vamos imaginar que você trabalha para a Fourth Coffee, uma rede nacional de cafés. Você foi solicitado a ajudar a criar uma solução de mineração de conhecimento que facilite a busca de insights sobre as experiências dos clientes. Você decide criar um índice do Azure AI Search usando dados extraídos de avaliações de clientes.

Neste laboratório você irá:

* Criar recursos do Azure.
* Extrair dados de uma fonte de dados.
* Enriqueça os dados com habilidades de IA.
* Utilize o indexador do Azure no portal do Azure.
* Consulte seu índice de pesquisa.
* Revise os resultados salvos em uma Loja de conhecimento.

# Recursos do Azure necessários:

A solução que você criará para o Fourth Coffee requer os seguintes recursos na sua assinatura do Azure:

* Um recurso do **Azure AI Search**, que gerenciará a indexação e a consulta.
* Um recurso **de serviços de IA do Azure**, que fornece serviços de IA para habilidades que sua solução de pesquisa pode usar para enriquecer os dados na fonte de dados com insights gerados por IA.
  
**Nota:** Os recursos do Azure AI Search e dos serviços Azure AI devem estar no mesmo local!
* Uma **conta de armazenamento** com contêineres de blobs, que armazenará documentos brutos e outras coleções de tabelas, objetos ou arquivos.

## Crie um recurso do _*Azure AI Search*_.

1. Entre no [portal do Azure](https://portal.azure.com/signin/index/)
2.  Clique no botão **+ Criar um recurso**, pesquise *Azure AI Search* e crie um recurso **Azure AI Search** com as seguintes configurações:
   * **Assinatura** : *sua assinatura do Azure*.
   * **Grupo de recursos**: *selecione ou crie um grupo de recursos com um nome exclusivo*.
   * **Nome do serviço**: *um nome exclusivo*.
   * **Localização**: *Escolha qualquer região disponível*.
   * **Nível de preços**: Básico.
3. Selecione **Review + create** e depois de ver a resposta **Validation Success**, selecione **Create**.
4. Após a conclusão da implantação, selecione **Ir para o recurso**. Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

## Crie um recurso de serviços de IA do Azure.

Você precisará provisionar um recurso **de serviços de IA do Azure** que esteja no mesmo local que seu recurso do Azure AI Search. Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

1. Retorne à página inicial do portal do Azure. Clique no botão **＋Criar um recurso** e pesquise os serviços de IA do Azure. Selecione **criar** um plano **de serviços de IA do Azure**. Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:

   * **Assinatura**: *sua assinatura do Azure*.
   * **Grupo de recursos**: *O mesmo grupo de recursos que seu recurso do Azure AI Search*.
   * **Região**: o mesmo local do recurso do Azure AI Search.
   * **Nome**: *Um nome exclusivo*.
   * **Nível de preços**: Padrão S0
   * **Ao marcar esta caixa, confirmo que li e compreendi todos os termos abaixo**: Selecionado.

     ## Crie uma conta de armazenamento

     1. Retorne à página inicial do portal do Azure e selecione o botão **+ Criar um recurso**.
     2. Procure conta de armazenamento e crie um recurso **de conta de armazenamento** com as seguintes configurações:

        * **Assinatura** : *sua assinatura do Azure*.
        * Grupo de recursos : *O mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI*.
        * **Nome da conta de armazenamento**: *um nome exclusivo*.
        * **Localização**: *Escolha qualquer localização disponível*.
        * Padrão de **desempenho**.
        * **Redundância** : armazenamento localmente redundante (LRS).
    3. Clique em **Revisar** e em **Criar**. Aguarde a conclusão da implantação e vá para o recurso implantado.
    4. Na conta de Armazenamento do Azure que você criou, no painel de menu esquerdo, selecione **Configuração** (em **Configurações**).
    5. Altere a configuração de *Permitir acesso anônimo de Blob* para **Habilitado** e selecione **Salvar**.

  # Carregar documentos para o armazenamento do Azure.
  1. No painel do menu esquerdo, selecione **Containers**.


  2. Selecione **+ Contêiner**. Um painel do seu lado direito é aberto.
  3. Insira as seguintes configurações e clique em **Criar**:
     
