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

![storage-blob-1](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/f3ebf30b-2c99-42da-a93a-70d854946eb6)

  2. Selecione **+ Contêiner**. Um painel do seu lado direito é aberto.
  3. Insira as seguintes configurações e clique em **Criar**:

     * **Nome**: Coffee-Reviews.
     * **Nível de acesso público**: Container (acesso de leitura anônimo para containers e blobs).
     * **Avançado**: *sem alterações*.

  4. Em uma nova guia do navegador, baixe as avaliações de café compactadas em `https://aka.ms/mslearn-coffee-reviews` e extraia os arquivos para a pasta de *avaliações*.
  5. No portal do Azure, selecione o contêiner de *avaliações de café*. No contêiner, selecione **Carregar**.
     
![storage-blob-3](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/902fa1d5-9084-4136-91ad-52b306f7dfa7)

 7. No painel **Carregar blob**, selecione **Selecionar um arquivo**.
 8. Na janela do Explorer, selecione **todos** os arquivos na pasta de avaliações , selecione **Abrir** e, em seguida, selecione **Carregar**.

![azure-search-wizard-1](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/b9842bd6-9197-4ded-9cf0-d4340d1cf95b)

 8. Depois que o upload for concluído, você poderá fechar o painel **Upload blob**. Seus documentos estão agora em seu contêiner de armazenamento de avaliações de café.

# Indexar os documentos

Depois de armazenar os documentos, você poderá usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um *assistente de importação de dados*. Com este assistente, você pode criar automaticamente um índice e um indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

1. No portal do Azure, navegue até o recurso Azure AI Search. Na página **Visão geral**, selecione **Importar dados**.

   ![6a-azure-container-upload-files](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/5b22b8d1-1631-43ca-ad48-3fd9d75bd26a)

2. Na página **Conectar-se aos seus dados**, na lista **Fonte de Dados** , selecione **Azure Blob Storage**. Preencha os detalhes do armazenamento de dados com os seguintes valores:

   * **Fonte de dados**: Armazenamento de Blobs do Azure.
   * **Nome da fonte de dados**: coffee-customer-data.
   * **Dados a extrair**: Conteúdo e metadados.
   * **Modo de análise**: Padrão.
   * **Cadeia de conexão**: *Selecione **Escolha uma conexão existente**. Selecione sua conta de armazenamento, selecione o contêiner **de avaliações de café** e clique em **Selecionar**.
   * **Autenticação de identidade gerenciada**: Nenhuma.
   * **Nome do contêiner**: *esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente*.
   * **Pasta Blob**: *deixe em branco*.
   * **Descrição**: Avaliações sobre Fourth Coffee Shops.

3. Selecione **Próximo: Adicionar habilidades cognitivas (opcional)**.
4. Na secção **Anexar Serviços Cognitivos**, selecione o seu recurso de serviços Azure AI.
5. Na seção **Adicionar enriquecimentos**:

   * Altere o **nome da qualificação** para **coffee-skillset**.
   * Marque a caixa de seleção **Habilitar OCR e mesclar todo o texto no campo merged_content**.

     ! **Nota** É importante selecionar **Habilitar OCR** para ver todas as opções de campo enriquecido.

     * Certifique-se de que o **campo Dados de origem** esteja configurado como **merged_content**.
     * Altere o **nível de granularidade de enriquecimento** para **Páginas (blocos de 5.000 caracteres)**.
     * Não selecione *Habilitar enriquecimento incremental*.
     * Selecione os seguintes campos enriquecidos:
       
Habilidade | Parâmetro | Nome do Campo
---|---|---|
Extraia nomes de locais|---| Localizações
Extraia frases-chave|---| frases chaves
Detectar |---| sentimento
Gerar tags de imagens |---| imagemTags
Gere legendas|---| legenda da imagem

6. Em **Salvar enriquecimentos em um armazenamento de conhecimento**, selecione:

* Projeções de imagem.
* Documentos.
* Páginas.
* Frases chave.
* Entidades.
* Detalhes da imagem.
* Referências de imagem.
  
  ! **Nota** Se aparecer um aviso solicitando uma **cadeia de conexão de conta de armazenamento**.

a. Selecione **Escolha uma conexão existente**. Escolha a conta de armazenamento que você criou anteriormente.

b. Clique em + **Container** para criar um novo contêiner chamado **armazenamento de conhecimento** com o nível de privacidade definido como Private e selecione Create.

c. Selecione o contêiner **de armazenamento de conhecimento** e clique em **Selecionar** na parte inferior da tela.
  
7. Selecione **projeções de blob do Azure: Documento**. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento . Não altere o nome do contêiner.
8. Selecione **Próximo: Personalizar índice de destino** . Altere o **nome do índice** para **coffee-index**.
9. Certifique-se de que a **chave** esteja configurada como **metadata_storage_path**. Deixe **o nome do sugeridor** em branco e **o modo de pesquisa** preenchido automaticamente.
10. Revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão.

![6a-azure-cognitive-search-customize-index](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/66a6ddb2-9e4a-453a-ad1d-035812798af9)

11. Selecione **Próximo: Criar um indexador**.
12. Altere o **nome do indexador** para **coffee-indexer**.
13. Deixe a **programação** definida como **Once**.
14. Expanda as **opções avançadas**. Certifique-se de que a opção **Base-64 Encode Keys** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.
15. Selecione **Enviar** para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:

    * Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
    * Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
    * Mapeia os campos extraídos para o índice.

16. Volte à página de recursos do Azure AI Search. No painel esquerdo, em **Gerenciamento de pesquisa**, selecione **Indexadores**. Selecione o **indexador de café** recém-criado. Espere um minuto e selecione ↻ Atualize até que o **Status** indique sucesso.
17. Selecione o nome do indexador para ver mais detalhes.

![6a-search-indexer-success](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/5bbfdcb7-f3f7-4ec8-9e75-9b14ca058589)

# Consultar o índice

Use o Search Explorer para escrever e testar consultas. O explorador de pesquisa é uma ferramenta incorporada no portal do Azure que oferece uma maneira fácil de validar a qualidade do seu índice de pesquisa. Você pode usar o Search Explorer para escrever consultas e revisar resultados em JSON.

1. Na *página Visão geral* do serviço de pesquisa , selecione **Explorador de pesquisa** na parte superior da tela.

![5-exercise-screenshot-7](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/dbff777c-19e5-4779-90d2-fdf58005d755)

2. Observe como o índice selecionado é o índice de café que você criou. Abaixo do índice selecionado, altere a visualização para **JSON view**.

![search-explorer-query](https://github.com/WaldeniseMoraes/Laboratorio-4-consulta-de-dados/assets/161647255/175228ba-adcb-4f65-a498-4d6f1d91bde1)

No campo **do editor de consultas JSON**, copie e cole:
```
{
    "search": "*",
    "count": true
}
```

1. Selecione **Pesquisar** . A consulta de pesquisa retorna todos os documentos no índice de pesquisa, incluindo uma contagem de todos os documentos no campo **@odata.count**. O índice de pesquisa deve retornar um documento JSON contendo os resultados da pesquisa.
2. Agora vamos filtrar por localização. No campo **do editor de consultas JSON** , copie e cole:
```
  {
 "search": "locations:'Chicago'",
 "count": true
}
```

3. Selecione **Pesquisar** . A consulta pesquisa todos os documentos no índice e filtra revisões com localização em Chicago. Você deveria ver 3 no `@odata.count`campo.
4. Agora vamos filtrar por sentimento. No campo **do editor de consultas JSON** , copie e cole:
```
{
 "search": "sentiment:'negative'",
 "count": true
}
```

5. Selecione Pesquisar . A consulta pesquisa todos os documentos no índice e filtra revisões com sentimento negativo. Você deveria ver 1 no `@odata.count`campo.

! **Nota** Veja como os resultados são classificados por `@search.score`. Esta é a pontuação atribuída pelo mecanismo de pesquisa para mostrar o quão próximos os resultados correspondem à consulta fornecida.

6. Um dos problemas que podemos querer resolver é por que pode haver certas avaliações. Vamos dar uma olhada nas frases-chave associadas à avaliação negativa. O que você acha que pode ser a causa da revisão?

# Revise o armazenamento de conhecimento

Vamos ver o poder do armazenamento de conhecimento em ação. Ao executar o assistente Importar dados , você também criou um armazenamento de conhecimento. Dentro do armazenamento de conhecimento, você encontrará os dados enriquecidos extraídos pelas habilidades de IA que persistem na forma de projeções e tabelas.

1. No portal do Azure, navegue de volta para a sua conta de armazenamento do Azure.
2. No painel do menu esquerdo, selecione Containers . Selecione o contêiner **de armazenamento de conhecimento**.
