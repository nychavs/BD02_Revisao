# revisao 
- tipos de relacionamento
- os basicos crud
- store procedure
- autocommit, savepoint, rollback e commit
- hot backup e col backup
- backup incremental e full
- restore
- dado x metadado
- view normal e materialized
  
## Tipos de Relacionamento

Relacionamentos 1:1
> Relacionamentos (0,1) para (0,1) -> coluna a mais
> - vantagem minimiza ter que criar tabela
> - desvantagem ter coluna vazia<br>

> Relacionamentos (0,1) para (0,1) -> tabela própria para evitar ter coluna nula
> - vantagem não vai ter coluna em vão
> - desvantagem ter que fazer join<br>

> Relacionamentos (0,1) para (1,1) -> juntar tabelas<br>
> Relacionamentos (0,1) para (1,1) -> adicionar coluna em quem é opcional

> Relacionamentos (1,1) para (1,1) -> juntar tabelas

Relacionamentos 1:n
> Relacionamentos (0,1) para (0,n) -> coluna a mais em quem tem n<br>
> Relacionamentos (0,1) para (0,n) -> cria tabela
> - vantagem campos ser not null em alguns casos e em outros não
> - desvantagem fazer join

> Relacionamentos (0,1) para (1,n) -> coluna a mais em quem tem n

> Relacionamentos (1,1) para (0,n) -> coluna a mais em quem tem n

> Relacionamentos (1,1) para (1,n) -> coluna a mais em quem tem n

Relacionamentos n:n
> Sempre optar por criar tabela própria <br/>
> Não é possível adicionar colunas pois são monovaloradas <br/>
> Nova tabela herda chaves primárias

## Normalização
Primeira forma normal:
> Possui chave primária
> Não possui multivalorado (telefone)
> Não possui dados compostos (endereco)
> Não possui colunas com tipos de dados repetidos

Segunda forma normal:
> Está na 1FN
> Não possui dependencia parcial

Terceira forma normal:
> Está na 3FN
> Criar nova tabela para cada atributo não chave x e seu respectivo atributo nao chave do qual depende y
> Se x depende de y, y passa a ser PK em outra tabela e x é removido da original enquanto y é mantido

## Alteração na Tabela
o Alter table é usado para fazer alterações na estrutura da tabela, não ppodemos confudir com o update!! (apenas para alterações nas linhas ja existentes). Com o alter table, podemos fazer várias ações como adicionar/remover colunas, mudar o tipo da coluna, criar um valor default para uma coluna, criar chave secundária etc...
> Alter Table *nomeTabela*<br>
> ação...

## Funções de Agregação

## Restrições
As restrições mais utilizadas são: not null, unique, primary key, foreign key e check;
> Check: registro só pode ser inserido se respeitar uma condição (como um if). Exemplo: <br>

> É possível atribuir um nome ao check, por exemplo:<br>
> dataInicio date not null; dataFim date not null<br>
> constraint periodoValido<br>
> check (dtInicio < dtFim)<br>

> age integer check (age >= 18)<br>
> Nesse caso, o sgbd define o nome para a constraint

## Funções
Usadas para gerar um valor que pode ser usado em uma expressão, que pode ser baseado em parametros ou não. Sendo executada como parte de uma expressão. Ex:
> Select atributos, nomeFuncao<br>
> from tabela<br>
Estrutura da função <br>
> create or replace function *func* (*param TIPO,...*) <br>
> returns *tipo* as $$ <br>
> begin<br>
> *Lógica de programação…*<br>
> end;<br>
> $$ language plpgsql;<br>
Lembre-se: Ao modificar uma tabela ja criada e manter o tipo, pode ainda usar-se o or replace. No entanto, se modificar oo tipo de retorno, uma nova tabela deverá ser criada.

Declarar variaveis:
> DECLARE... *tipo*;<br>
> DECLARE... *tipo* := *valor*

Printar:
> Raise notice 'aaa: %', *variavel*
Condições:
> IF... THEN... END IF<br>
> IF... THEN... ELSE... END IF

>WHILE () LOOP... END LOOP;

>FOR... IN.. LOOP.. END LOOP;

Não necessariamente as funções possuem print, elas podem apenas retornar algo,  nesse caso na logica de programacao ficaria:
> if (*condicao*) then return...<br>
> if (time = 'palmeiras') then return time||':sem mundial'<br>
> o operador || serve para indicar concatenação

## Gatilhos
Quando uma ação for realizada a trigger (o gatilho) é disparado realizando suas funções
Podem ser usadas por exemplo para guardar as infos de modificações feitas no banco, como quando foi feito um insert e com o que. 
Sua estrutura é:
> create trigger *nome*<br>
> after/before *acao* on *nomeTabela*<br>
> for each row/statement (linha/comando transação)<br>
> execute procedure *nomeFunc()*<br>

exemplo de função:
> create function *nome* <br>
> returns trigger as $$ <br>
> begin <br>
> insert into *coluna*(*nome dos campos)*<br>
> values(old.*campo*,new.*campo*, *valor*)<br>
> return old/new<br>
> end $$ language plpgsql<br>
> ***old é usado para manter a linha antiga quando usa-se update ou delete;<br>
> ***new é usado quando contem uma nova linha para armazenar, quando usa-se insert ou update

## hot backup
conhecido como backup online/dinâmico, o backup dos dados é feito enquanto as pessoas operam e/ou utilizam o banco
> Desvantagens:
> - Consome maior investimento em sua infraestrutura pois precisa ser mais rápido do que o cold storage

> Vantagens:
> - evitar gastar tempo com o banco inativo para realizar o backup
> - util quando o banco precisa ficar 24 horas online

## cold backup
conhecido como backup offline/estático <br></br>
backup quando a aplicação/database não ta rodando e não pode ser acessada por ninguém

> Desvantagens:
> - ninguém pode mexer
<br></br>
> Vantagens:
> - melhora a segurança dos dados
> - consistência dos dados (ja que ta desligado, nao tem nenhuma operação acontecendo, evitando dados corrompidos ou então não gravados durante o backup)

## backup incremental
de período em período, exemplo: a cada cinco minutos, a cada vinte e quatro horas... Depois do backup incremental o backup full é feito para guardar todos esses conjuntos e depois de um tempo o incremental é apagado(ex: dois dias)<br></br>
nao faz sentindo backup cold ser incremental, normalmente é hot

## backup full
faz sentido o backup ser cold 
Um backup completo é quando uma cópia completa de todos os arquivos e pastas é feita. De todos os métodos, esse é o backup mais demorado e pode sobrecarregar a rede se o backup estiver ocorrendo na rede.

## restore 
depois que faz o backup, a gente resgata ele pra ver se ta funcionando

## metadado
estrutura do dado, exemplo -> quando estamos dando select em uma tabela, a tabela é um metadado. Estrutura de sistemas (tabelas internas)
> a tabela USER_TABLES é uma meta-tabela que possui informações a respeito das tabelas criadas pelos usuários. 

## views
### comandos:
- materialized:
> - CREATE MATERIALIZED VIEW mymatview AS SELECT * FROM mytab;
> - REFRESH MATERIALIZED VIEW mymatview;
- normal:
> - CREATE TABLE mymatview AS SELECT * FROM mytab;

a diferença é que a view materialized vai criar uma copia do select que usamos e deixa pronto pra quando precisar, e ai é feito o refresh, são indicados para dados que são apenas consultados.<br></br>
Já a view normal realiza uma consulta (query) em tempo de execução

### materialized:
CREATE MATERIALIZED VIEW mv_cliente_pedidos AS
SELECT c.nome, SUM(p.valor) AS total_pedidos
FROM clientes c
JOIN pedidos p ON c.id_cliente = p.id_cliente
GROUP BY c.nome;

### normal: 
CREATE VIEW view_cliente_pedidos AS
SELECT c.nome, SUM(p.valor) AS total_pedidos
FROM clientes c
JOIN pedidos p ON c.id_cliente = p.id_cliente
GROUP BY c.nome;


## transactions
são alterações feitas no banco, tendo com principais transactions: commit, rollback e savepoint

# links uteis
https://storware.eu/blog/pros-and-cons-of-cold-backup-and-hot-backup-comparison/
