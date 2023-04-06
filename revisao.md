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

> É possível atribuir um nome ao check, por exemplo:
> dataInicio date not null; dataFim date not null<br>
> Constraint periodoValido
> check (dtInicio < dtFim)

> age integer check (age >= 18)
> Nesse caso, o sgbd define o nome para a constraint

## Funções

## Gatilhos

