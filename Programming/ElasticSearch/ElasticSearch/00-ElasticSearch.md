# ElasticSearch

# Создаем индекс

## Создаем индекс#1

```json
PUT /channel-parser-settings
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "chat_name": {
        "type": "keyword"
      },
      "source": {
        "type": "keyword"
      },
      "chat_id": {
        "type": "long"
      },
      "last_message_id": {
        "type": "long"
      }
    }
  }
}
```

## Создаем индекс#2

```json
PUT /channel-parser-messages5
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    },
    "analysis": {
      "analyzer": {
        "ru_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "synonym_filter", "ru_stemming", "word_delimiter", "ru_stopwords"],
          "char_filter": ["ru_mappings_char_filter"]
        }
      },
      "char_filter": {
        "ru_mappings_char_filter": {
          "type": "mapping",
          "mappings": [
            "ё => е",
            "Ё => Е",
            ", => ."
          ]
        }
      },
      "filter": {
        "synonym_filter": {
          "type": "synonym_graph",
          "synonyms": [
            "гкэталон,Компания Эталон,ГК Эталон",
            "соуэталон,«Эталон»,Эталон,«СОУ» Эталон,СОУ Эталон",
            "эталон,эталонным,эталонный,эталоном,эталонной,эталонную,эталонов"
          ]
        },
        "ru_stopwords": {
          "type": "stop",
            "stopwords": "а,без,более,бы,был,была,были,было,быть,в,вам,вас,весь,во,вот,все,всего,всех,вы,где,да,даже,для,до,его,ее,если,есть,еще,же,за,здесь,и,из,или,им,их,ихние,к,как,ко,когда,кто,ли,либо,мне,может,мы,на,надо,наш,не,него,нее,нет,ни,них,но,ну,о,об,однако,он,она,они,оно,от,очень,по,под,при,с,со,так,также,такой,там,те,тем,то,того,тоже,той,только,том,ты,у,уже,хотя,чего,чей,чем,что,чтобы,чье,чья,эта,эти,это,я,a,an,and,are,as,at,be,but,by,for,if,in,into,is,it,no,not,of,on,or,such,that,the,their,then,there,these,they,this,to,was,will,with"
        },
        "ru_stemming": {
          "type": "snowball",
          "language": "russian"
        },
        "word_delimiter": {
          "catenate_all": "true",
          "type": "word_delimiter",
          "preserve_original": "true"
        }
      }
    }
  },
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "chat_id": {
        "type": "long"
      },
      "chat_title": {
        "type": "keyword"
      },
      "message_id": {
        "type": "long"
      },
      "message_date": {
        "type": "date"
      },
      "message_text": {
        "type": "text",
        "search_analyzer": "ru_analyzer",
        "analyzer": "ru_analyzer"
      },
      "username": {
        "type": "keyword"
      },
      "source": {
        "type": "keyword"
      }
    }
  }
}
```

## Удаляем индекс

```json
DELETE /channel-parser-messages
```

## Получаем информацию по индексу

```json
GET /channel-parser-settings
```

## Создаем новый документ

```json
POST channel-parser-settings/_doc
{
  "chat_name": "3",
  "chat_id": 3,
  "last_message_id": 3,
  "source": "telegram"
}
```

## Анализируем текст, на то какие ключевые слова выдаст

```json
POST /channel-parser-messages5/_analyze
{
    "analyzer": "ru_analyzer",
    "text": "Эталоный"
}
```

## Перегоняем данные из индекса в индекс

```json
POST /_reindex
{
  "source": {
    "index": "channel-parser-messages-5"
  },
  "dest": {
    "index": "channel-parser-messages"
  }
}
```

{
"query": {
"bool": {
"should": {
"query_string": {
"query": "эталон",
"default_field": "message_text.company"
}
}
}
}
}

## Обновляем settings у индекса

Обновляем settings у индекса:

-   необходимо закрыть индекс
-   установить новые настройки
-   открыть индекс

```json
POST /channel-parser-messages/_close
PUT /channel-parser-messages/_settings
{
  "analysis": {
    "analyzer": {
      "ru_analyzer": {
        "type": "custom",
        "tokenizer": "standard",
        "filter": ["lowercase", "synonym_filter", "ru_stemming", "word_delimiter", "ru_stopwords"],
        "char_filter": ["ru_mappings_char_filter"]
      }
    },
    "char_filter": {
      "ru_mappings_char_filter": {
        "type": "mapping",
        "mappings": [
          "ё => е",
          "Ё => Е",
          ", => ."
        ]
      }
    },
    "filter": {
      "synonym_filter": {
        "type": "synonym_graph",
        "synonyms": [
          "гкэталон,Компания Эталон,Компании Эталон,ГК Эталон",
          "соуэталон,«Эталон»,\"Эталон\",«СОУ» Эталон,СОУ Эталон,\"СОУ\" Эталон,модуль «Эталон»,модуль \"Эталон\",модуль эталон",
          "сис, Сибирские интеграционные системы, ООО «Сибирские интеграционные системы»",
          "эталон,эталонным,эталонный,эталоном,эталонной,эталонную,эталонов"
        ]
      },
      "ru_stopwords": {
        "type": "stop",
          "stopwords": "а,без,более,бы,был,была,были,было,быть,в,вам,вас,весь,во,вот,все,всего,всех,вы,где,да,даже,для,до,его,ее,если,есть,еще,же,за,здесь,и,из,или,им,их,ихние,к,как,ко,когда,кто,ли,либо,мне,может,мы,на,надо,наш,не,него,нее,нет,ни,них,но,ну,о,об,однако,он,она,они,оно,от,очень,по,под,при,с,со,так,также,такой,там,те,тем,то,того,тоже,той,только,том,ты,у,уже,хотя,чего,чей,чем,что,чтобы,чье,чья,эта,эти,это,я,a,an,and,are,as,at,be,but,by,for,if,in,into,is,it,no,not,of,on,or,such,that,the,their,then,there,these,they,this,to,was,will,with"
      },
      "ru_stemming": {
        "type": "snowball",
        "language": "russian"
      },
      "word_delimiter": {
        "catenate_all": "true",
        "type": "word_delimiter",
        "preserve_original": "true"
      }
    }
  }
}
POST /channel-parser-messages/_open
```

# Поиск

Поиск query_string:

```json
GET /channel-parser-messages/_search
{
  "query": {
    "query_string" : {
      "query": "пенсия",
      "default_field": "message_text",
      "default_operator": "or",
      "analyze_wildcard": true,
      "minimum_should_match": "-35%",
      "analyzer": "ru_analyzer"
    }
  }
}
```

Поиск query с использованием query_string:

```json
GET /channel-parser-settings
{
  "query": {
    "bool": {
      "must_not": {
        "query_string": {
          "query": "гкэталон",
          "default_field": "message_text.company"
        }
      },
      "should": {
        "query_string": {
          "query": "соуэталон",
          "default_field": "message_text.company"
        }
      }
    }
  }
}
```

##

```
PUT /channel-parser-settings
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  },
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "chat_name": {
        "type": "keyword"
      },
      "source": {
        "type": "keyword"
      },
      "chat_id": {
        "type": "long"
      },
      "from_date": {
        "type": "long"
      },
      "first_message_id": {
        "type": "long"
      },
      "last_message_id": {
        "type": "long"
      }
    }
  }
}

DELETE /channel-parser-settings

GET /channel-parser-settings/_search
{
	"_source": ["chat_id", "last_message_id", "chat_name"],
    "size": 10000,
    "query": {
		"bool": {
			"must": [
				{
					"term": {
						"source": "telegram"
					}
				}
			]
		}
	}
}

PUT /channel-parser-messages4
{
  "settings": {
    "index": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    },
    "analysis": {
      "analyzer": {
        "ru_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": ["lowercase", "ru_stemming", "word_delimiter", "ru_stopwords"],
          "char_filter": ["ru_mappings_char_filter"]
        },
        "ru_company_name": {
          "type": "custom",
          "tokenizer": "whitespace",
          "filter": ["lowercase", "synonym_filter", "ru_stopwords"],
          "char_filter": ["ru_mappings_char_filter", "dot_delete_filter"]
        }
      },
      "char_filter": {
        "ru_mappings_char_filter": {
          "type": "mapping",
          "mappings": [
            "ё => е",
            "Ё => Е"
          ]
        },
        "dot_delete_filter": {
          "type": "mapping",
          "mappings": [
            ". => "
          ]
        }
      },
      "filter": {
        "synonym_filter": {
          "type": "synonym_graph",
          "synonyms": [
            "гкэталон,Компания Эталон,Компании Эталон,ГК Эталон,ГК «Эталон»",
            """соуэталон,«Эталон»,"Эталон","Эталон",«СОУ» Эталон,СОУ Эталон,"СОУ" Эталон,модуль «Эталон»,модуль \"Эталон\",модуль эталон,система "Эталон""",
            "сис, Сибирские интеграционные системы, ООО «Сибирские интеграционные системы»"
          ]
        },
        "ru_stopwords": {
          "type": "stop",
            "stopwords": "а,без,более,бы,был,была,были,было,быть,в,вам,вас,весь,во,вот,все,всего,всех,вы,где,да,даже,для,до,его,ее,если,есть,еще,же,за,здесь,и,из,или,им,их,ихние,к,как,ко,когда,кто,ли,либо,мне,может,мы,на,надо,наш,не,него,нее,нет,ни,них,но,ну,о,об,однако,он,она,они,оно,от,очень,по,под,при,с,со,так,также,такой,там,те,тем,то,того,тоже,той,только,том,ты,у,уже,хотя,чего,чей,чем,что,чтобы,чье,чья,эта,эти,это,я,a,an,and,are,as,at,be,but,by,for,if,in,into,is,it,no,not,of,on,or,such,that,the,their,then,there,these,they,this,to,was,will,with"
        },
        "ru_stemming": {
          "type": "snowball",
          "language": "russian"
        },
        "word_delimiter": {
          "catenate_all": "true",
          "type": "word_delimiter",
          "preserve_original": "true"
        }
      }
    }
  },
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "chat_id": {
        "type": "long"
      },
      "chat_name": {
        "type": "keyword"
      },
      "message_id": {
        "type": "long"
      },
      "message_date": {
        "type": "date"
      },
      "message_text": {
        "type": "text",
        "search_analyzer": "ru_analyzer",
        "analyzer": "ru_analyzer",
        "fields": {
          "company": {
            "type": "text",
            "search_analyzer": "ru_company_name",
            "analyzer": "ru_company_name"
          }
        }
      },
      "username": {
        "type": "keyword"
      },
      "source": {
        "type": "keyword"
      }
    }
  }
}

GET /channel-parser-messages

POST /channel-parser-messages3/_close
PUT /channel-parser-messages3/_settings
{
  "analysis": {
    "analyzer": {
      "ru_analyzer": {
        "type": "custom",
        "tokenizer": "standard",
        "filter": ["lowercase", "ru_stemming", "word_delimiter", "ru_stopwords"],
        "char_filter": ["ru_mappings_char_filter"]
      },
      "ru_company_name": {
        "type": "custom",
        "tokenizer": "whitespace",
        "filter": ["lowercase", "synonym_filter", "ru_stopwords"],
        "char_filter": ["ru_mappings_char_filter", "dot_delete_filter"]
      }
    },
    "char_filter": {
      "ru_mappings_char_filter": {
        "type": "mapping",
        "mappings": [
          "ё => е",
          "Ё => Е"
        ]
      },
      "dot_delete_filter": {
        "type": "mapping",
        "mappings": [
          ". => "
        ]
      }
    },
    "filter": {
      "synonym_filter": {
        "type": "synonym_graph",
        "synonyms": [
          "гкэталон,Компания Эталон,Компании Эталон,ГК Эталон,ГК «Эталон»",
          """соуэталон,«Эталон»,"Эталон","Эталон",«СОУ» Эталон,СОУ Эталон,"СОУ" Эталон,модуль «Эталон»,модуль \"Эталон\",модуль эталон,система "Эталон""",
          "сис, Сибирские интеграционные системы, ООО «Сибирские интеграционные системы»"
        ]
      },
      "ru_stopwords": {
        "type": "stop",
          "stopwords": "а,без,более,бы,был,была,были,было,быть,в,вам,вас,весь,во,вот,все,всего,всех,вы,где,да,даже,для,до,его,ее,если,есть,еще,же,за,здесь,и,из,или,им,их,ихние,к,как,ко,когда,кто,ли,либо,мне,может,мы,на,надо,наш,не,него,нее,нет,ни,них,но,ну,о,об,однако,он,она,они,оно,от,очень,по,под,при,с,со,так,также,такой,там,те,тем,то,того,тоже,той,только,том,ты,у,уже,хотя,чего,чей,чем,что,чтобы,чье,чья,эта,эти,это,я,a,an,and,are,as,at,be,but,by,for,if,in,into,is,it,no,not,of,on,or,such,that,the,their,then,there,these,they,this,to,was,will,with"
      },
      "ru_stemming": {
        "type": "snowball",
        "language": "russian"
      },
      "word_delimiter": {
        "catenate_all": "true",
        "type": "word_delimiter",
        "preserve_original": "true"
      }
    }
  }
}
POST /channel-parser-messages3/_open

POST /_reindex
{
  "source": {
    "index": "channel-parser-messages"
  },
  "dest": {
    "index": "channel-parser-messages4"
  }
}

POST /_reindex
{
  "source": {
    "index": "channel-parser-messages2"
  },
  "dest": {
    "index": "channel-parser-messages"
  }
}

POST /channel-parser-messages3/_analyze
{
    "analyzer": "ru_analyzer",
    "text": "100. ГК «Эталон» @Group_etalon"
}

POST /channel-parser-messages3/_analyze
{
    "analyzer": "ru_company_name",
    "text": """"#Калуга #Калужская_обл\n\n\"В Калужской области в тестовом режиме работает система "Эталон". С ее помощью можно следить за ситуацией на опасных участках дорог, анализировать и устранять их... Повышением безопасности на дорогах с 2019 года занимается нацпроект \"Безопасные и качественные дороги\". За это время на 173 км дорог появилось искусственное освещение, установлено около 150 проекционных пешеходов. Из них 85 зебр оборудовано в Калуге\".\nhttps://www.kaluga-poisk.ru/news/obschestvo/v-kaluzhskoy-oblasti-v-testovom-rezhime-rabotaet-sistema-etalon"ге"""
}

POST /channel-parser-messages2/_doc
{
  "chat_id": 2,
  "chat_name": "asd",
  "message_id": 12,
  "message_date": 100,
  "message_text": "взята «эталон»",
  "source": "telegram"
}
```
