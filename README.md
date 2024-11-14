# Guia de Criação de Modelo de Previsão com Azure Machine Learning

Este guia fornece um passo a passo para criar um modelo de previsão usando o Azure Machine Learning. O exemplo utiliza dados históricos de aluguel de bicicletas para prever o número de aluguéis esperados em um determinado dia, com base em características sazonais e meteorológicas.

## 1. 🏗️ Criar um Workspace no Azure Machine Learning

Para usar o Azure Machine Learning, é necessário provisionar um workspace na sua assinatura do Azure.

### 📖 Passos:

1. Acesse o [portal do Azure](https://portal.azure.com) com suas credenciais Microsoft.
2. Selecione **+ Criar um recurso**, procure por **Machine Learning** e crie um novo recurso com as seguintes configurações:
   - **Assinatura**: Sua assinatura do Azure.
   - **Grupo de recursos**: Crie ou selecione um grupo de recursos.
   - **Nome**: Insira um nome único para seu workspace.
   - **Região**: Selecione a região geográfica mais próxima.
   - **Conta de armazenamento, Cofre de chaves, Application Insights**: Observe os recursos padrão que serão criados.
   - **Registro de contêiner**: Nenhum (um será criado automaticamente na primeira vez que você implantar um modelo).

3. Selecione **Revisar + criar** e depois **Criar**. Aguarde a criação do workspace e acesse o recurso implantado.
4. Selecione **Lançar estúdio** ou acesse [Azure Machine Learning studio](https://ml.azure.com) e faça login.

## 2. 🤖 Usar Machine Learning Automatizado para Treinar um Modelo

O machine learning automatizado permite testar múltiplos algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados.

### 📖 Passos:

1. No Azure Machine Learning studio, acesse a página **Automated ML**.
2. Crie um novo trabalho de ML automatizado com as seguintes configurações:

   - **Configurações básicas**:
     - Nome do trabalho: Nome único pré-preenchido.
     - Novo nome de experimento: `mslearn-bike-rental`
     - Descrição: `Automated machine learning for bike rental prediction`

   - **Tipo de tarefa e dados**:
     - Tipo de tarefa: Regressão
     - Conjunto de dados: Crie um novo conjunto de dados com:
       - Nome: `bike-rentals`
       - Descrição: `Historic bike rental data`
       - Tipo: Tabela (mltable)
       - Fonte de dados: Arquivos locais
       - Tipo de armazenamento de destino: Azure Blob Storage

3. Após criar o conjunto de dados, continue para enviar o trabalho de ML automatizado.

4. **Configurações da tarefa**:
   - Tipo de tarefa: Regressão
   - Coluna alvo: `rentals` (inteiro)
   - Métrica primária: `NormalizedRootMeanSquaredError`
   - Modelos permitidos: Selecione apenas `RandomForest` e `LightGBM`
   - Limites: Máximo de 3 testes, 3 testes simultâneos, 3 nós, e limite de pontuação de métrica de 0.085.

5. **Validação e teste**:
   - Tipo de validação: Divisão de treino-validação
   - Percentual de dados de validação: 10%

6. **Computação**:
   - Tipo de computação: Serverless
   - Tipo de máquina virtual: CPU
   - Tamanho da máquina virtual: `Standard_DS3_V2`
   - Número de instâncias: 1

7. Envie o trabalho de treinamento e aguarde a conclusão.

## 3. 📊 Revisar o Melhor Modelo

Após a conclusão do trabalho de ML automatizado, revise o melhor modelo treinado.

### 📖 Passos:

1. Na aba **Visão Geral** do trabalho de ML automatizado, observe o resumo do melhor modelo.
2. Selecione o nome do algoritmo para ver os detalhes.
3. Na aba **Métricas**, revise os gráficos de resíduos e valores previstos versus reais.

## 4. 🚀 Implantar e Testar o Modelo

Implante o modelo usando um endpoint em tempo real.

### 📖 Passos:

1. Na aba **Modelo** do melhor modelo, selecione **Implantar** e use a opção de endpoint em tempo real com as seguintes configurações:
   - Máquina virtual: `Standard_DS3_v2`
   - Contagem de instâncias: 3
   - Nome do endpoint: Deixe o padrão ou escolha um nome único globalmente.

2. Aguarde a implantação ser concluída.

3. Para testar o serviço implantado, acesse a aba **Testar** do endpoint em tempo real e insira os dados de entrada no formato JSON fornecido.

## 5. 🧹 Limpeza

Para evitar cobranças desnecessárias, exclua o endpoint e, se necessário, o workspace do Azure Machine Learning.

### 📖 Passos:

1. No Azure Machine Learning studio, na aba **Endpoints**, selecione o endpoint e exclua-o.
2. No portal do Azure, exclua o grupo de recursos associado ao workspace, se não for mais necessário.

---

Este guia fornece uma visão geral do processo de criação, treinamento, implantação e teste de um modelo de previsão usando o Azure Machine Learning. Siga os passos cuidadosamente para garantir uma implementação bem-sucedida.
