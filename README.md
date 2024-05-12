## RESUMO: Utilizando a Biblioteca Beautiful Soup para Web Scraping

1. **Introdução à Biblioteca Beautiful Soup**

   - Propósito: Extrair informações de páginas da web
   - Exemplo: Coletando dados do site do Mercado Livre
   > https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr/

2. **Definindo Valores para Extrair**

    1. Identificar os valores a serem extraídos (por exemplo, título, preço, avaliação)
   ![image](https://github.com/pedrowill-dev/bs4-crawler-mercadolivre/assets/110316192/d5747883-8c5d-4007-a2b7-d10071b276d7)


    2. Escolher uma classe pai que contenha esses valores
    ![image](https://github.com/pedrowill-dev/bs4-crawler-mercadolivre/assets/110316192/d62b2eac-58d1-4edc-b75f-b4754473edbc)


3. **Configurando o Código**

     1. Importar bibliotecas necessárias: Beautiful Soup, pandas, requests, regex
     ```py
      import re
      import requests
      import bs4
      import pandas as pd
     ```
     2. Fazer uma requisição para obter o conteúdo do site
    ```py
     response = requests.get(host).content
    ```

     
4. **Extraindo Informações**

    1. Recuperar informações de título para testar a funcionalidade
    2. Usar métodos como `find_all` para localizar elementos específicos com base em valores de classe
      ```py
      container_items = soup.find_all("div", class_="ui-search-result__content-wrapper")
      ```


6. **Processamento de Dados**

    1. Definir funções para processar e retornar os dados extraídos
    2. Utilizar regex para extrair informações de preço e avaliação
   ```py
      
    def add_product(texto):
      # Expressões regulares para extrair o título, preço e avaliação
      regex_preco = r'R\$(\d+\.?\d*)'
  
      # Encontrar correspondências usando regex
      titulo = texto.split('por ')[0]
      preco = re.search(regex_preco, texto).group(1)
      try:
          avaliacao =  re.search(r'\((\d+)\)', texto).group(1)
          avaliacao = avaliacao.replace('(', '').replace(')', '')
      except:
          avaliacao = 0
  
  
      return {
          'title': titulo,
          'price': float(preco),
          'assessment': int(avaliacao)
      }

   ```

7. **Criando um DataFrame**

   1. Armazenar os dados extraídos em uma lista para conversão em DataFrame
   ```py
     for item in container_items:

      data.append(add_product(item.text))
   ```
   2. Converter a lista em um DataFrame para facilitar a manipulação de dados
    ```py
    df = pd.DataFrame(data)
    ```
     

8. **Filtrar os dados**

    1. Realizar filtragem de dados (por exemplo, filtrar avaliações diferentes de zero)
    ```py
    df = df.query('assessment != 0')
    df = df.query('assessment >= 1.000')
    ```
    2. Conduzir análises adicionais ou métricas sobre os dados extraídos

9. **Conclusão**

    1. Apresentar o DataFrame gerado
    2. Demonstrar capacidades de filtragem e análise de dados
    3. Destacar o potencial para análises ou métricas adicionais utilizando os dados extraídos
