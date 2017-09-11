# How to build a blog in rails 4


######（文字版）


----------


<div class="toc">
 <div class="toc">
 </div>
</div>

# 建立新项目


----------


##Step1: Initial Commit


**open iTerm**

`mkdir 12in12`
`rails new blog`
`cd blog`
`git init`
`git add .`
`git commit -m "Initial Commit"`
`atom .`

打开atom
在终端打开另外一个窗口command+T


# Posts controller


----------



##Step2: Posts controller


`rails g controller posts`

修改文件 routes.rb

```rb
resources :posts
root "posts#index"
```
修改文件 posts_controller.rb

```rb
def index
end
```

增加文件

`touch app/views/posts/index.html.erb`

修改文件 app/views/posts/index.html.erb

```erb
<h1>"This is index"</h1>
```

修改文件 posts_controller.rb

```erb
def new
end
```

新增文件

`touch app/views/posts/new.html.erb`

修改文件 app/views/posts/new.html.erb

```erb
  <h1>New Post</h1>
	<%= form_for :post, url: posts_path do |f|  %>
  <p>
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body %><br>
    <%= f.text_field :body %>
  </p>

  <p>
    <%= f.submit %>
  </p>

<% end %>
```

##Step3: Post model


`rails g model Post title:string body:text`
`rake db:migrate`

在网址栏输入http://localhost:3000/posts/new

修改文件 posts_controller.rb

```rb
def create
  @post = Post.new(post_params)
  @post.save

  redirect_to @post
end

private
  def post_params
    params.require(:post).permit(:title, :body)
  end
end
```

输入title和body，点击submit
提示缺少post／show.html.erb


##Step4: Posts views


`touch app/views/posts/show.html.erb`

修改文件 show.html.erb

```erb
<h1 class="title">
  <%= @post.title %>
</h1>

<p class="date">
  Submitted <%= time_ago_in_words(@post.created_at) %> Ago
</p>

<p class="body">
  <%= @post.body %>
</p>
```

修改文件 posts_controller.rb

```rb
def show
  @post = Post.find(params[:id])
end
```

刷新页面看一下

修改文件 posts_controller.rb

```rb
def index
  @posts = Post.all
end
```

修改文件 posts/index.html.erb

```erb
<% @posts.each do |post| %>
  <div class="post_wrapper">
    <h2 class="title"><%= link_to post.title, post %></h2>
    <p class="date"><%= post.created_at.strftime("%B, %d, %Y") %></p>
  </div>
<% end %>
```

回到主页http://localhost:3000

保存：
`git status`
`git add .`
`git commit -am "Posts Controller"`



# 前段代码


----------


##Step5: Styling and structure


修改文件 application.html.erb

```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Blog2</title>
    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
+    <%= stylesheet_link_tag    'application', 'http://fonts.goodleapis.com/css?family=Raleway:400,700' %>

    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
    <%= csrf_meta_tags %>
  </head>

  +<body>
+  <div id="sidebar">
+    <div id="logo">
+      <%= link_to root_path do %>
+        <%= image_tag "logo2.svg" %>
+      <% end %>
+    </div>
+
+    <ul>
+      <li class="category">Website</li>
+      <li><%= link_to "Blog", root_path %></li>
+      <li><%= link_to "About" %></li>
+    </ul>
+
+    <ul>
+      <li class="category">Social</li>
+      <li><a href="#">Twitter</a></li>
+      <li><a href="#">Instagram</a></li>
+      <li><a href="https://github.com/liubingjei">Github</a></li>
+      <li><a href="#">Email</a></li>
+    </ul>
+  
+      <p class="sign_in">Admin Login</p>
+  </div>

+    <div id="main_content">
+      <div id="header">
+        <p>All Posts</p>
+
+        <div class="buttons">
+          <button class="button"><%= link_to "New Post", new_post_path %></button>
+          <button class="button">Log Out</button>
+        </div>
+      </div>
+
+      <% flash.each do |name, msg| %>
+        <%= content_tag(:div, msg, class: "alert")%>
+      <% end %>
+
+      <%= yield %>
+    </div>

  </body>
</html>
```

添加全局scss文件

由于这些文件特别长，为了不影响阅读，重新做了链接：
1:[app/assets/stylesheets/_normalize.css.scss](http://liubingjie-blog.logdown.com/posts/2331427)
2:[assets/stylesheets/application.css.scss](http://liubingjie-blog.logdown.com/posts/2390152-2)
3:[app/assets/images/logo2.svg](http://liubingjie-blog.logdown.com/posts/2390169)


修改posts/new.html.erb

```erb
<div id="page_wrapper">
  <h1>New Post</h1>

  <%= form_for :post, url: posts_path do |f|  %>
    <p>
      <%= f.label :title %><br>
      <%= f.text_field :title %>
    </p>

    <p>
      <%= f.label :body %><br>
      <%= f.text_field :body %>
    </p>

    <p>
      <%= f.submit %>
    </p>

  <% end %>
</div>
```

修改 posts/edit.html.erb

```erb
<div id="post_content">
  <h1 class="title">
    <%= @post.title %>
  </h1>

  <p class="date">
    Submitted <%= time_ago_in_words(@post.created_at) %> Ago
  </p>

  <p class="body">
    <%= @post.body %>
  </p>
</div>
```

`git status`
`git add .`
`git commit -am "Styling and structure"`



##Step6: Add validation to post form


修改文件  model/post.rb

```rb
class Post < ActiveRecord::Base
  validates :title, presence: true, length: { minimum: 5 }
  validates :body, presence: true
end
```

修改文件  posts.controller.rb
```rb
def new
  @post = Post.new
end

def create
  @post = Post.new(post_params)
  if @post.save
    redirect_to @post
  else
    render 'new'
  end
end
```

修改文件  posts/new.html.erb
```erb
<%= form_for :post, url: posts_path do |f| %>
  <% if @post.errors.any? %>
    <div id="errors">
      <h2><%= pluralize(@post.errors.count, "error")%> prevented this post from saving</h2>
      <ul>
        <% @post.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
<% end %>
```

`git status`
`git add `
`git commit -am "Add validation to post form"`


##Step7: Edit and Delete posts


添加文件
`touch app/views/posts/edit.html.erb`
`touch app/views/posts/_form.html.erb`

修改文件 new.html.erb
```erb
<div id="page_wrapper">
  <h1>New Post</h1>

  <%= render 'form' %>
</div>
```

修改文件 edit.html.erb
```erb
<div id="page_wrapper">
  <h1>Edit Post</h1>

  <%= render 'form' %>
</div>
```

修改文件 form.html.erb
```erb
<%= form_for @post do |f| %>
  <% if @post.errors.any? %>
    <div id="errors">
      <h2><%= pluralize(@post.errors.count, "error")%> prevented this post from saving</h2>
      <ul>
        <% @post.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
<% end %>

<%= form_for :post, url: posts_path do |f|  %>
  <p>
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </p>

  <p>
    <%= f.label :body %><br>
    <%= f.text_field :body %>
  </p>

  <p>
    <%= f.submit %>
  </p>

<% end %>
```

修改文件 show.html.erb

```erb
<p class="date">
  Submitted <%= time_ago_in_words(@post.created_at) %> Ago
  | <%= link_to "Edit", edit_post_path(@post) %>
  | <%= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?"}%>
</p>
```

修改文件 post.controller.rb

```rb
def edit
  @post = Post.find(params[:id])
end

def update
  @post = Post.find(params[:id])

  if @post.update(params[:post].permit(:title, :body))
    redirect_to @post
  else
    render 'edit'
  end
end

def destroy
  @post = Post.find(params[:id])
  @post.destroy

  redirect_to posts_path
end
```

`git status`
`git add `
`git commit -am "Edit and Delete posts"`


# COMMENTS


----------


##Step8: Add comments


`rails g model Comment name:string body:text post:references`
`rake db:migrate`

修改文件 posts.model

`has_many :commnets`

修改文件 routes.rb

```rb
resources :posts do
  resources :comments
end
```

`rails g controller Commnets`
`touch app/views/comments/_comment.html.erb`
`touch app/views/comments/_form.html.erb`

修改文件 comments.controller.rb

```rb
def create
  @post = Post.find(params[:post_id])
  @comment = @post.comments.create(params[:comment].permit(:name, :body))

  redirect_to post_path(@post)
end
```

修改文件 comment.html.erb

```erb
<div class="comment clearfix">
  <div class="comment_content">
    <p class="comment_name"><strong><%= comment.name %></strong></p>
    <p class="comment_body"><%= comment.body %></p>
    <p class="comment_time"><%= time_ago_in_words(comment.created_at) %> Ago</p>
  </div>
</div>
```

修改文件 form.html.erb

```erb
<%= form_for([@post, @post.comments.build]) do |f| %>
  <p>
    <%= f.label :name %>
    <%= f.text_field :name %>
  </p>

  <p>
    <%= f.label :body %>
    <%= f.text_area :body %>
  </p>
  <br>
  <p>
    <%= f.submit %>
  </p>
<% end %>
```

修改文件 posts/show.html.erb

```erb
<div id="comments">
  <h2><%= @post.comments.count %> Comments</h2>
  <%= render @post.comments %>

  <h3>Add a comment:</h3>
  <%= render "comments/form" %>
</div>
```

现在就可以留言了
实作comment destroy action

修改文件 comment.html.erb

```erb
<p><%= link_to 'Delete', [comment.post, comment],
                method: :delete,
                class: "button",
                data: { confirm: "Are you sure?" }%>
</p>
```

修改文件 comments.contoller.erb

```erb
def destroy
  @post = Post.find(params[:post_id])
  @comment = @post.comments.find(params[:id])
  @comment.destroy

  redirect_to post_path(@post)
end
```

现在就可以删除comment了

修改文件 posts.model.rb

```rb
has_many :comments, dependent: :destroy
```

`git status`
`git add `
`git comment -m "Add comments"`


# ABOUT


----------


##Step9: Add About

实作About按钮

修改文件 views/layouts/application.html.erb

```erb
<li><%= link_to "About", about_path %></li>
```

创建about action

`rails g contoller pages`

修改文件 pages.controller.rb

```rb
def about
end
```

创建about页面

`touch app/views/pages/about.html.erb`

修改文件 about.html.erb

```erb
<div id="page_wrapper">
	<div id="profile_image">
		<%= image_tag "profile.jpeg" %>
	</div>

	<div id="content">
		<h1>Hey, I'm Mackenzie Child</h1>
		<p>Welcome to week 2 of my 12 Web Apps in 12 Weeks Challenge.</p>
		<p>This week I built a blog in Rails 4. You're actually on the demo application right now. Cool stuff, right!.</p>
		<p>If you'd like to follow along as I learn more Ruby on Rails, find me on Twitter <a href="http://twitter.com/mackenziechild">@MackenzieChild</a> or on the web at <a href="http://mackenziechild.me">mackenziechild.me</a>, and get my posts and exclusive content in your inbox by signing up for my newsletter <a href="http://eepurl.com/02olT">here</a>.</p>
	</div>
</div>
```

刷新页面，点击About

优化页面，使页面上方的标签显示所在位置

修改文件 layouts/application.html.erb

```erb
<% if current_page?(root_path) %>
  <p>All Posts</p>
<% elsif current_page?(about_path) %>
  <p>About</p>
<% else %>
  <%= link_to "Back to All Posts", root_path %>
<% end %>
```

`git status`
`git add `
`git comment -m "Add About"`

# DEVISE


----------


##Step10: Add devise and users


实作devise
`gem 'devise'`
`bundle`
`rails g devise:install`

修改文件 config/environments/development.rb
```rb
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```
加在end之上

`rails g devise:views`
`rails g devise User`
`rake db:migrate`

重启rails s

在网址栏位输入http://localhost:3000/users/sign_up

注册一个账号

修改文件 views/devise/sessions/new.html.erb

```erb
<div id="page_wrapper"> #这个是调整页面布局的
  <h2>Log in</h2>

  <%= form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
  .....
  <% end %>
  <br>
  <%= render "devise/shared/links" %>

</div>
```

修改文件 posts.controller.rb

```rb
before_action :authenticate_user!, except: [:idex, :show]
```

修改文件 layous/application.html.erb

```erb
......
+  <% if !user_signed_in? %>
    <p class="sign_in">Admin Login</p>
+  <% end %>
......
+    <% if user_signed_in? %>
      <div class="buttons">
        <button class="button"><%= link_to "New post", new_post_path %></button>
        <button class="button">Log Out</button>
      </div>
+    <% end %>
```

修改文件 posts/show.html.erb

```erb
<p class="date">
  Submitted <%= time_ago_in_words(@post.created_at) %> Ago
+  <% if user_signed_in? %>
    | <%= link_to "Edit", edit_post_path(@post) %>
    | <%= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?"}%>
+  <% end %>
</p>
```

修改文件 comments/comment.html.erb

```erb
+ <% if user_signed_in? %>
  <p><%= link_to 'Delete', [comment.post, comment],
                  method: :delete,
                  class: "button",
                  data: { confirm: "Are you sure?" }%>
  </p>
+ <% end %>
```

`git status`
`git add `
`git commit -am "Add devise and users"`

未完待续...
