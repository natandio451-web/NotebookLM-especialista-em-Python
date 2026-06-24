# NotebookLM-especialista-em-Python
Criei uma notebook com o que consegui encontrar sobre phyton e e com o Deep Research.

O assunto de interesse escolhido foi a linguagem de programação Phyton e suas diversas funcionaldades. Meus objetivos com essa notebook são explorar as funncionalidades oferecidas pela linguagem, bem como melhorar automações entre outros.

Fontes utilizadas:
https://blog.dsacademy.com.br/10-bibliotecas-python-para-automacao-e-web-scraping/
https://pythonacademy.com.br/blog/automatize-planilhas-excel-usando-python
https://revistas.uece.br/index.php/linguagememfoco/article/download/16081/13944/73333
https://docs.python.org/pt-br/3/tutorial/index.html

Alem dessas fontes utilizdas foram usadas mais 28 fontes encontradas com a função Deep Research,totalizando 32 fontes.

Prompts utilizados e respostas recebidas.

1. PROMPT:
Crie um codigo simples para web scraping de dados imobiliários e salvar em um banco SQL

RESPOSTA:
Com base nas nformações das fontes sobre a linguagem Python (atualmente na versão estável 3.14.6
), você pode criar um script utilizando bibliotecas populares como Requests para fazer as requisições HTTP
 e Beautiful Soup para extrair os dados do HTML
. Para o armazenamento, Python possui suporte nativo em sua biblioteca padrão para conexão com bancos de dados relacionais
.
Abaixo está um exemplo de código simples utilizando o banco de dados SQLite (que não requer instalação externa) e as bibliotecas de scraping mencionadas nas fontes:
import requests
from bs4 import BeautifulSoup
import sqlite3

# 1. Configuração do Banco de Dados SQL (usando sqlite3 da biblioteca padrão [5])
def setup_db():
    conn = sqlite3.connect('imoveis.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS imoveis (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            titulo TEXT,
            preco TEXT,
            link TEXT
        )
    ''')
    conn.commit()
    return conn

# 2. Função para Web Scraping (usando Requests e Beautiful Soup [3, 4])
def scrape_imoveis(url, conn):
    headers = {'User-Agent': 'Mozilla/5.0'}
    response = requests.get(url, headers=headers)
    
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Exemplo hipotético de seletores CSS (ajuste conforme o site real)
        anuncios = soup.find_all('div', class_='card-imovel')
        
        cursor = conn.cursor()
        for item in anuncios:
            titulo = item.find('h2').text.strip()
            preco = item.find('span', class_='preco').text.strip()
            link = item.find('a')['href']
            
            # Salvando no SQL
            cursor.execute('INSERT INTO imoveis (titulo, preco, link) VALUES (?, ?, ?)', 
                           (titulo, preco, link))
        
        conn.commit()
        print(f"Sucesso: {len(anuncios)} imóveis salvos.")
    else:
        print(f"Erro ao acessar o site: {response.status_code}")

# Execução
if __name__ == "__main__":
    db_connection = setup_db()
    # Substitua pela URL real que deseja raspar
    target_url = "https://exemplo-site-imobiliario.com.br/venda"
    scrape_imoveis(target_url, db_connection)
    db_connection.close()
Detalhes Importantes baseados nas fontes:
Segurança SQL: As fontes destacam que a nova versão Python 3.14 introduziu as t-strings (Template String Literals), que são um mecanismo nativo e seguro para processar strings de forma a evitar ataques de injeção de SQL
. No exemplo acima, utilizei a parametrização padrão (?), que é a prática recomendada para segurança.
Bibliotecas: Embora o Python tenha uma vasta biblioteca padrão ("batteries-included"
), bibliotecas de terceiros como Beautiful Soup e Requests (disponíveis via PyPI
) são o padrão da indústria para essas tarefas.
Desempenho: Para scraping em larga escala, o Python 3.14 agora oferece suporte oficial para free-threading, permitindo que tarefas de CPU sejam executadas em paralelo em múltiplos núcleos
.
Nota: As informações sobre a lógica específica de busca de elementos HTML (como find_all e seletores CSS) e a estrutura da URL são baseadas em conhecimentos gerais de programação, pois as fontes focam nas capacidades da linguagem e suas bibliotecas, e não em um site imobiliário específico. Recomenda-se sempre verificar os termos de uso do site antes de realizar o scraping.
