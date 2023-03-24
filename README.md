# Desafio-Estagio-Refera
Pensando em não sobrecarregar nosso banco de dados transacional, precisamos ter um ambiente separado para analisar nossos dados sem grandes problemas. Assim, escreva um código local que faça uma extração total de todas as tabelas do banco de dados transactional e as carregue para o banco de dados analytics.

**Resolução do desafio abaixo**

# Acessa o contêiner do banco de dados transacional e exporta as tabelas
subprocess.run("docker exec -it refera-data-engineer-short-challenge_db_1 pg_dumpall -U dvdrental -f backup.sql", shell=True)

# Copia o arquivo SQL para o contêiner do banco de dados analytics
subprocess.run("docker cp backup.sql refera-data-engineer-short-challenge_analytics-db_1:/", shell=True)

# Acessa o contêiner do banco de dados analytics e importa as tabelas
subprocess.run("docker exec -it refera-data-engineer-short-challenge_analytics-db_1 psql -U analytics < backup.sql", shell=True)

# Remove o arquivo SQL
subprocess.run("rm backup.sql", shell=True)

print("Extração e carregamento de dados concluídos com sucesso!")
