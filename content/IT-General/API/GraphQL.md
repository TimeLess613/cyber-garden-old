---
tags:
  - IT/API
---
## 概念

> 与传统的RESTful API不同，GraphQL允许客户端应用程序在一个请求中指定其需要的数据，并只返回所需的数据，而不是像RESTful API一样返回固定的数据结构。这使得客户端可以更精确地控制其数据需求，减少了不必要的数据传输，提高了效率。
> 虽然GraphQL名称中包含"Graph"，但它更多地涉及数据的图形表示，而不是可视化图形。GraphQL查询通常以树形结构表示，这个结构对应于要获取的数据的层次结构，这些数据可能是图形结构（如关联的数据库表）。

> 在数据的图形表示中，数据元素通常被表示为节点（nodes），而节点之间的关系则以边（edges）连接。
> 这些节点和边的组合形成了一个图形，其中节点代表实体或对象，而边表示这些实体之间的关系。这种表示方式非常适合用于表示具有多对多关系、层次结构或网络结构的数据。
> - 树形结构：树是一种常见的数据图形表示，其中根节点连接到一个或多个子节点，每个子节点也可以连接到其他子节点，以此类推。这种结构用于表示层次性数据，如文件系统、组织结构等。
> - 关联数据图：在关联数据图中，不同实体之间的关系以边的形式表示。例如，社交媒体中的朋友关系图可以表示为一个关联数据图，其中用户是节点，而他们之间的关系（朋友关系）是边。
> - 数据库关系图：数据库中的表可以通过它们之间的外键关系来表示为数据图。每个表是一个节点，而外键关系是边。


## 有用

> https://graphql.org/learn/introspection/

如果不知道有什么type可用：
```GraphQL
{
  __schema {
    types {
      name
    }
  }
}
```
然后根据响应，单独查看特定type：
```GraphQL
{
  __type(name: "Droid") {
    name
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}
```


## 语法

> https://graphql.org/learn/queries/

### 变量与参数

- 传递参数：以下面的`hero(episode: $ep)`为例，`episode`是`hero`查询字段的一个参数，而`$ep`是传给这个参数的变量值。变量定义参照下一条，固定值的话则直接`hero(episode: 1)`这种形式。
- 变量定义：`query ($variableName: VariableType)`
	- 类似这个定义格式：[[编程习惯#type hints]]

### Fragments

片段。
类似封装重用，将共同需要请求的字段单独定义，然后请求文的多个地方可以**用`...<fragment名>`调用**。


### 内联片段

```
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}

---variables---
{
  "ep": "JEDI"
}
```

结果：即从其他地方搜索想要的字段，合并到hero的结果中。
**从哪里搜索——`... on <TypeName>`**。
```
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "primaryFunction": "Astromech"
    }
  }
}
```




## 例：python发送请求

```python
import requests
import json

# GraphQL端点URL
url = 'https://your-graphql-api.com/graphql'

# GraphQL查询字符串
query = """
query ($types: [TimelineEventType!], $startTime: DateTimeInput, $endTime: DateTimeInput, $sourceEndpointQuery: EntityQueryInput, $sourceEntityQuery: EntityQueryInput, $targetEndpointQuery: EntityQueryInput, $targetEntityQuery: EntityQueryInput, $activityQuery: ActivityQueryInput, $alertQuery: AlertQueryInput, $relatedTo: EventId, $after: Cursor, $limit: Int!, $sortOrder: SortOrder) {
  timeline(categories: [THREAT_HUNTER], sourceEntityQuery: $sourceEntityQuery, types: $types, startTime: $startTime, endTime: $endTime, activityQuery: $activityQuery, alertQuery: $alertQuery, sourceEndpointQuery: $sourceEndpointQuery, targetEndpointQuery: $targetEndpointQuery, targetEntityQuery: $targetEntityQuery, relatedTo: $relatedTo, after: $after, first: $limit, sortOrder: $sortOrder) {
    edges {
      cursor
      node {
        eventId
        timestamp
        eventType
        ... # 省略部分细节以保持简洁
      }
    }
  }
}
"""

# 变量字典
variables = {
    "types": ["SOME_EVENT_TYPE"],
    "startTime": "2023-01-01T00:00:00Z",
    "endTime": "2023-01-02T00:00:00Z",
    "limit": 10,
    # 添加更多变量...
}

# 请求头
headers = {
    'Content-Type': 'application/json',
    # 如果需要的话，还可以添加更多头信息，比如Authorization头
}

# 构建请求数据
request_data = {
    'query': query,
    'variables': variables
}

# 发送POST请求
response = requests.post(url, json=request_data, headers=headers)

# 打印响应数据
print(json.dumps(response.json(), indent=2))

```

