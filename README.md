![capa](https://github.com/leoaguiar07/pyMongo/blob/main/mongo_python.fw.png)

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


SELECT divisions.division, divisions.name, divisions.country
FROM divisions

 OpenAI
This code is written in SQL.
• The code selects the columns "division", "name", and "country" from the table "divisions".
• The "FROM" keyword specifies the table from which the data is being selected.
• In summary, this code retrieves the specified columns from the "divisions" table.
Was this helpful? Yes
 No
SQL query result
We will now execute the query using the connection object and extract the first five rows. 

fetchone(): it will extract a single row at a time.
fetchmany(n): it will extract the n number of rows at a time.
fetchall(): it will extract all of the rows.  

exe = conn.execute(query) #executing the query
result = exe.fetchmany(5) #extracting top 5 results
print(result)

 OpenAI
The result shows the first five rows of the table. 


[('B1', 'Division 1A', 'Belgium'), ('D1', 'Bundesliga', 'Deutschland'), ('D2', '2. Bundesliga', 'Deutschland'), ('E0', 'Premier League', 'England'), ('E1', 'EFL Championship', 'England')]

 OpenAI
Python SQLAlchemy Examples
In this section, we will look at various SQLAlchemy examples for creating tables, inserting values, running SQL queries, data analysis, and table management. 

You can follow along or check out DataCamp’s Workspace. It contains a database, source code, and results. 

Creating Tables
First, we will create a new database called “datacamp.sqlite”. The create_engine will create a new database automatically if there is no database with the same name. So, creating and connecting are pretty much similar.

After that, we will connect the database and create a metadata object. 

We will use SQLAlchmy’s Table function to create a table called “Student”

It consists of columns:

Id: Integer and primary key
Name: String and non-nullable 
Major: String and default = “Math”
Pass: Boolean and default =True 
We have created the structure of the table. Let’s add it to the database using `metadata.create_all(engine)`.


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

 OpenAI
Insert one
To add a single row, we will first use `insert` and add the table object. After that, use `values` and add values to the columns manually. It works similarly to adding arguments to Python functions.   

Finally, we will execute the query using the connection to execute the function.


query = db.insert(Student).values(Id=1, Name='Matthew', Major="English", Pass=True)
Result = conn.execute(query)

 OpenAI
Let’s check if we add the row to the “Student” table by executing a select query and fetching all the rows. 


output = conn.execute(Student.select()).fetchall()
print(output)

 OpenAI
We have successfully added the values. 


[(1, 'Matthew', 'English', True)]

 OpenAI
Insert many
Adding values one by one is not a practical way of populating the database. Let’s add multiple values using lists. 

Create an insert query for the Student table.
Create a list of multiple rows with column names and values.
Execute the query with a second argument as values_list. 

query = db.insert(Student)
values_list = [{'Id':'2', 'Name':'Nisha', 'Major':"Science", 'Pass':False},
              {'Id':'3', 'Name':'Natasha', 'Major':"Math", 'Pass':True},
              {'Id':'4', 'Name':'Ben', 'Major':"English", 'Pass':False}]
Result = conn.execute(query,values_list)

 OpenAI
To validate our results, run the simple select query.


output = conn.execute(db.select([Student])).fetchall()
print(output)

 OpenAI
The table now contains more rows. 


[(1, 'Matthew', 'English', True), (2, 'Nisha', 'Science', False), (3, 'Natasha', 'Math', True), (4, 'Ben', 'English', False)]

 OpenAI
Simple SQL Query with SQLAlchemy
Instead of using Python objects, we can also execute SQL queries using String. 

Just add the argument as a String to the `execute` function and view the result using `fetchall`.


output = conn.execute("SELECT * FROM Student")
print(output.fetchall())

 OpenAI
Output:


[(1, 'Matthew', 'English', 1), (2, 'Nisha', 'Science', 0), (3, 'Natasha', 'Math', 1), (4, 'Ben', 'English', 0)]

 OpenAI
You can even pass more complex SQL queries. In our case, we are selecting the Name and Major columns where the students have passed the exam. 


output = conn.execute("SELECT Name, Major FROM Student WHERE Pass = True")
print(output.fetchall())

 OpenAI
Output:


[('Matthew', 'English'), ('Natasha', 'Math')]

 OpenAI
Using SQLAlchemy API
In the previous sections, we have been using simple SQLAlchemy API/Objects. Let’s dive into more complex and multi-step queries.

In the example below, we will select all columns where the student's major is English.  


query = Student.select().where(Student.columns.Major == 'English')
output = conn.execute(query)
print(output.fetchall())

 OpenAI
Output:


[(1, 'Matthew', 'English', True), (4, 'Ben', 'English', False)]

 OpenAI
Let’s apply AND logic to the WHERE query. 

In our case, we are looking for students who have an English major, and they have failed.  

Note: not equal to ‘!=’ True is False. 


query = Student.select().where(db.and_(Student.columns.Major == 'English', Student.columns.Pass != True))
output = conn.execute(query)
print(output.fetchall())

 OpenAI
Only Ben has failed the exam with an English major. 


[(4, 'Ben', 'English', False)]

 OpenAI
Using a similar table, we can run all kinds of commands, as shown in the table below. 

You can copy and paste these commands to test the results on your own. Check out the DataCamp’s Workspace if you get stuck in any of the given commands. 

Commands

API

in

Student.select().where(Student.columns.Major.in_(['English','Math']))

and, or, not

Student.select().where(db.or_(Student.columns.Major == 'English', Student.columns.Pass = True))

order by

Student.select().order_by(db.desc(Student.columns.Name))

limit

Student.select().limit(3)

sum, avg, count, min, max

db.select([db.func.sum(Student.columns.Id)])

group by

db.select([db.func.sum(Student.columns.Id),Student.columns.Major]).group_by(Student.columns.Pass)

distinct

db.select([Student.columns.Major.distinct()])

To learn about other functions and commands, check out SQL Statements and Expressions API official documentation.

Output to Pandas DataFrame
Data scientists and analysts appreciate pandas dataframes and would love to work with them. In this part, we will learn how to convert an SQLAlchemy query result into a pandas dataframe. 

First, execute the query and save the results. 


query = Student.select().where(Student.columns.Major.in_(['English','Math']))
output = conn.execute(query)
results = output.fetchall()

 OpenAI
Then, use the DataFrame function and provide the SQL results as an argument. Finally, add the column names using the result first-row `results[0]` and `.keys()`

Note: you can provide any valid row to extract the names of the columns using `keys()`


data = pd.DataFrame(results)
data.columns = results[0].keys()
data

 OpenAI
Output to Pandas DataFrame
Data Analytics With SQLAlchemy
In this part, we will connect the European football database and perform complex queries and visualize the results.  

Connecting two tables
As usual, we will connect the database using the `create_engine` and `connect` functions.

In our case, we will be joining two tables, so we have to create two table objects: division and match.  


engine = create_engine("sqlite:///european_database.sqlite")
conn = engine.connect()
metadata = db.MetaData()
division = db.Table('divisions', metadata, autoload=True, autoload_with=engine)
match = db.Table('matchs', metadata, autoload=True, autoload_with=engine)

 OpenAI
Running complex query
We will select both division and match columns.
Join them using a common column: division.division and match.Div.
Select all columns where the division is E1 and the season is 2009.
Order the result by HomeTeam.
You can even create more complex queries by adding additional modules.

Note: to auto-join two table you can also use:  `db.select([division.columns.division,match.columns.Div])`


query = db.select([division,match]).\
select_from(division.join(match,division.columns.division == match.columns.Div)).\
where(db.and_(division.columns.division == "E1", match.columns.season == 2009 )).\
order_by(match.columns.HomeTeam)
output = conn.execute(query)
results = output.fetchall()

data = pd.DataFrame(results)
data.columns = results[0].keys()
data

 OpenAI
Data Analytics With SQLAlchemy

After executing the query, we converted the result into a pandas dataframe. 

Both tables are joined, and the results only show the E1 division for the 2009 season ordered by the HomeTeam column. 

Data Visualization
Now that we have a dataframe, we can visualize the results in the form of a bar chart using Seaborn. 

We will:

Set the theme to “whitegrid”
Resize the visualization size to 15X6
Rotate x-axis ticks to 90
Set color palates to “pastels”
Plot a bar chart of "HomeTeam" v.s "FTHG" with the color Blue.
Plot a bar chart of "HomeTeam" v.s "FTAG" with the color Red.
Display the legend on the upper left.
Remove the x and y labels. 
Despine left and bottom.
The main purpose of this part is to show you how you can use the output of the SQL query and create amazing data visualization. 


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

 OpenAI
Data Visualization with SQLAlchemy
Saving Results to CSV
After converting the query result to pandas dataframe, you can simply use the '.to_csv' function with the file name. 


output = conn.execute("SELECT * FROM matchs WHERE HomeTeam LIKE 'Norwich'")
results = output.fetchall()


data = pd.DataFrame(results)
data.columns = results[0].keys()

 OpenAI
Avoid adding a column called “Index” by using `index=False`.


data.to_csv("SQl_result.csv",index=False)

 OpenAI
CSV file to SQL Table
In this part, we will convert the Stock Exchange Data CSV file to an SQL table. 

First, connect to the datacamp sqlite database.


engine = create_engine("sqlite:///datacamp.sqlite")

 OpenAI
Then, import the CSV file using the read_csv function. In the end, use the `to_sql` function to save the pandas dataframe as an SQL table.  

Primarily, the `to_sql` function requires connection and table name as an argument. You can also use `if_exisits` to replace an existing table with the same name and `index` to drop the index column. 


df = pd.read_csv('Stock Exchange Data.csv')
df.to_sql(con=engine, name="Stock_price", if_exists='replace', index=False)
>>> 2222

 OpenAI
To validate the results, we need to connect the database and create a table object. 


conn = engine.connect()
metadata = db.MetaData()
stock = db.Table('Stock_price', metadata, autoload=True, autoload_with=engine)

 OpenAI
Then, execute the query and display the results.


query = stock.select()
exe = conn.execute(query)
result = exe.fetchmany(5)
for r in result:
    print(r)

 OpenAI
As you can see, we have successfully transferred all the values from the CSV file to the SQL table. 


('HSI', '1986-12-31', 2568.300049, 2568.300049, 2568.300049, 2568.300049, 2568.300049, 0, 333.87900637)
('HSI', '1987-01-02', 2540.100098, 2540.100098, 2540.100098, 2540.100098, 2540.100098, 0, 330.21301274)
('HSI', '1987-01-05', 2552.399902, 2552.399902, 2552.399902, 2552.399902, 2552.399902, 0, 331.81198726)
('HSI', '1987-01-06', 2583.899902, 2583.899902, 2583.899902, 2583.899902, 2583.899902, 0, 335.90698726)
('HSI', '1987-01-07', 2607.100098, 2607.100098, 2607.100098, 2607.100098, 2607.100098, 0, 338.92301274)

 OpenAI
SQL Table Management
Updating the values in table
Updating values is straightforward. We will use the update, values, and where functions to update the specific value in the table. 


table.update().values(column_1=1, column_2=4,...).where(table.columns.column_5 >= 5)

 OpenAI
In our case, we have changed the “Pass” value from False to True where the name of the student is “Nisha”.  


Student = db.Table('Student', metadata, autoload=True, autoload_with=engine)
query = Student.update().values(Pass = True).where(Student.columns.Name == "Nisha")
results = conn.execute(query)

 OpenAI
To validate the results, let’s execute a simple query and display the results in the form of a pandas dataframe. 


output = conn.execute(Student.select()).fetchall()
data = pd.DataFrame(output)
data.columns = output[0].keys()
data

 OpenAI
We have successfully changed the “Pass” value to True for the student name “Nisha”.

Update values in SQL
Delete the records
Deleting the rows is similar to updating. It requires delete and where function. 


table.delete().where(table.columns.column_1 == 6)

 OpenAI
In our case, we are deleting the record of the student named “Ben”.


Student = db.Table('Student', metadata, autoload=True, autoload_with=engine)
query = Student.delete().where(Student.columns.Name == "Ben")
results = conn.execute(query)

 OpenAI
To validate the results, we will run a quick query and display the results in the form of a dataframe. As you can see, we have deleted the row containing the student name ”Ben”.


output = conn.execute(Student.select()).fetchall()
data = pd.DataFrame(output)
data.columns = output[0].keys()
data

 OpenAI
Delete Values
Dropping tables
If you are using SQLite, dropping the table will throw an error “database is locked“. Why? Because SQLite is a very light version. It can only perform one function at a time. Currently, it is executing a select query. We need to close all of the execution before deleting the table. 


results.close()
exe.close()

 OpenAI
After that, use metadata’s drop_all function and select a table object to drop the single table. You can also use the `Student.drop(engine)` command to drop a single table.


metadata.drop_all(engine, [Student], checkfirst=True)

 OpenAI
If you don’t specify any table for the drop_all function. It will drop all of the tables in the database. 


metadata.drop_all(engine)

 OpenAI
Conclusion 
The SQLAlchemy tutorial covers various functions of SQLAlchemy, from connecting the database to modifying tables, and if you are interested in learning more, try completing the Introduction to Databases in Python interactive course. You will learn about the basics of relational databases, filtering, ordering, and grouping. Furthermore, you will learn about advanced SQLAlchemy functions for data manipulation.  

If you are finding any issues in following the tutorial, you can run the source code using workspace and compare your code with it. You can even duplicate the Jupyter notebook and run the code by clicking just two buttons.  
