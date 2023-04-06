# postgreSQL

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
