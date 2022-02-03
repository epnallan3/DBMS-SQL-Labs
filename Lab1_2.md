# Lab 1.2

## Lecture 4
```sql
drop table if exists product; 
drop table if exists company;

create table company (
    cname varchar(20) primary key, -- company name uniquely identifies the company.
    stockprice decimal(10,2), -- stock price is in money 
    country varchar(10)); -- country is just a string
insert into company values ('ToyWorks', 25.0, 'USA');
insert into company values ('ToyFriends', 65.0, 'China');
insert into company values ('ToyCo', 15.0, 'China');

create table product(
       pname varchar(10), -- name of the product
       price decimal(10,2), -- price of the product
       category varchar(10), -- category
       manufacturer varchar(10), -- manufacturer
       primary key (pname, manufacturer),
       foreign key (manufacturer) references company(cname));
insert into product values('Pikachu', 19.99, 'Toy', 'ToyWorks');
insert into product values('Pikachu', 19.99, 'Toy', 'ToyFriends');
insert into product values('Pokeball', 29.99, 'Electronic', 'ToyCo');
insert into product values('Bulbasaur', 149.99, 'Toy', 'ToyFriends');
insert into product values('Charizard', 203.99, 'Toy', 'ToyCo');
insert into product values('PokeCamera', 19.99, 'Electronic', 'ToyWorks');
```

Our goal is to answer the following question:
Find all categories of products that are made by Chinese companies
