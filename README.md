# Data Science Independent Project #2 – Explore a Sample Database

## Descrição
Neste projeto, uu sou o principal analista de dados de uma loja de música popular. Estou ajudando a analisar as vendas e serviços da loja. Para isso eu baixei o banco de dados de amostra SQLite fornecido pelo site.

Utilizei o SQLite e o DB Browser for SQLite para manupilação e consulta do banco de dados.

E agora irei responder as perguntas:

<br>

**- Quais faixas apareceram na maioria das playlists? em quantas playlists eles apareceram?**

```sql
select pt.TrackId, t.Name, count(PlaylistId) as Total 
from playlist_track pt
inner join tracks t
on pt.TrackId = t.TrackId
group by pt.TrackId, t.name
order by total desc
limit 10;
```

| TrackId | Name                                                      | Total |
|---------|-----------------------------------------------------------|-------|
| 3403    | Intoitus: Adorate Deum                                    | 5     |
| 3404    | Miserere mei, Deus                                        | 5     |
| 3408    | Aria Mit 30 Veränderungen, BWV 988 "Goldberg Variations": Aria | 5     |
| 3409    | Suite for Solo Cello No. 1 in G Major, BWV 1007: I. Prélude | 5     |
| 3410    | The Messiah: Behold, I Tell You a Mystery... The Trumpet Shall Sound | 5     |
| 3411    | Solomon HWV 67: The Arrival of the Queen of Sheba         | 5     |
| 3415    | Symphony No.5 in C Minor: I. Allegro con brio             | 5     |
| 3416    | Ave Maria                                                 | 5     |
| 3417    | Nabucco: Chorus, "Va, Pensiero, Sull'ali Dorate"          | 5     |
| 3418    | Die Walküre: The Ride of the Valkyries                    | 5     |

R: Limitei a query as 10 faixas que mais aparecem. Todas elas aparecem em no minimo 5 playlists diferentes. 

<br>
<br>

**- Qual faixa gerou mais receita? qual álbum? qual gênero?**

Este código retorna as músicas com mais vendas.

```sql
select iv.TrackId, t.Name, count(Quantity) as Total
from invoice_items iv
inner join tracks t
on iv.TrackId =  t.TrackId
group by iv.TrackId, t.name
order by total desc;
```

| TrackId | Name             | Total |
|---------|------------------|-------|
| 2       | Balls to the Wall| 2     |
| 8       | Inject The Venom | 2     |
| 9       | Snowballed       | 2     |
| 20      | Overdose         | 2     |
| 32      | Deuces Are Wild  | 2     |

<br>

Já este, os albums com musicas mais vendidas.

```sql
select a.AlbumId, a.Title, count(Quantity) as Total
from invoice_items iv
left join tracks t
on iv.TrackId = t.TrackId
left join albums a
on a.AlbumId = t.AlbumId
group by a.AlbumId, a.Title
order by total desc
limit 5;
```

| AlbumId | Title          | Total |
|---------|----------------|-------|
| 23      | Minha Historia | 27    |
| 141     | Greatest Hits  | 26    |
| 73      | Unplugged      | 25    |
| 224     | Acústico       | 22    |
| 37      | Greatest Kiss  | 20    |
<br>

Por fim, este retorna os 5 generos musicas com mais vendas.

```sql
select g.GenreId, g.Name, count(Quantity) as Total
from invoice_items iv
left join tracks t
on iv.TrackId =  t.TrackId
left join genres g
on g.GenreId = t.GenreId
group by g.GenreId, g.name
order by total desc
limit 5;
```

| GenreId | Name               | Total |
|---------|--------------------|-------|
| 1       | Rock               | 835   |
| 7       | Latin              | 386   |
| 3       | Metal              | 264   |
| 4       | Alternative & Punk | 244   |
| 2       | Jazz               | 80    |

<br>
<br>

**- Quais países têm a maior receita de vendas? Que porcentagem da receita total cada país representa?**

```sql
select i.BillingCountry, count(Quantity) as Total, round((count(Quantity) * 100.0) / (select sum(Quantity) from invoice_items), 2) as Percentage
from invoices i 
left join invoice_items iv
on i.InvoiceId = iv.InvoiceId
group by i.BillingCountry
order by total desc;
```

Segue o código usado e os 10 países com a maior porcentagem:

| Country         | Sales | Percentage |
|-----------------|-------|------------|
| USA             | 494   | 22.05      |
| Canada          | 304   | 13.57      |
| France          | 190   | 8.48       |
| Brazil          | 190   | 8.48       |
| Germany         | 152   | 6.79       |
| United Kingdom  | 114   | 5.09       |
| Portugal        | 76    | 3.39       |
| Czech Republic  | 76    | 3.39       |
| India           | 74    | 3.3        |
| Sweden          | 38    | 1.7        |

<br>
<br>

**- Quantos clientes cada funcionário atendeu, qual é a receita média de cada venda e qual é a venda total?**

```sql
select e.EmployeeId, e.FirstName, e.LastName, count(i.CustomerId) as ClientQuantity, round(avg(i.Total), 2) as AverageSale, round(sum(i.Total), 2) as TotalSale 
from employees e
inner join invoices i
on i.CustomerId = c.CustomerId
inner join customers c
on e.EmployeeId = c.SupportRepId
group by e.EmployeeId
order by client_qtd desc;
```

Aqui está as informações de acordo com o maior para o menor numero de clientes.

| EmployeeId | FirstName | LastName | ClientQuantity | AverageSale | TotalSale |
|------------|-----------|----------|----------------|-------------|-----------|
| 3          | Jane      | Peacock  | 146            | 5.71        | 833.04    |
| 4          | Margaret  | Park     | 140            | 5.54        | 775.4     |
| 5          | Steve     | Johnson  | 126            | 5.72        | 720.16    |



## Considerações finais
Foi um projeto que me fez pensar bastante em como eu abordava cada pergunta e me fez pesquisar bastante, além de reforçar vários assuntos que havia estudado em SQL.

Segue o link do projeto (em inglês) e do curso que estou fazendo, caso queiram dar uma olhada:

https://discuss.codecademy.com/t/data-science-independent-project-2-explore-a-sample-database/419945

https://www.codecademy.com/career-journey/data-scientist-aly