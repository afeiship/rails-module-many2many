# rails-module-many2many
> Rails module for many to many model associations.

## step by step
- create models:
```bash
# 多对多 主体1
rails g model Article name:string published_on:date content:text

# 多对多 主体2
rails g model Tag name:string

# 多对多 关系体：一般命名：主体1s + 主体2 (主体1/主体2 哪个更重要，就把哪个放前面)
rails g model ArticlesTag article_id:integer tag_id:integer

# 推荐用下面的方式
# 1. 可以产生 belongs_to 关系
# 2. 可以 migration 中产生如下关系对
    # t.references :article, null: false, foreign_key: true
    # t.references :tag, null: false, foreign_key: true
rails g model ArticlesTag article:references tag:references
```

### solution1
> 这种方式会自动处理好 destroy 的情况。
- create associations1
```rb
# model/article.rb
class Article < ApplicationRecord
    has_and_belongs_to_many :tags
end

# model/tag.rb
class Tag < ApplicationRecord
    has_and_belongs_to_many :articles
end
```

### destroy
```rb
a1 = Article.first
# will show tags
a1.tags

# will show many records
ArticlesTag.all

# destroy
a1.destory

# 相关联的内容会被删除掉
a1.tags # []
ArticlesTag.all # []
```


### solution2
- create associations2(Maybe this is another way)
```rb
class Article < ActiveRecord::Base
    has_many :articles_tags
    has_many :tags, :through => :articles_tags
end

class ArticlesTags < ActiveRecord::Base
    belongs_to :article
    belongs_to :tag
end

class Tag < ActiveRecord::Base
    has_many :articles_tags
    has_many :contacts, :through => :articles_tags
end
```

## rake tasks:
```bash
rake db:migrate
rake db:seed
```

## seed code:
```rb
a1 = Article.first
a1.tags<<Tag.first
a1.tags<<Tag.second
a1.tags<<Tag.third

a1.tag_ids
# [1,2,3]
```