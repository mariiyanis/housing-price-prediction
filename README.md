# Modelo Preditivo para Precificação de Imóveis Residenciais
Projeto desenvolvido na disciplina IMD3002 - Aprendizado de Máquina Supervisionado.

## Descrição
A precificação de imóveis é complexa e, portanto, depende de diversas variáveis como tamanho, localização, nº de quartos, ano de construção, etc. Sob esse viés, um modelo de Machine Learning pode ajudar a estimar preços de forma mais objetiva e precisa, sendo completamente relevante para imobiliárias, compradores e vendedores.

## Objetivo
O presente projeto tem o objetivo de desenvolver um modelo de Aprendizado de Máquina Supervisionado capaz de estimar o preço de venda de residências de forma objetiva e precisa. Para alcançar essa meta, utilizou-se uma abordagem de regressão univariada focada em decodificar o valor real das propriedades a partir de dados históricos disponíveis na base de dados Ames Housing.
  
## Metodologia
O projeto foi feito seguindo a metodologia CRISP-DM *(Cross Industry Standard Process for Data Mining)*. Confira como cada etapa foi aplicada:

### Entendimento do negócio
Inicialmente, delimitou-se o escopo da solução com a tarefa de regressão, onde a variável alvo escolhida foi o preço de venda (SalePrice). O foco do negócio visa entregar estimativas precisas de cotação para corretores, assim como garantir transparência aos vendedores e compradores sobre o que realmente valoriza o imóvel. Para validar esse desempenho, adotou-se a métrica RMSE (avaliada na escala monetária real) juntamente com o coeficiente R² para comprovar o distanciamento da predição frente à simples média.

### Compreensão dos dados
A investigação estatística preliminar apontou que a variável SalePrice não seguia um comportamento Gaussiano, caracterizando-se por uma assimetria positiva puxada pela presença de uma "cauda longa" de casas de alto padrão financeiro. Paralelamente a isso, o diagnóstico sobre as 79 features constatou inconsistências semânticas entre grandezas numéricas, ordinais qualitativas e puramente nominais. Além de tudo, detectou-se a existência de outliers estruturais sensíveis: casas com mais de 4.000 pés quadrados sendo comercializadas a preços desproporcionalmente baixos.

### Preparação dos dados
Como resposta à compreensão anterior, filtrou-se inicialmente a base descartando os imóveis outliers estruturais, para que não alavancassem uma falsa tendência durante o treino. Em seguida, o preço dos imóveis (SalePrice) passou por uma conversão algorítmica de base logarítmica (np.log1p) visando normalizar a sua dispersão. Para evitar erros de manipulação em diferentes bases e data leakage (vazamento), estruturou-se uma automação via Pipelines que aplicou imputação com mediana e redimensionamento por StandardScaler nos números, e um mapeamento customizado pelas codificações do OrdinalEncoder e OneHotEncoder para informações em formato de texto e hierarquia.

<img width="1044" height="484" alt="image" src="https://github.com/user-attachments/assets/a3d6a8c3-667d-40d9-bf61-e0263ad20530" />

### Modelagem
Visando garantir a fidelidade científica na reprodutibilidade do aprendizado, estabeleceu-se a divisão do dataset em 80% reservado para treino e 20% guardado exclusivamente para os testes simulados (mantendo um random_state fixo). Para cobrir amplas alternativas e ajustar hiperparâmetros, aplicou-se exaustivamente a validação cruzada (GridSearchCV com 5 folds) na base e testaram-se quatro estimadores fundamentais: Regressão Linear Simples, Ridge (Regularização L2), Lasso (Regularização L1) e Random Forest.

### Avaliação 
Os resultados provaram de maneira um pouco contraintuitiva frente aos ensaios baseados em árvores densas que a regressão Lasso superou drasticamente o modelo Random Forest. O Lasso obteve a métrica recordista de mínimo erro RMSE (avaliado em torno de 19.211 dólares) combinada ao coeficiente de explicação com aderência excepcional e superior a 0,92.

<img width="987" height="546" alt="image" src="https://github.com/user-attachments/assets/6e2a7bd2-af33-4e4c-bb82-2c398abde8f3" />


### Implantação
Embora não tenha resultado em um software de usuário final hospedado na nuvem, o projeto foi totalmente documentado e encapsulado dentro de um ColumnTransformer flexível e sustentável que retém os passos lógicos de forma sistêmica, firmando-se como um verdadeiro utilitário pronto para ingestão de novas informações de imobiliárias e implantação no futuro.

## Análise
Evidenciou-se que a conversão logarítmica e a preparação robusta induziram uma simplificação onde o padrão linear emergiu de maneira limpa. O algoritmo Lasso utilizou o seu mecanismo embutido de esparsidade para varrer os coeficientes irrelevantes (zerar features desnecessárias), provando que é capaz de lidar de maneira mais refinada com correlações cruzadas dezenas de vezes melhor que o peso bruto do ensaio do Random Forest.

<img width="925" height="701" alt="image" src="https://github.com/user-attachments/assets/c92d8127-704f-4198-8606-c3349c28e1c3" />

## Principais Insights
A análise revelou um insight valioso para o mercado imobiliário: investir na qualidade do acabamento agrega mais valor à propriedade do que simplesmente aumentar a sua metragem. Aqui, os modelos identificaram de forma empírica que a variável da qualidade geral do acabamento do imóvel (OverallQual) tem uma influência expressivamente superior comparada à variável tradicional de área habitável (GrLivArea), confrontando o mito no mercado de que em todos os casos o tamanho em pé quadrado determina um valor astronômico de avaliação.

<img width="1137" height="713" alt="image" src="https://github.com/user-attachments/assets/7d04d133-420d-4195-91a0-4bf0aa1ad2c7" />

## Tecnologias utilizadas
- **Python:** Linguagem principal do projeto;
- **Pandas e NumPy:** Manipulação, limpeza e processamento de dados tabulares;
- **Matplotlib e Seaborn:** Análise Exploratória de Dados e visualização gráfica;
- **Scikit-Learn:** Criação de Pipelines, pré-processamento (codificação e normalização) e treinamento dos modelos de regressão (Lasso, Ridge, Random Forest e Regressão Linear).
