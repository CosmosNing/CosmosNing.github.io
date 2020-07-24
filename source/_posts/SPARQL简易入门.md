---
title: SPARQL简易入门
toc: true
mathjax: true
date: 2020-07-22 21:27:40
description: 本文将简要介绍 SPARQL 的基本语法
permalink: sparql-grammar-tutorial/
tags:
- SPARQL
- 图数据库查询语言
categories:
- 研究
---

> 最近在学习图数据库的基础知识。本文将参考 《[Learning SPARQL: querying and updating with SPARQL 1.1](http://learningsparql.com/)》这本书，简单介绍 RDF 数据查询语言 SPARQL。主要内容如下：
>
> 1. SPARQL简介
> 2. SPARQL 基本语法
> 3. SPARQL 常见关键字
> 4. SPARQL 常见函数

# SPARQL 简介

SPARQL（SPARQL Protocol and RDF Query Language）是一种数据查询语言。它不仅仅支持查询 RDF 数据，也可以在部分关系型数据库中对数据库进行数据操作。

# 基本语法

SPARQL 的语法与 SQL 很类似，也有许多不同之处。下面将简要介绍 SPARQL 的基本语法。

## 符号

### 变量和常量（字符串、数字及URI）

在 SPARQL 语句中，通常以 `？变量名` 表示变量；而常量一般为字符串、数字及 URI，其中 URI 由尖括号（< >）包裹。

### 标点符号

* 逗号（,）与分号（;）

与 RDF Turtle 序列化格式类似，`;` 代表下一个三元组与当前三元组拥有**相同的主语**，`,` 代表下一个三元组与当前三元组拥有**相同的主语和谓语**。

* 井号（#）

在 SPARQL 脚本文件中（文件后缀名为 `.rq`），**`#` 为注释标记**。

* 尖括号（< >）

尖括号常用来**包裹 URI**。

* 星号（*）与加号（+）

星号和加号常用于**查询的正则匹配**。在正则匹配中，**星号**代表**零个或多个**，**加号**代表**一个或多个**。例如，假设有一个论文相互引用的关系图，那么 `?s c:cites :paperA` 中的 `？s` 则代表所有一次直接引用 `:paperA` 的论文；而 `?s c:cites+ :paperA` 中的 `?s`则在前者的基础之上，进一步包含了间接引用 `:paperA` 的论文。

此外，与 SQL 相同，查询子句 `SELECT *` 中的星号代表所有数据。

* 叹号（!）

叹号用于布尔条件判断中的**否定词**。

* 脱字符（^）

脱字符在 SPARQL 中可用来修饰谓语，用来反转主语与宾语之间的关系。例如，假设有一个三元组 `s p o;` ，那么  `o ^p s;` 将与之等价（**主宾颠倒**）。 

*  斜杠（/）

斜杠用来分隔几个连续的谓语，例如，`?s c:cites/c:cites/c:cites :paperA` 中 `?s` 代表与 `:paperA` 引用属性距离为 3 的所有论文。

## 语句

### 查询

SPARQL 支持多种关键词查询数据。这些关键词包括 `SELECT` 、`CONSTRUCT` 、`DESCRIBE` 、 `ASK` 。

* `SELECT` 查询结果返回一个二维表（与 SQL 中 `SELECT` 类似），其语句一般格式如下：

```SPARQL
SELECT [DISTINCT] <variable1> [<variable2> ...]
[FROM ...]
WHERE
{
    triple pattern 1.
    [triple pattern 2.]
    ...
    [附加条件...]
}
[OFFSET 数字]
[LIMIT 数字]
[ORDER BY | GROUP BY ...]
```

* `CONSTRUCT` 查询结果返回一个 RDF 图，其语句一般格式如下：

```SPARQL
CONSTRUCT 
{ 
    triple pattern .
    ...
} 
WHERE 
{ 
    triple pattern . 
    ...
}
```

{% note info %}

关于 `CONSTRUCT` 更多用法请参看 [W3C Recommendation](https://www.w3.org/TR/sparql11-query/#construct)

{% endnote %}

* `ASK` 查询结果返回真或者假，表示 RDF 数据中是否存在指定模式的三元组，其语句一般格式如下：

```SPARQL
ASK  
{ 
    triple pattern .
}
```

* `DESCRIBE` 查询结果返回对指定数据的资源描述（以 RDF 图的形式存储），该图的结果由 SPARQL 处理器决定（也就是说，不同 SPARQL 处理器运行同一条 `DESCRIBE` 查询语句，可能会有不同的结果），其语句一般格式如下：

```SPARQL
DESCRIBE <variable1>|<IRI1> [<variable2>|<IRI2> ...]
WHERE 
{
    triple pattern .
}
```

{% note info %}

关于 `DESCRIBE` 更多用法请参看 [W3C Recommendation](https://www.w3.org/TR/sparql11-query/#describe)

{% endnote %}

{% note info %}

<p class="note-title">URLs, URN，URIs，IRIs</p>

* URL（Uniform Resource Locator）：用于指明网络资源的位置。
* URN（Universal Resource Name）：用于指明网络资源的名称。
* URI（Universal Resource Identifier）：用于唯一指定资源，包含 URL 和 URN（很少在用）。
* IRI（Internationalized Resource Identifier）：IRI 是 URI，但是可包含更广泛的字符（如中文等）

{% endnote %}

### 增加（TBD）

### 更新（TBD）

### 删除（TBD）

# 常见关键字

## WHERE

`WHERE` 用于指明查询条件，例如：

```SPARQL
# filename: ex003.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?craigEmail
WHERE
{ ab:craig ab:email ?craigEmail . }
```

## DISTINCT

和 SQL 一样，`DISTINCT` 用于消去查询结果中重复的数据项：

```SPARQL
# filename: ex092.rq

SELECT DISTINCT ?p 
WHERE
{ ?s ?p ?o . }
```

上述查询将会返回**数据集中所有不重复的属性名称**。当遇到一个新的数据集时，建议执行该脚本，从而初步认识该数据集。

## FROM

`FROM` 关键字常用于指明数据集所在位置，有以下两种形式：

* `FROM <URI>`

```SPARQL
# filename: ex123.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?email 
FROM <ex069.ttl>
FROM <ex122.ttl>
WHERE
{ ?s ab:email ?email . }
```

例如，上述 SPARQL 脚本文件从 `<ex069.ttl>` 和 `<ex122.ttl>` 文件中查询数据

* `FROM NAMED`

`FROM NAMED` 为提供了在 Named Graph 中查询数据的功能。下面是一个具体的例子：

```SPARQL
# filename: ex126.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#>

SELECT ?lname ?courseName 
FROM <ex069.ttl> 
FROM NAMED <ex125.ttl>
FROM NAMED <ex122.ttl>   # unnecessary

WHERE
{
  { ?student ab:lastName ?lname }
  UNION
  { GRAPH <ex125.ttl> { ?course ab:courseTitle ?courseName } }
}
```

该查询包含三个数据集，一个是通过 `FROM <URI>` 指定，另外两个以 `FORM NAMED` 指定。在 `WHERE` 查询条件中，`GRAPH <ex125.ttl>` 说明下面一条三元组模式将仅在 `<ex125.ttl>` 中查找；如不指定 `GRAPH <URI>` ，则从 `FROM <URI>` 中查找。

{% note info %}

<p class="note-title"><code>FROM &lt;URI&gt;</code> V.S. <code>FROM NAMED</code></p>

* 在使用 `FROM <URI>` 的方式指明数据集时，这些数据都会添加到一个默认的 RDF 图（default graph）中以供查询；

* 而 `FROM NAMED` 则不会将数据集加入到默认的 RDF 图中，需要在 `WHERE` 额外声明 `GRAPH <URI>` 才能仅在该 Named Graph 中查询数据。

{% endnote %}

## GRAPH

`GRAPH` 常与 `FROM NAMED` 配合使用，用于指明下一个模式只在 `GRAPH` 指定的数据集中查询（例子见本文 `FROM NAMED` 部分）。

## OPTIONAL

关键词 `OPTIONAL` 的含义是，**如果存在的话，请返回该值，否则返回为空**。例如：

```SPARQL
# filename: ex057.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?first ?last ?workTel
WHERE
{
  ?s ab:firstName ?first ;
     ab:lastName ?last .
  OPTIONAL 
  { ?s ab:workTel ?workTel . }
}
```

上述脚本查询了一个通讯录，无论其是否有工作电话这个属性（如果没有，将该值置空即可），都返回其名、姓和工作电话的值。

**一个查询脚本可有多个 `OPTIONAL` 子句，并且一个 `OPTIONAL` 子句可有多个匹配模式**。值得注意的是，查询处理将**先后**处理 `OPTIONAL` 子句中的条件。利用这个特性，我们可以完成下列查询：优先返回每个人的昵称，如果没有则返回其名。查询脚本示例如下：

```SPARQL
# filename: ex063.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?first ?last
WHERE
{
  ?s ab:lastName ?last . 
  OPTIONAL { ?s ab:nick ?first . }
  OPTIONAL { ?s ab:firstName ?first . }
}
```

## FILTER 、 FILTER NOT EXISTS 及 MINUS

* `FILTER`

`FILTER` 意为筛选，顾名思义，其功能就是筛选出满足某种条件的数据。`FILTER` 接受一个结果为布尔量的输入：如果该布尔量为真，则选出；否则，不选。

```SPARQL
# filename: ex105.rq

PREFIX dm: <http://learningsparql.com/ns/demo#> 

SELECT ?s ?cost
WHERE
{
  ?s dm:cost ?cost .
  FILTER (?cost < 10)
}
```

上述查询就筛选出花费小于 10 的项目和具体花费。

* `FILTER NOT EXISTS`

`FILTER NOT EXISTS` 是 `FILTER` 的一种否定表达。例如：

```SPARQL
# filename: ex067.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?first ?last 

WHERE
{
  ?s ab:firstName ?first ;
     ab:lastName ?last .
  FILTER NOT EXISTS { ?s ab:workTel ?workNum }
}
```

上述查询就筛选出没有工作电话的人的名和姓。

* `MINUS`

`MINUS` 表示在查询结果中减去满足某种模式的数据。那么，筛选出没有工作电话的人的名和姓也可以通过 `MINUS`  实现：

```SPARQL
# filename: ex068.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#> 

SELECT ?first ?last 

WHERE
{
  ?s ab:firstName ?first ;
     ab:lastName ?last .
  MINUS { ?s ab:workTel ?workNum }
```

## UNION

通过 `UNION` 关键字，我们可以合并不同模式匹配到的结果。例如下列查询使用了 `UNION` 得到了会演奏萨克斯或者小号的人的名、姓和能够演奏的乐器：

```SPARQL
# filename: ex103.rq

PREFIX ab: <http://learningsparql.com/ns/addressbook#>

SELECT ?first ?last ?instrument 
WHERE
{
    ?person ab:firstName ?first ;
            ab:lastName ?last ;
            ab:instrument ?instrument . 

    { ?person ab:instrument "sax" . }

    UNION

    { ?person ab:instrument "trumpet" . }

}
```

## （NOT）IN

`IN` 关键字用来判断指定变量是否在指定枚举集合中：

```SPARQL
# filename: ex109.rq

PREFIX dm:  <http://learningsparql.com/ns/demo#>
PREFIX db: <http://dbpedia.org/resource/>

SELECT ?s ?cost ?location
WHERE
{
  ?s dm:location ?location ;
     dm:cost ?cost . 
  FILTER (?location IN (db:Montreal, db:Lisbon)) . 
}
```

上述 `FILTER` 过滤条件为：地点是否在蒙特利尔或者里斯本。如果在，则选出，否则不选。

`NOT IN` 为 `IN` 的否定形式。

## LIMIT 及 OFFSET

* `LIMIT`

`LIMIT` 用于限定返回数据显示的数量：

```SPARQL
# filename: ex116.rq

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?label 
WHERE
{ ?s rdfs:label ?label . }
LIMIT 2
```

* `OFFSET`

`OFFSET` 用于指定数据在返回数据的偏移量（即**显示第几个数据**）：

```SPARQL
# filename: ex118.rq

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?label 
WHERE
{ ?s rdfs:label ?label . }
OFFSET 3
```

## ORDER BY 和 GROUP BY [HAVING <条件>]

* `ORDER BY`

关键词 `ORDER BY` 告诉查询处理器，按照指定变量对返回结果进行排序（默认升序，降序需指明 `DESC(?变量名)`）:

```SPARQL
# filename: ex146.rq

PREFIX e: <http://learningsparql.com/ns/expenses#>

SELECT ?description ?date ?amount
WHERE
{
  ?meal e:description ?description ;
        e:date ?date ;
        e:amount ?amount . 
}

ORDER BY ?amount
```

```SPARQL
# filename: ex148.rq

PREFIX e: <http://learningsparql.com/ns/expenses#>

SELECT ?description ?date ?amount
WHERE
{
  ?meal e:description ?description ;
        e:date ?date ;
        e:amount ?amount . 
}

ORDER BY DESC(?amount)

```

* `GROUP BY`

`GROUP BY` 会将返回结果按照指定规则分组，可用 `HAVING <条件>` 做进一步条件限定：

```SPARQL
# filename: ex164.rq

PREFIX e: <http://learningsparql.com/ns/expenses#> 

SELECT ?description (SUM(?amount) AS ?mealTotal)
WHERE
{
  ?meal e:description ?description ;
        e:amount ?amount . 
}
GROUP BY ?description
HAVING (SUM(?amount) > 20)
```

## AS

`AS` 用于给数据起名字，例如：

```SPARQL
# filename: ex139.rq

PREFIX e: <http://learningsparql.com/ns/expenses#> 

SELECT ?description ?amount ((?amount * .2) AS ?tip) 
       ((?amount + ?tip) AS ?total)
WHERE
{
  ?meal e:description ?description ;
        e:amount ?amount . 
}
```

## BIND

`BIND` 用于给变量赋值。常见用法为 `BIND (数字计算 AS ?变量名)`

```SPARQL
# 来源：https://www.w3.org/TR/sparql11-query/#bind

PREFIX  dc:  <http://purl.org/dc/elements/1.1/>
PREFIX  ns:  <http://example.org/ns#>

SELECT  ?title ?price
{  { ?x ns:price ?p .
     ?x ns:discount ?discount
     BIND (?p*(1-?discount) AS ?price)
   }
   {?x dc:title ?title . }
   FILTER(?price < 20)
}
```

## VALUES

`VALUES` 提供了更方便的过滤数据的方式。其一般格式如下：

```SPARQL
VALUES(?变量名1 [?变量名2 ...])
{
    （变量名1应取值 [?变量名2应取值 ...]）
    （变量名1应取值 [?变量名2应取值 ...]）
     ...
}
# 当条件仅有一个变量时，小括号可以省略
```

将 `VALUES` 子句放在 `WHERE` 条件中，就可以筛选出变量名满足指定取值的数据：

```SPARQL
# filename: ex498.rq

PREFIX e: <http://learningsparql.com/ns/expenses#> 

SELECT ?description ?date ?amount
WHERE
{
  ?meal e:description ?description ;
        e:date ?date ;
        e:amount ?amount . 

  VALUES (?date ?description) {
         ("2011-10-15" "lunch") 
         ("2011-10-16" "dinner")
  } 

}
```

上述查询则会从返回结果中过滤出 **2011-10-15 的午饭**和 **2011-10-16 的晚饭**的相关数据。

## UNDEF

`UNDEF` 关键词代表任意值。下面一个例子返回结果满足这样的条件：任意时间下满足 `?description` 为 `"lunch"`的数据，以及任意 `?description` 下时间为 `"2011-10-16"` 的数据。

```SPARQL
# filename: ex500.rq

PREFIX e: <http://learningsparql.com/ns/expenses#> 

SELECT ?description ?date ?amount
WHERE
{
  ?meal e:description ?description ;
        e:date ?date ;
        e:amount ?amount . 

  VALUES (?date ?description) {
         (UNDEF "lunch") 
         ("2011-10-16" UNDEF) 
  }
}
```

## SERVICE

要查询远端的数据，一种方式是直接在 `FROM` 关键字之后指明 RDF 文件 URI：

```SPARQL
# filename: ex166.rq

PREFIX dc: <http://purl.org/dc/elements/1.1/>

SELECT ?title
FROM <http://dig.csail.mit.edu/2008/webdav/timbl/foaf.rdf>
WHERE { ?s dc:title ?title .}
```

除了 `FROM <URI>` 查询远端的 RDF 序列化文件，我们还可以用 `SERVICE` 关键字访问远端的数据：

```SPARQL
# filename: ex167.rq

PREFIX cat:     <http://dbpedia.org/resource/Category:>
PREFIX skos:    <http://www.w3.org/2004/02/skos/core#>
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl:     <http://www.w3.org/2002/07/owl#>
PREFIX foaf:    <http://xmlns.com/foaf/0.1/>

SELECT ?p ?o 
WHERE
{
  SERVICE <http://DBpedia.org/sparql>
  { SELECT ?p ?o 
    WHERE { <http://dbpedia.org/resource/Joseph_Hocking> ?p ?o . }
  }
}
```

```SPARQL
# filename: ex170.rq

PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX gp:   <http://wifo5-04.informatik.uni-mannheim.de/gutendata/resource/people/>

SELECT ?p ?o 
WHERE
{ 
  SERVICE <http://wifo5-04.informatik.uni-mannheim.de/gutendata/sparql>
  { gp:Hocking_Joseph ?p ?o . }
}
```

亦可从多个远端数据源联合查询：

```SPARQL
# filename: ex172.rq

PREFIX cat:  <http://dbpedia.org/resource/Category:>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX gp:   <http://wifo5-04.informatik.uni-mannheim.de/gutendata/resource/people/>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

SELECT ?dbpProperty ?dbpValue ?gutenProperty ?gutenValue 
WHERE
{
  SERVICE <http://DBpedia.org/sparql>
  {
    <http://dbpedia.org/resource/Joseph_Hocking> ?dbpProperty ?dbpValue .
  }

  SERVICE <http://wifo5-04.informatik.uni-mannheim.de/gutendata/sparql>
  {
    gp:Hocking_Joseph ?gutenProperty ?gutenValue . 
  }
}
```

{% note info %}

注意：由于`SERVICE` 方式的远端查询在服务器端执行， `SERVICE` 后的 URI 应是远端 SPARQL 执行入口。

{% endnote %}

# 常见函数

## MAX()、MIN()、AVG()、SUM()、COUNT()

这些聚集函数与 SQL 中类似，不再赘述。

# 总结

本文主要介绍了 SPARQL 语言的功能和基本语法，要点如下：

* 在 SPARQL 语句中，通常以 `？变量名` 表示变量；而常量一般为字符串、数字及 URI，其中 URI 由尖括号（< >）包裹。
* SPARQL 中使用了多种标点符号，具有丰富的含义。
* SPARQL 支持多种关键词查询数据。这些关键词包括 `SELECT` 、`CONSTRUCT` 、`DESCRIBE` 、 `ASK` 。
* SPARQL 提供了很多关键字，能够表达复杂的查询任务。
* SPARQL 支持许多强大的函数，提高了其易用性。

# 参考资料

* Bob DuCharme. [Learning SPARQL: querying and updating with SPARQL 1.1](http://learningsparql.com/)
* [SPARQL 1.1 Query Language W3C Recommendation](https://www.w3.org/TR/sparql11-query/).

# 推荐阅读

* [Introduction to: SPARQL](https://www.dataversity.net/introduction-to-sparql/)