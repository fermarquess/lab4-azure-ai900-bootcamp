# Desafio de projeto

## Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

Projeto desenvolvido no Bootcamp Microsoft Azure AI Fundamentals - oferecido pela **[Digital Innovation One](https://www.dio.me/)**.

Para a prática deste laboratório é necessário ter uma conta na Microsoft e acessar o portal [Azure](https://portal.azure.com/).

## Passos

1. Realizar o login no portal [Azure](https://portal.azure.com/).

Então será necessário criar os seguintes resources: 

- Azure AI Search
- Azure AI services
- Storage account


2. Clicar em **Create Resource**, em **AI + Machine Learning** buscar por **Azure AI Search**.

   ![image](https://github.com/fermarquess/lab2-azure-ai900-bootcamp/assets/100250814/f8e55bf3-a8b9-4f45-aaf5-a0dbf81d4363)

   ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/e40bdcd9-dd92-4a24-a3d1-739eb97f30c3)

3. Ao clicar em **Create**, inserir as informações solicitadas nos campos:
    - Subscription
    - Resource group
    - Service name
    - Location
    - Pricing tier: Basic
  ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/4f313d3d-e58f-46d1-864d-b7f31ab27206)

Selecionar **Review + Create**, assim que validado, clicar em **Create**
Depois disso já é possível acessar o que foi criado clicando em **Go to resources**.

4. Clicar em **Create Resource**, em **AI + Machine Learning** buscar por **Azure AI services**.

![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/40fe4bb2-4439-4901-b567-8e7dc497962a)

5. Ao clicar em **Create**, inserir as informações solicitadas nos campos:
  - Subscription
  - Resource group: O mesmo que foi selecionado no Azure AI Search resource.
  - Region: O mesmo que foi selecionado no Azure AI Search resource.
  - Name
  - Pricing tier: Standard S0
  - Selecionar a box que pergunta sobre seu conhecimento sobre os termos de uso.

Selecionar **Review + Create**, assim que validado, clicar em **Create**
Depois disso já é possível acessar o que foi criado clicando em **Go to resources**.

6. Na **Home** basta localizar e clicar em **Storage accounts** 

  ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/7958ea92-bdef-4447-a3a6-e8646791f872)

7. Clicando em **Create** será necessário inserir as seguintes informações:
  - Subscription
  - Resource group: O mesmo que foi selecionado no Azure AI Search e no Azure AI services resources.
  - Storage account name
  - Location
  - Performance: Standard
  - Redundancy: Locally redundant storage (LRS)

Clicar em **Review** e **Create**, o deploy começará e assim que for concluído, selecionar no menu no canto esquerdo da tela a opção **Configuration (under Settings)**. 
Alterar o Allow Blob para acesso anônimo e então Salvar.

A partir deste momento já é possível Subir Documentos para Azure Storage.

8. Para fazer o Upload de documentos é necessário clicar em **Containers** no menu no canto esquerdo da tela. Clicar em **+Containers**, assim um painel vai aparecer no canto direito da tela.
9. Inserir as seguintes configurações para este teste:
  - Name: coffee-reviews
  - Public access level: Container (anonymous read access for containers and blobs)
  - Advanced: no changes

10. Fazer o download dos arquivos disponíveis para este teste em https://aka.ms/mslearn-coffee-reviews e extrair os arquivos de Review na pasta.
11. No portal Azure selecionar o container coffee-reviews clicar em **Upload**, selecionar os arquivos de review. Assim que o upload for concluído os documentos já estarão disponíveis no storage container.
12. Após os documentos já estarem disponíveis, agora é possível utilizar o Azure AI Search para extrair insights destes documentos. Assim podemos criar um Index e um indexer para dar suporte aos dados em estoque.
13. No portal Azure, em **Resource groups** clicar na principal resource e então em Overview selecionar o Azure AI Search que foi criado, clicar em **Import Data**

![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/0af72e13-b4c5-4192-bd9d-426202324014)

![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/f11432bc-77e5-4721-902a-7e3f76179080)

14.Para conectar a página que está na lista de data sources é necessário selecionar select Azure Blob Storage e inserir os seguintes valores:
 
 - Data Source: Azure Blob Storage
 - Data source name: coffee-customer-data
 - Data to extract: Content and metadata
 - Parsing mode: Default
 - Connection string: Selecionar o coffee-reviews container e clicar em Select.
 - Managed identity authentication: None
 - Container name: this setting is auto-populated after you choose an existing connection.
 - Blob folder: Deixar em branco.
 - Description: Reviews for Fourth Coffee shops.

15. Selecionar **Next: Add cognitive skills (Optional).**
16. Na seção **Attach Cognitive Services** selecionar a Azure AI services resource que foi criada.
17. Na seção **Add enrichments**:

    - Mudar o Skillset name para coffee-skillset
    - Selecionar a checkbox OCR e o merge all text tem que estar como merged_content.
    - Mudar o Enrichment granularity level to Pages (5000 character chunks).
    - Não selecionar Enable incremental enrichment.
    - Selecionar os campos:   Extract location names	 	locations
                              Extract key phrases	 	keyphrases
                              Detect sentiment	 	sentiment
                              Generate tags from images	 	imageTags
                              Generate captions from images	 	imageCaption

18. Em **Save enrichments to a knowledge store**, selecionar:
 - Image projections
 - Documents
 - Pages
 - Key phrases
 - Entities
 - Image details
 - Image references

   Se houver uma mensagem de atenção em Storage Account Connection String será necessário fazer os seguintes ajustes:

   - Selecionar uma conexão existente. Selecionar a storage account criada antes.
   - Clicar em + Container para criar um novo container chamado knowledge-store com nível de privacidade Private, e selecionar Create.
   - Então selecionar o knowledge-store container e clicar em Select.

19. Selecionar **Azure blob projections: Document**
20. Selecionar **Next: Customize target index**. Mudar o Index name para coffee-index.
21. Garantir que **Key** esteja como metadata_storage_path. Deixar o Suggester name em branco and Search mode auto.
22. Revisar os campos de Index e selecionar **filterable**
23. Selecionar **Next: Create an indexer.** e mudar o Indexer name para coffee-indexer.
24. Deixar o Schedule em Once.
25. Nas opções avançadas garantir que Base-64 Encode Keys está selecionado.
26. Selecionar **Submit**
27. Voltar para a página da resource, selecionar Indexers e clicar em coffee-indexer para verificar os detalhes

    ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/1f6c604e-0515-4c48-8553-5195741d1eae)
    ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/81773d6c-6ad0-4578-b9f2-b44ed350a85b)


28. Agora é possível buscar no explorador para escrever e testar queries, para isso:

    - Clicar em Search explorer e ajustar a view para JSON view e inserir alguns testes como estes abaixo, que podem ser encontrados na [documentação oficial](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html) deste lab:

    ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/177bcb07-30cc-4409-9f64-f0303cad90bf)
    1

    ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/9291bfef-65ba-4783-b835-98c7025873aa)
    2

    ![image](https://github.com/fermarquess/lab4-azure-ai900-bootcamp/assets/100250814/39c486bf-56fe-4ef3-beb2-0b934e37b4c5)
    3
----------------------------------------------------------------------------------------------------

💡  **Principais insights deste teste**:

 - A ferramenta de AI Search pode ser muito útil para trazer agilidade na análise de documentos.
 - Personalização da pesquisa conforme as necessidades de cada projeto.
 - Para grandes quantidades de dados pode ser uma ferramenta muito útil em outras aplicações.  
