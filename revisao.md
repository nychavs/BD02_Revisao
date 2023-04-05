# postgreSQL

## Tipos de Relacionamento

Relacionamentos 1:1
> Relacionamentos (0,1) para (0,1) -> coluna a mais
> - vantagem minimiza ter que criar tabela
> - desvantagem ter coluna vazia
> Relacionamentos (0,1) para (0,1) -> tabela própria para evitar ter coluna nula
> - vantagem não vai ter coluna em vão
> - desvantagem ter que fazer join<br>
> *ex:* casamento, se a pessoa for casada e ela for obrigada a informar isso então pode só adicionar coluna. No entanto, se a pessoa não for casada e ela TIVER que informar isso E a coluna for not null então ferrou. Para evitar isso, quando o campo NÃO PODE ser NULO, é melhor criar uma tabela própria. 

> Relacionamentos (0,1) para (1,1) -> juntar tabelas<br>
> Relacionamentos (0,1) para (1,1) -> adicionar coluna em quem é opcional

> Relacionamentos (1,1) para (1,1) -> juntar tabelas

Relacionamentos 1:n
> Relacionamentos (0,1) para (0,n) -> coluna a mais em quem tem n
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
## Alteração na Tabela
## Funções de Agregação
## Restrições
## Funções
## Gatilhos

