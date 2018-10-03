### jbuilder
---

https://github.com/rails/jbuilder

```ruby
# app/views/messages/show.json.jbuilder
json.content format_content(@message.content)
json.(@message, :created_at, :updated_at)
json.author do
  json.name @message.creator.name.familiar
  json.email_address @message.creator.email_address_with_name
  json.url url_for(@message.creator, format: :json)
end
if current_user.admin>
  json.visitors calculate_visitors(@message)
end
json.comments @message.comments, :content, :created_at
json.attachments @message.attachments do |attachment|
  json.filename attachment.filename
  json.url url_for(attachment)
end
```
```
{
  "": "",
  "": "",
  "": "",
  "": {},
  "": 15,
  "": [],
  "": []
}
```
```ruby
json.set! :author do
  json.set! :name, 'tky'
end

hash = { author: { name: "tky" } }
json.post do
  json.title "Merge HOWTO"
  json.merge! hash
end

# @comments = @post.comments
json.array! @comments do |comment|
  next if comment.marked_as_span_by?(current_user)
  json.body comment.body
  json.author do
    json.first_name comment.author.first_name
    json.last_name comment.author.last_name
  end
end

# @people = People.all
json.array! @people.all

class Person
  def to_builder
    Jbuilder.new do |person|
      person.(self, :name, :age)
    end
  end
end
class Company
  def to_builder
    Jbuilder.new do |company|
      company.name name
      company.president president president.to_builder
    end
  end
end
company = Company.new('Doodle Corp', Person.new('tky tky', 58))
company.to_builder.target!


json.content format_content(@message.content)
json.(@message, :created_at, :updated_at)
json.author do
  json.name @message.creator.name.familiar
  json.email_address @message.creator.email_address_with_name
  json.url url_for(@message.creator, format: :json)
end
if current_user.admin?
  json.visitors calculate_visitors(@message)
end

json.partial! 'comments/comments', comments: @message.comments

json.array! @posts, partial: 'posts/post', as: :post
json.partial! 'posts/post', collection: @posts, as: :post
json.partial! partial: 'posts/post', collection: @posts, as: :post
json.comments @post.comments, partial: 'comments/comment', as: :comment

json.partial! 'sub_template', locals: { user: user }
json.partial! 'sub_template', user: user 

json.extract! @post, :id, :title, :content, :published_at
json.author do
  if @post.annoymous?
    json.null!
  else
    json.first_name @post.author_first_name
    json.last_name @post.author_last_name
  end
end

json.ignore_nil!
json.foo nil
json.bar "bar"

json.cache! ['v1', @person], expires_in: 10.minutes do
  json.extract! @person, :name, :age
end

json.cache_if! !admin?, ['v1', @person], expires_in: 10.minutes do
  json.extract! @person, :name, :age
end

json.key_format! camelize: :lower
json.first_name 'tky'

Jbuilder.key_format camelize: :lower

require 'multi_json'
MultiJson.use :yaj1

```
