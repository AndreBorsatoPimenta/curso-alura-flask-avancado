# Curso flask avançado

## Modulo 1(Percistencia com SQL)

### Gerando banco de dados
* Como os dados adicionados na tabela apenas ficam salvos no site adicionaremos um banco de dados para poder nao perder as mudanças feitas na lista de "jogoteca"
* Em um arquivo python especifico para criação do banco de dados que irei dar o nome de "prepara_banco.py" iremos colocar a ligação com o banco de dados para a criaçao de uma database e tabelas para os nomes de "usuarios" e tambem "jogos" assim criando uma data base nova não em um ambiente virtual e sim no proprio computador

### Conectando ao banco de dados
* Agora com o nosso banco de dados criados nos precisaremos de conectar com a aplicação ultilizando um ORM(Object-Relational Mapping) ultilizando o ORM SQLAlchemy 
  * Assim intalando o SQLAlchemy com o comando:
```python
pip install flask -sqlalchemy
```
  * Apos isso importaremos o SQLAlchemy para o codigo com:
```python
from flask_sqlalchemy import SQLAlchemy
```
  * E instanciando o objeto de banco de dados colocando "db = SQLAlchemy" e dentro o argumento da aplicação flask ficando deste jeito
```python
db = SQLAlchemy(app)
```
* Agora que o SQLAlchemy esta conectado com nossa aplicação deveremos conectalo tambem ao banco de dados que vamos fazer com uma nova variavel sendo a parte entre colchetes o nome dessa variavel
```python
app.config['SQLAlchemy_DATABASE_uri'] = /
"SGBD://usuario:senha@servidor/database"
#ultilizando a "/" poderemos dar um "enter" para ficar esteticamente melhor no python
```
* Assim finalmente conectando a aplicaçao ao banco de dados

### Criando tabelas

* Agora deveremos criar duas classes na aplicação para poder usarmos de ponte para as tabelas do banco de dados
  * classe de Jogos
    ```python
        Class Jogos(db.Model):
        id = db.column(db.Integer, primary_key=True, autoincrement=True)
        nome = db.column(db.String(50), nullable=False)
        categoria = db.column(db.String(40), nullable=False)
        console = db.column(db.String(20), nullable=False)

    def __repr__(self):
        return '<Name %r>' % self.name
    ```
  * Classe de Usuarios
      ```python
        Class Usuarios(db.Model):
        nome = db.column(db.String(20), nullable=False)
        nickname = db.column(db.String(8), primary_key=True)
        senha = db.column(db.String(100), nullable=False)

    def __repr__(self):
        return '<Name %r>' % self.name
    ```
* Agora com acesso ao banco de dados poderemos remover as classes antigas de instaciação de jogos e usuarios que ultilizavamos anteriormente mas ocorrendo um erro por o "Index.html" busca pela lista de nomes que corrigiremos a seguir

### recuperando a listagem persistida

* para corrigir o erro ocorrido na parte anterior poderemos fazer uma simples mudança para corrigir aqueles problemas
* colocando nas funcões de rota dessa certa maneira:
  * Rota Index:
@app.route('/')
def index():
lista = Jogos.query.order_by(Jogos.id)
return render_templates('lista.html', titulo='Jogos', jogos='lista')
  * Rota Autenticar:
@app.route('/autenticar', methods=['POST',])
def autenticar():
usuario = Usuarios.query.filter_by(nickname=request.form['usuario']).first()
if usuario:
<--Mantem o codigo do autenticar apartir do segundo "if"-->
  * Rota Criar:
@app.route('/criar', methods=['POST',])
def criar():
 nome = request.form[nome]
categoria = request.form[categoria]
console = request.form[console]
jogo = Jogos.query.filter_by(nome=nome).first()
if jogo:
    flash('Jogo ja existente!')
novo_jogo = Jogos(nome=nome, categoria=categoria, console=console)
db.session.add(novo_jogo)
db.session.commit()
