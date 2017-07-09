# rails-module-many2many
> Rails module for many to many model associations.


## step by step
+ create models:
```bash
rails g model Article name:string published_on:date content:text
rails g model Tag name:string
rails g model ArticlesTags article_id:integer tag_id:integer
```

+ create associations1
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

+ create associations2(Maybe this is another way)
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