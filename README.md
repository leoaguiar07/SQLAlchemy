![capa](https://a.neko.red/mk/dcf3c9e6-7972-45dc-bffa-ab32bea347b6.jpg)

## O que é SQLAlchemy?
SQLAlchemy é o kit de ferramentas Python SQL que permite aos desenvolvedores acessar e gerenciar bancos de dados SQL usando a linguagem de domínio Pythonic. Você pode escrever uma consulta na forma de uma string ou cadeia de objetos Python para consultas semelhantes. Trabalhar com objetos proporciona flexibilidade aos desenvolvedores e permite que eles criem aplicativos baseados em SQL de alto desempenho.

Em palavras simples, permite aos usuários conectar bancos de dados usando a linguagem Python, executar consultas SQL usando programação baseada em objetos e agilizar o fluxo de trabalho.

### Instalar SQLAlchemy
É bastante fácil instalar o pacote e começar a codificar.

Você pode instalar o SQLAlchemy usando o Python Package Manager (pip):

```python
pip install sqlalchemy
```
• Este código instala o pacote Python sqlalchemy usando o gerenciador de pacotes pip.<br>
• pip é uma ferramenta para instalar e gerenciar pacotes Python, e sqlalchemy é um pacote que fornece um conjunto de API de alto nível para conexão com bancos de dados relacionais e execução de operações SQL.<br>
• O comando install é usado para baixar e instalar o pacote do repositório Python Package Index (PyPI).<br>

Caso você esteja usando a distribuição Anaconda do Python, tente inserir o comando no terminal conda:

```python
conda install -c anaconda sqlalchemy
```
• Este código instala o pacote SQLAlchemy usando o gerenciador de pacotes Anaconda.
• A flag -c especifica o canal a partir do qual instalar o pacote, que neste caso é o canal anaconda.
• O comando conda é usado para gerenciar pacotes e ambientes no Anaconda.

Vamos verificar se o pacote foi instalado com sucesso:

```python
 import sqlalchemy
 sqlalchemy.__version__
```
`
'1.4.41'
`

• Este código importa a biblioteca SQLAlchemy e depois imprime o número da versão da biblioteca.<br>
• A primeira linha importa a biblioteca SQLAlchemy, que é um kit de ferramentas SQL do Python e uma biblioteca de mapeamento relacional de objetos (ORM).<br>
• A segunda linha acessa o atributo __version__ do módulo sqlalchemy e imprime seu valor, que é o número da versão da biblioteca.<br>
• Neste caso, o número da versão é '1.4.41' <br>

Excelente, instalamos com sucesso o SQLAlchemy versão 1.4.41.

## Conectar bancos de dados SQLite, criar objetos de tabela e usá-los para executar a consulta SQL

###  Conectando o banco de dados
Usaremos o banco de dados SQLite do Futebol Europeu do Kaggle, que possui duas tabelas: divisões e partidas.

Primeiro, criaremos objetos do mecanismo SQLite usando ‘create_object’ e passaremos o endereço de localização do banco de dados. Em seguida, criaremos um objeto de conexão conectando o mecanismo. Usaremos o objeto ‘conn’ para executar todos os tipos de consultas SQL.

```python
from sqlalchemy as db
engine = db.create_engine("sqlite:///european_database.sqlite")

conn = engine.connect() 
```
Se você deseja conectar bancos de dados PostgreSQL, MySQL, Oracle e Microsoft SQL Server, verifique a configuração do mecanismo para obter uma conectividade suave com o servidor.

Este tutorial SQLAlchemy pressupõe que você entenda os fundamentos de Python e SQL. Se não, então está perfeitamente bem. Você pode fazer o curso de habilidades de Fundamentos de SQL e Fundamentos de Python para construir uma base sólida.

### Acessando a tabela
Para criar um objeto de tabela, precisamos fornecer nomes de tabelas e metadados. Você pode produzir metadados usando a função `MetaData()` do SQLAlchemy.

```python
metadata = db.MetaData() #extracting the metadata
division= db.Table('divisions', metadata, autoload=True, 
autoload_with=engine) #Table object
```

Vamos imprimir os metadados  “divisions”. 

```python
print(repr(metadata.tables['divisions']))
```
 
Os metadados contêm o nome da tabela, nomes das colunas com o tipo e esquema.

`
Table('divisions', MetaData(), Column('division', TEXT(), table=<divisions>), 
Column('name', TEXT(), table=<divisions>), Column('country', TEXT(), 
table=<divisions>), schema=None)
`
Vamos usar o objeto de tabela “division” para imprimir nomes de colunas.

```python
print(division.columns.keys())
```
 
A tabela é composta pelas colunas: division, name, and country. 

`
['division', 'name', 'country']
`

### Consulta SQL simples
Agora vem a parte divertida. Usaremos o objeto table para executar a consulta e extrair os resultados.

No código abaixo, estamos selecionando todas as colunas da tabela “division”.

```python
query = division.select() #SELECT * FROM divisions
print(query)
```

Nota: você também pode escrever o comando select assim:  ‘db.select([division])’

Para visualizar a consulta, imprima o objeto de consulta e ele mostrará o comando SQL.


`SELECT divisions.division, divisions.name, divisions.country
FROM divisions`

Resultado da consulta SQL
Agora executaremos a consulta usando o objeto de conexão e extrairemos as primeiras cinco linhas.

• fetchone(): it will extract a single row at a time.
• fetchmany(n): it will extract the n number of rows at a time.
• fetchall(): it will extract all of the rows.  

```python
exe = conn.execute(query) #executing the query
result = exe.fetchmany(5) #extracting top 5 results
print(result)
```

O resultado mostra as primeiras cinco linhas da tabela.


`[('B1', 'Division 1A', 'Belgium'), ('D1', 'Bundesliga', 'Deutschland'), ('D2', '2. Bundesliga', 'Deutschland'), ('E0', 'Premier League', 'England'), ('E1', 'EFL Championship', 'England')]`

 
### Exemplos de SQLAlchemy em Python
Nesta seção, veremos vários exemplos de SQLAlchemy para criação de tabelas, inserção de valores, execução de consultas SQL, análise de dados e gerenciamento de tabelas.

### Criando Tabelas
Primeiro, criaremos um novo banco de dados chamado “datacamp.sqlite”. 
O create_engine criará um novo banco de dados automaticamente se não houver nenhum banco de dados com o mesmo nome. Portanto, criar e conectar são bastante semelhantes.

Depois disso, conectaremos o banco de dados e criaremos um objeto de metadados.

Usaremos a função Table do SQLAlchmy para criar uma tabela chamada “Student”

Consiste em colunas:

Id: Integer and primary key
Name: String and non-nullable 
Major: String and default = “Math”
Pass: Boolean and default =True 
We have created the structure of the table. Let’s add it to the database using `metadata.create_all(engine)`.

```python
engine = db.create_engine('sqlite:///datacamp.sqlite')
conn = engine.connect()
metadata = db.MetaData()

Student = db.Table('Student', metadata,
              db.Column('Id', db.Integer(),primary_key=True),
              db.Column('Name', db.String(255), nullable=False),
              db.Column('Major', db.String(255), default="Math"),
              db.Column('Pass', db.Boolean(), default=True)
              )

metadata.create_all(engine) 
```

### Inserir
Para adicionar uma única linha, primeiro usaremos `insert` e adicionaremos o objeto tabela. Depois disso, use `values` e adicione valores às colunas manualmente. Funciona de forma semelhante a adicionar argumentos às funções Python.

Por fim, executaremos a consulta usando a conexão para executar a função.

```python
query = db.insert(Student).values(Id=1, Name='Matthew', Major="English", Pass=True)
Result = conn.execute(query)
```

Vamos verificar se adicionamos a linha à tabela “Aluno” executando uma consulta de seleção e buscando todas as linhas.

```python
output = conn.execute(Student.select()).fetchall()
print(output)
```

Adicionamos os valores com sucesso.


`1, 'Matthew', 'English', True)]`


### Insira muitos
Adicionar valores um por um não é uma forma prática de preencher o banco de dados. Vamos adicionar vários valores usando listas.

Crie uma consulta de inserção para a tabela Aluno.
Crie uma lista de várias linhas com nomes e valores de colunas.
Execute a consulta com um segundo argumento como lista_valores.

```python
query = db.insert(Student)
values_list = [{'Id':'2', 'Name':'Nisha', 'Major':"Science", 'Pass':False},
              {'Id':'3', 'Name':'Natasha', 'Major':"Math", 'Pass':True},
              {'Id':'4', 'Name':'Ben', 'Major':"English", 'Pass':False}]
Result = conn.execute(query,values_list)
```

Para validar nossos resultados, execute a consulta de seleção simples.

```python
output = conn.execute(db.select([Student])).fetchall()
print(output)
```


A tabela agora contém mais linhas.


`[(1, 'Matthew', 'English', True), (2, 'Nisha', 'Science', False), (3, 'Natasha', 'Math', True), (4, 'Ben', 'English', False)]`


### Consulta SQL simples com SQLAlchemy
Em vez de usar objetos Python, também podemos executar consultas SQL usando String.

Basta adicionar o argumento como uma String à função `execute` e visualizar o resultado usando `fetchall`.


```python
output = conn.execute("SELECT * FROM Student")
print(output.fetchall())
```

`[(1, 'Matthew', 'English', 1), (2, 'Nisha', 'Science', 0), (3, 'Natasha', 'Math', 1), (4, 'Ben', 'English', 0)]`


Você pode até passar consultas SQL mais complexas. No nosso caso, estamos selecionando as colunas Nome e Principais onde os alunos foram aprovados no exame.
```python
output = conn.execute("SELECT Name, Major FROM Student WHERE Pass = True")
print(output.fetchall())
```

`[('Matthew', 'English'), ('Natasha', 'Math')]`

### Usando API SQLAlchemy
Nas seções anteriores, usamos API/objetos SQLAlchemy simples. Vamos mergulhar em consultas mais complexas e com várias etapas.

No exemplo abaixo, selecionaremos todas as colunas onde a especialização do aluno é Inglês.

```python
query = Student.select().where(Student.columns.Major == 'English')
output = conn.execute(query)
print(output.fetchall())
```

`[(1, 'Matthew', 'English', True), (4, 'Ben', 'English', False)]`


Vamos aplicar a lógica AND à consulta WHERE.

No nosso caso, procuramos alunos com especialização em inglês e que foram reprovados.

Nota: diferente de ‘!=’ Verdadeiro é Falso.

```python
query = Student.select().where(db.and_(Student.columns.Major == 'English', Student.columns.Pass != True))
output = conn.execute(query)
print(output.fetchall())
```
Apenas Ben foi reprovado no exame com especialização em inglês.


`[(4, 'Ben', 'English', False)]`


Usando uma tabela semelhante, podemos executar todos os tipos de comandos, conforme mostrado na tabela abaixo.

Você pode copiar e colar esses comandos para testar os resultados por conta própria. Confira o espaço de trabalho do DataCamp se você ficar preso em algum dos comandos fornecidos.


`in`

Student.select().where(Student.columns.Major.in_(['English','Math']))

`and, or, not`

Student.select().where(db.or_(Student.columns.Major == 'English', Student.columns.Pass = True))

`order by`

Student.select().order_by(db.desc(Student.columns.Name))

`limit`

Student.select().limit(3)

`sum, avg, count, min, max`

db.select([db.func.sum(Student.columns.Id)])

`group by`

db.select([db.func.sum(Student.columns.Id),Student.columns.Major]).group_by(Student.columns.Pass)

`distinct`

db.select([Student.columns.Major.distinct()])

Para aprender sobre outras funções e comandos, verifique a documentação oficial da API de instruções e expressões SQL.

## Saída para Pandas DataFrame
Cientistas e analistas de dados apreciam os dataframes do pandas e adorariam trabalhar com eles. Nesta parte, aprenderemos como converter o resultado de uma consulta SQLAlchemy em um dataframe do pandas.

Primeiro, execute a consulta e salve os resultados.

```python
query = Student.select().where(Student.columns.Major.in_(['English','Math']))
output = conn.execute(query)
results = output.fetchall()
```

Em seguida, use a função DataFrame e forneça os resultados SQL como argumento. Por fim, adicione os nomes das colunas usando o resultado da primeira linha `results[0]` e `.keys()`

Nota: você pode fornecer qualquer linha válida para extrair os nomes das colunas usando `keys()`
Abrir no Google Tradutor
```python
data = pd.DataFrame(results)
data.columns = results[0].keys()
data
```
### Análise de dados com SQLAlchemy
Nesta parte iremos conectar a base de dados do futebol europeu e realizar consultas complexas e visualizar os resultados.

Conectando duas tabelas
Como de costume, conectaremos o banco de dados usando as funções `create_engine` e `connect`.

No nosso caso estaremos juntando duas tabelas, então temos que criar dois objetos de tabela: divisão e correspondência.
```python
engine = create_engine("sqlite:///european_database.sqlite")
conn = engine.connect()
metadata = db.MetaData()
division = db.Table('divisions', metadata, autoload=True, autoload_with=engine)
match = db.Table('matchs', metadata, autoload=True, autoload_with=engine)
```
### Executando uma consulta complexa 
 1. Selecionaremos colunas de divisão e correspondência.
 2. Junte-os usando uma coluna comum: division.division e match.Div. 
 3. Selecione todas as colunas onde a divisão é E1 e a temporada é 2009. 
 4.Ordene o resultado por HomeTeam. Você pode até criar consultas mais complexas adicionando módulos adicionais.

Nota: para juntar automaticamente duas tabelas você também pode usar: db.select([division.columns.division,match.columns.Div])
```python
query = db.select([division,match]).\
select_from(division.join(match,division.columns.division == match.columns.Div)).\
where(db.and_(division.columns.division == "E1", match.columns.season == 2009 )).\
order_by(match.columns.HomeTeam)
output = conn.execute(query)
results = output.fetchall()

data = pd.DataFrame(results)
data.columns = results[0].keys()
data
```
### Análise de dados com SQLAlchemy

Após executar a consulta, convertemos o resultado em um dataframe do pandas.

Ambas as tabelas estão unidas e os resultados mostram apenas a divisão E1 da temporada de 2009 ordenada pela coluna HomeTeam.

### Visualização de dados
Agora que temos um dataframe, podemos visualizar os resultados na forma de um gráfico de barras usando Seaborn.

1. Defina o tema como “whitegrid”
2. Redimensione o tamanho da visualização para 15X6
3. Gire os ticks do eixo x para 90
4. Defina paletas de cores para “pastéis”
5. Trace um gráfico de barras de "HomeTeam" vs "FTHG" com a cor Azul.
6. Trace um gráfico de barras de "HomeTeam" vs "FTAG" com a cor vermelha.
7. Exiba a legenda no canto superior esquerdo.
8. Remova os rótulos x e y.
9. Despine para a esquerda e para baixo.
O objetivo principal desta parte é mostrar como você pode usar a saída da consulta SQL e criar visualizações de dados incríveis.

```python
import seaborn as sns
import matplotlib.pyplot as plt
sns.set_theme(style="whitegrid")

f, ax = plt.subplots(figsize=(15, 6))
plt.xticks(rotation=90)
sns.set_color_codes("pastel")
sns.barplot(x="HomeTeam", y="FTHG", data=data,
            label="Home Team Goals", color="b")

sns.barplot(x="HomeTeam", y="FTAG", data=data,
            label="Away Team Goals", color="r")
ax.legend(ncol=2, loc="upper left", frameon=True)
ax.set(ylabel="", xlabel="")
sns.despine(left=True, bottom=True)
```

### Visualização de dados com SQLAlchemy
Salvando resultados em CSV
Depois de converter o resultado da consulta para o dataframe do pandas, você pode simplesmente usar a função '.to_csv' com o nome do arquivo.

```python
output = conn.execute("SELECT * FROM matchs WHERE HomeTeam LIKE 'Norwich'")
results = output.fetchall()


data = pd.DataFrame(results)
data.columns = results[0].keys()
```


Evite adicionar uma coluna chamada “Index” usando `index=False`.

```python
data.to_csv("SQl_result.csv",index=False)
```

### Arquivo CSV para tabela SQL
Nesta parte, converteremos o arquivo CSV de dados da Bolsa de Valores em uma tabela SQL.

Primeiro, conecte-se ao banco de dados sqlite do datacamp.

```python
engine = create_engine("sqlite:///datacamp.sqlite")
```
Em seguida, importe o arquivo CSV usando a função read_csv. No final, use a função `to_sql` para salvar o dataframe do pandas como uma tabela SQL.

Principalmente, a função `to_sql` requer conexão e nome da tabela como argumento. Você também pode usar `if_exisits` para substituir uma tabela existente com o mesmo nome e `index` para eliminar a coluna do índice.

```python
df = pd.read_csv('Stock Exchange Data.csv')
df.to_sql(con=engine, name="Stock_price", if_exists='replace', index=False)
```


Para validar os resultados, precisamos conectar o banco de dados e criar um objeto tabela.

```python
conn = engine.connect()
metadata = db.MetaData()
stock = db.Table('Stock_price', metadata, autoload=True, autoload_with=engine)
```
 
Em seguida, execute a consulta e exiba os resultados.

```python
query = stock.select()
exe = conn.execute(query)
result = exe.fetchmany(5)
for r in result:
    print(r)
```
Como você pode ver, transferimos com sucesso todos os valores do arquivo CSV para a tabela SQL.


('HSI', '1986-12-31', 2568.300049, 2568.300049, 2568.300049, 2568.300049, 2568.300049, 0, 333.87900637)
('HSI', '1987-01-02', 2540.100098, 2540.100098, 2540.100098, 2540.100098, 2540.100098, 0, 330.21301274)
('HSI', '1987-01-05', 2552.399902, 2552.399902, 2552.399902, 2552.399902, 2552.399902, 0, 331.81198726)
('HSI', '1987-01-06', 2583.899902, 2583.899902, 2583.899902, 2583.899902, 2583.899902, 0, 335.90698726)
('HSI', '1987-01-07', 2607.100098, 2607.100098, 2607.100098, 2607.100098, 2607.100098, 0, 338.92301274)


### Gerenciamento de tabelas SQL
#### Atualizando os valores na tabela
Atualizar valores é simples. Usaremos as funções update, valores e where para atualizar o valor específico na tabela.
```python
table.update().values(column_1=1, column_2=4,...).where(table.columns.column_5 >= 5)
```

No nosso caso, alteramos o valor “Pass” de False para True onde o nome do aluno é “Nisha”.

```python
Student = db.Table('Student', metadata, autoload=True, autoload_with=engine)
query = Student.update().values(Pass = True).where(Student.columns.Name == "Nisha")
results = conn.execute(query)
```

Para validar os resultados, vamos executar uma consulta simples e exibir os resultados na forma de um dataframe do pandas.

```python
output = conn.execute(Student.select()).fetchall()
data = pd.DataFrame(output)
data.columns = output[0].keys()
data
```

Alteramos com sucesso o valor “Pass” para True para o nome do aluno “Nisha”.


### Excluir os registros
Excluir as linhas é semelhante à atualização. Requer a função delete e where.
```python
table.delete().where(table.columns.column_1 == 6)
```

No nosso caso, estamos excluindo o cadastro do aluno chamado “Ben”.

```python

Student = db.Table('Student', metadata, autoload=True, autoload_with=engine)
query = Student.delete().where(Student.columns.Name == "Ben")
results = conn.execute(query)
```
 
Para validar os resultados, executaremos uma consulta rápida e exibiremos os resultados na forma de um dataframe. Como você pode ver, excluímos a linha que contém o nome do aluno “Ben”.

```python
output = conn.execute(Student.select()).fetchall()
data = pd.DataFrame(output)
data.columns = output[0].keys()
data
```
### Deletando tabelas
Se você estiver usando SQLite, eliminar a tabela gerará um erro “banco de dados está bloqueado“. Por que? Porque SQLite é uma versão muito leve. Ele só pode executar uma função por vez. Atualmente, ele está executando uma consulta selecionada. Precisamos fechar toda a execução antes de deletar a tabela.

```python
results.close()
exe.close()
```

Depois disso, use a função drop_all dos metadados e selecione um objeto de tabela para eliminar a tabela única. Você também pode usar o comando `Student.drop(engine)` para eliminar uma única tabela.
```python
metadata.drop_all(engine, [Student], checkfirst=True)
```

Se você não especificar nenhuma tabela para a função drop_all. Isso eliminará todas as tabelas do banco de dados.

```python
metadata.drop_all(engine)
```
