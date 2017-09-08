#How to build a blog in rails
建立一个新Rails项目
open iTerm
mkdir 12in12
rails new blog
cd blog
git init
git add .
git commit -m "Initial Commit"
打开atom
在终端打开另外一个窗口command+T

rails g controller posts
修改routes.rb
resources :posts
root "posts#index"
修改posts_controller.rb
def index
end
增加posts.index页面
touch app/views/posts/index.html.erb
<h1>"This is index"</h1>
刷新页面
修改routes.rb
def new
end
新增posts.new页面
touch app/views/posts/new.html.erb
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
rails g model Post title:string body:text
rake db:migrate
在网址栏输入http://localhost:3000/posts/new
修改posts_controller.rb
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
输入title和body，点击submit
提示缺少post／show.html.erb
touch app/views/posts/show.html.erb
修改show.html.erb
<h1 class="title">
  <%= @post.title %>
</h1>

<p class="date">
  Submitted <%= time_ago_in_words(@post.created_at) %> Ago
</p>

<p class="body">
  <%= @post.body %>
</p>
修改posts_controller.rb
def show
  @post = Post.find(params[:id])
end
刷新页面看一下
修改posts_controller.rb
def index
  @posts = Post.all
end
修改posts/index.html.erb
<% @posts.each do |post| %>
  <div class="post_wrapper">
    <h2 class="title"><%= link_to post.title, post %></h2>
    <p class="date"><%= post.created_at.strftime("%B, %d, %Y") %></p>
  </div>
<% end %>
回到主页http://localhost:3000
保存：
git status
git add .
git commit -am "Posts Controller"
