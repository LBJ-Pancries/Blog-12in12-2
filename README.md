#How to build a blog in rails
建立一个新Rails项目
open iTerm
`mkdir 12in12`
`rails new blog`
`cd blog`
`git init`
`git add .`
`git commit -m "Initial Commit"`
打开atom
在终端打开另外一个窗口command+T

`rails g controller posts`
修改routes.rb
```rb
resources :posts
root "posts#index"
```
修改posts_controller.rb
```rb
def index
end
```
增加posts.index页面
`touch app/views/posts/index.html.erb`
```erb
<h1>"This is index"</h1>
```
修改posts_controller.rb
```erb
def new
end
```
新增posts.new页面
`touch app/views/posts/new.html.erb`
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
`rails g model Post title:string body:text`
`rake db:migrate`
在网址栏输入http://localhost:3000/posts/new
修改posts_controller.rb
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
`touch app/views/posts/show.html.erb`
修改show.html.erb
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
修改posts_controller.rb
```rb
def show
  @post = Post.find(params[:id])
end
```
刷新页面看一下
修改posts_controller.rb
```rb
def index
  @posts = Post.all
end
```
修改posts/index.html.erb
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

样式
修改application.html.erb
```erb application.html.erb
<body>
  <div id="sidebar">
    <div id="logo">
      <%= link_to root_path do %>
        <%= image_tag "logo2.svg" %>
      <% end %>
    </div>

    <ul>
      <li class="category">Website</li>
      <li><%= link_to "Blog", root_path %></li>
      <li><%= link_to "About" %></li>
    </ul>

    <ul>
      <li class="category">Social</li>
      <li><a href="#">Twitter</a></li>
      <li><a href="#">Instagram</a></li>
      <li><a href="https://github.com/liubingjei">Github</a></li>
      <li><a href="#">Email</a></li>
    </ul>

  </div>
```
添加scss文件
`touch app/assets/stylesheets/_normalize.css.scss`
修改app/assets/stylesheets/normalize.css.scss
```scss
  /*! normalize.css v3.0.1 | MIT License | git.io/normalize */

  /**
   * 1. Set default font family to sans-serif.
   * 2. Prevent iOS text size adjust after orientation change, without disabling
   *    user zoom.
   */

  html {
    font-family: sans-serif; /* 1 */
    -ms-text-size-adjust: 100%; /* 2 */
    -webkit-text-size-adjust: 100%; /* 2 */
  }

  /**
   * Remove default margin.
   */

  body {
    margin: 0;
  }

  /* HTML5 display definitions
     ========================================================================== */

  /**
   * Correct `block` display not defined for any HTML5 element in IE 8/9.
   * Correct `block` display not defined for `details` or `summary` in IE 10/11 and Firefox.
   * Correct `block` display not defined for `main` in IE 11.
   */

  article,
  aside,
  details,
  figcaption,
  figure,
  footer,
  header,
  hgroup,
  main,
  nav,
  section,
  summary {
    display: block;
  }

  /**
   * 1. Correct `inline-block` display not defined in IE 8/9.
   * 2. Normalize vertical alignment of `progress` in Chrome, Firefox, and Opera.
   */

  audio,
  canvas,
  progress,
  video {
    display: inline-block; /* 1 */
    vertical-align: baseline; /* 2 */
  }

  /**
   * Prevent modern browsers from displaying `audio` without controls.
   * Remove excess height in iOS 5 devices.
   */

  audio:not([controls]) {
    display: none;
    height: 0;
  }

  /**
   * Address `[hidden]` styling not present in IE 8/9/10.
   * Hide the `template` element in IE 8/9/11, Safari, and Firefox < 22.
   */

  [hidden],
  template {
    display: none;
  }

  /* Links
     ========================================================================== */

  /**
   * Remove the gray background color from active links in IE 10.
   */

  a {
    background: transparent;
  }

  /**
   * Improve readability when focused and also mouse hovered in all browsers.
   */

  a:active,
  a:hover {
    outline: 0;
  }

  /* Text-level semantics
     ========================================================================== */

  /**
   * Address styling not present in IE 8/9/10/11, Safari, and Chrome.
   */

  abbr[title] {
    border-bottom: 1px dotted;
  }

  /**
   * Address style set to `bolder` in Firefox 4+, Safari, and Chrome.
   */

  b,
  strong {
    font-weight: bold;
  }

  /**
   * Address styling not present in Safari and Chrome.
   */

  dfn {
    font-style: italic;
  }

  /**
   * Address variable `h1` font-size and margin within `section` and `article`
   * contexts in Firefox 4+, Safari, and Chrome.
   */

  h1 {
    font-size: 2em;
    margin: 0.67em 0;
  }

  /**
   * Address styling not present in IE 8/9.
   */

  mark {
    background: #ff0;
    color: #000;
  }

  /**
   * Address inconsistent and variable font size in all browsers.
   */

  small {
    font-size: 80%;
  }

  /**
   * Prevent `sub` and `sup` affecting `line-height` in all browsers.
   */

  sub,
  sup {
    font-size: 75%;
    line-height: 0;
    position: relative;
    vertical-align: baseline;
  }

  sup {
    top: -0.5em;
  }

  sub {
    bottom: -0.25em;
  }

  /* Embedded content
     ========================================================================== */

  /**
   * Remove border when inside `a` element in IE 8/9/10.
   */

  img {
    border: 0;
  }

  /**
   * Correct overflow not hidden in IE 9/10/11.
   */

  svg:not(:root) {
    overflow: hidden;
  }

  /* Grouping content
     ========================================================================== */

  /**
   * Address margin not present in IE 8/9 and Safari.
   */

  figure {
    margin: 1em 40px;
  }

  /**
   * Address differences between Firefox and other browsers.
   */

  hr {
    -moz-box-sizing: content-box;
    box-sizing: content-box;
    height: 0;
  }

  /**
   * Contain overflow in all browsers.
   */

  pre {
    overflow: auto;
  }

  /**
   * Address odd `em`-unit font size rendering in all browsers.
   */

  code,
  kbd,
  pre,
  samp {
    font-family: monospace, monospace;
    font-size: 1em;
  }

  /* Forms
     ========================================================================== */

  /**
   * Known limitation: by default, Chrome and Safari on OS X allow very limited
   * styling of `select`, unless a `border` property is set.
   */

  /**
   * 1. Correct color not being inherited.
   *    Known issue: affects color of disabled elements.
   * 2. Correct font properties not being inherited.
   * 3. Address margins set differently in Firefox 4+, Safari, and Chrome.
   */

  button,
  input,
  optgroup,
  select,
  textarea {
    color: inherit; /* 1 */
    font: inherit; /* 2 */
    margin: 0; /* 3 */
  }

  /**
   * Address `overflow` set to `hidden` in IE 8/9/10/11.
   */

  button {
    overflow: visible;
  }

  /**
   * Address inconsistent `text-transform` inheritance for `button` and `select`.
   * All other form control elements do not inherit `text-transform` values.
   * Correct `button` style inheritance in Firefox, IE 8/9/10/11, and Opera.
   * Correct `select` style inheritance in Firefox.
   */

  button,
  select {
    text-transform: none;
  }

  /**
   * 1. Avoid the WebKit bug in Android 4.0.* where (2) destroys native `audio`
   *    and `video` controls.
   * 2. Correct inability to style clickable `input` types in iOS.
   * 3. Improve usability and consistency of cursor style between image-type
   *    `input` and others.
   */

  button,
  html input[type="button"], /* 1 */
  input[type="reset"],
  input[type="submit"] {
    -webkit-appearance: button; /* 2 */
    cursor: pointer; /* 3 */
  }

  /**
   * Re-set default cursor for disabled elements.
   */

  button[disabled],
  html input[disabled] {
    cursor: default;
  }

  /**
   * Remove inner padding and border in Firefox 4+.
   */

  button::-moz-focus-inner,
  input::-moz-focus-inner {
    border: 0;
    padding: 0;
  }

  /**
   * Address Firefox 4+ setting `line-height` on `input` using `!important` in
   * the UA stylesheet.
   */

  input {
    line-height: normal;
  }

  /**
   * It's recommended that you don't attempt to style these elements.
   * Firefox's implementation doesn't respect box-sizing, padding, or width.
   *
   * 1. Address box sizing set to `content-box` in IE 8/9/10.
   * 2. Remove excess padding in IE 8/9/10.
   */

  input[type="checkbox"],
  input[type="radio"] {
    box-sizing: border-box; /* 1 */
    padding: 0; /* 2 */
  }

  /**
   * Fix the cursor style for Chrome's increment/decrement buttons. For certain
   * `font-size` values of the `input`, it causes the cursor style of the
   * decrement button to change from `default` to `text`.
   */

  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    height: auto;
  }

  /**
   * 1. Address `appearance` set to `searchfield` in Safari and Chrome.
   * 2. Address `box-sizing` set to `border-box` in Safari and Chrome
   *    (include `-moz` to future-proof).
   */

  input[type="search"] {
    -webkit-appearance: textfield; /* 1 */
    -moz-box-sizing: content-box;
    -webkit-box-sizing: content-box; /* 2 */
    box-sizing: content-box;
  }

  /**
   * Remove inner padding and search cancel button in Safari and Chrome on OS X.
   * Safari (but not Chrome) clips the cancel button when the search input has
   * padding (and `textfield` appearance).
   */

  input[type="search"]::-webkit-search-cancel-button,
  input[type="search"]::-webkit-search-decoration {
    -webkit-appearance: none;
  }

  /**
   * Define consistent border, margin, and padding.
   */

  fieldset {
    border: 1px solid #c0c0c0;
    margin: 0 2px;
    padding: 0.35em 0.625em 0.75em;
  }

  /**
   * 1. Correct `color` not being inherited in IE 8/9/10/11.
   * 2. Remove padding so people aren't caught out if they zero out fieldsets.
   */

  legend {
    border: 0; /* 1 */
    padding: 0; /* 2 */
  }

  /**
   * Remove default vertical scrollbar in IE 8/9/10/11.
   */

  textarea {
    overflow: auto;
  }

  /**
   * Don't inherit the `font-weight` (applied by a rule above).
   * NOTE: the default cannot safely be changed in Chrome and Safari on OS X.
   */

  optgroup {
    font-weight: bold;
  }

  /* Tables
     ========================================================================== */

  /**
   * Remove most spacing between table cells.
   */

  table {
    border-collapse: collapse;
    border-spacing: 0;
  }

  td,
  th {
    padding: 0;
  }

```

修改application.css.scss
```scss
@important "normalize";

html, body {
	font-family: 'Raleway', sans-serif;
}

h1, h2, h3, h4, h5, h6 {
	font-weight: 500;
}

a {
	text-decoration: none;
	color: inherit;
}

#sidebar {
	width: 250px;
	position: fixed;
	left: 0;
	top: 0;
	height: 100%;
	background: #f5f7f9;
	padding: 7em 0 0 0;
	border-right: 1px solid #d6dce0;
	#logo {
		width: 40px;
		position: absolute;
		right: 3em;
		top: 3em;
	}
	ul {
		list-style: none;
		text-align: right;
		padding-right: 3em;
		.category {
			font-weight: 700;
			font-size: 0.7em;
			text-transform: uppercase;
			color: #33acb7;
		}
		li {
			padding: .5em 0;
			a {
				color: #9eafba;
				text-decoration: none;
				transition: all .4s ease;
				&:hover {
					color: #33acb7;
				}
			}
		}
		.active {
			a {
				color: #33acb7;
			}
		}
	}
	.sign_in {
		position: absolute;
		right: 3em;
		top: 80%;
		font-size: .8em;
		color: #9eafba;
	}
}

.button {
	outline: none;
	background: transparent;
	border: 1px solid #d6dce0;
	padding: .5em 1.5em;
	border-radius: 1.5em;
	&:hover {
		border: 1px solid #33acb7;
		color: #33acb7;
		a {
			color: #33acb7 !important;
		}
	}
}

.clearfix:after {
   content: ".";
   visibility: hidden;
   display: block;
   height: 0;
   clear: both;
}

#main_content {
	margin-left: 250px;
	#header {
		padding: 1em 3em;
		border-bottom: 1px solid #d6dce0;
		background: #f5f7f9;
		color: #9eafba;
		p {
			display: inline;
		}
		a {
			color: #9eafba;
			text-decoration: none;
		}
		.buttons {
			float: right;
			margin-top: -6px;
			.button {
				font-size: .8em;
				margin-left: .5em;
			}
		}
	}
	.post_wrapper {
		padding: 3em;
		border-bottom: 1px solid #d6dce0;
		.title {
			margin: 0;
			a {
				font-weight: 500;
				text-decoration: none;
				color: #2a2f35;
				font-size: 1.5em;
				&:hover {
					color: #33acb7;
				}
			}
		}
		.date_and_author {
			color: #9eafba;
			margin: .5em 0 0 0;
		}
	}
	#post_content {
		padding: 1em 3em;
		.title {
			font-weight: 500;
			text-decoration: none;
			color: #2a2f35;
			font-size: 2.5em;
			margin-bottom: 0;
		}
		.body {
			font-size: 1.1em;
			line-height: 1.75;
		}
		.date_and_author {
			color: #9eafba;
			margin: .5em 0 2em 0;
		}
		#comments {
			h2 {
				margin: 3em 0 1em 0;
				border-bottom: 1px solid #d6dce0;
				padding-bottom: 0.5em;
			}
			h3 {
				margin-top: 2em;
			}
			.comment {
				border-bottom: 1px solid #d6dce0;
				padding: 1.5em 2em;
				.clear_both {
					clear: both;
				}
				&:after {
					clear: both;
				}
				.comment_content {
					float: left;
					.comment_name {
						margin: 1em 0 0 0;
						font-size: 0.7em;
						text-transform: uppercase;
					}
					.comment_body {
						font-size: 1.2em;
						margin: 0.2em 0 0 0;
					}
					.comment_time {
						margin-top: 1.2em;
						font-size: .8em;
					}
				}
				.button {
					float: right;
				}
			}
			input[type="text"], textarea {
				width: 50%;
			}
		}
	}
	#page_wrapper {
		padding: 3em;
		#profile_image {
			width: 300px;
			float: left;
			margin-right: 2em;
			img {
				width: 100%;
				border-radius: 0.35em;
			}
		}
		#content {
			h1 {
				font-weight: 500;
			}
			p {
				font-size: 1.1em;
				line-height: 1.75;
			}
			a {
				color: #33acb7;
				font-weight: 700;
				text-decoration: none;
			}
		}
	}
	.links {
		margin: 2em 0;
	}
	input[type="text"], input[type="email"], input[type="password"], textarea {
		width: 90%;
		border: 1px solid #d6dce0;
		border-radius: .35em;
		margin-top: 10px;
		padding: .5em 1em;
		line-height: 1.75;
	}
	input[type="text"] {
		height: 35px;
	}
	textarea {
		min-height: 180px;
	}
	input[type="submit"] {
		outline: none;
		background: transparent;
		border: 1px solid #d6dce0;
		padding: .5em 1.5em;
		font-size: 1.1em;
		border-radius: 1.5em;
		margin-left: .5em;
		&:hover {
			border: 1px solid #33acb7;
			color: #33acb7;
		}
	}
}
```
打开finder,12in12/log2/app/assets/images
在网站http://www.flaticon.com/中找到一个自己喜欢的图标，下载后命名logo2.svg,然后放入上面文件中
或者你也可以这样做
`touch app/assets/images/logo2.svg`
修改 app/assets/images/logo2.svg
```svg
<?xml version="1.0" encoding="iso-8859-1"?>
<!-- Generator: Adobe Illustrator 19.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
	 viewBox="0 0 291.319 291.319" style="enable-background:new 0 0 291.319 291.319;" xml:space="preserve">
<g>
	<path style="fill:#F4B459;" d="M145.659,0c80.44,0,145.66,65.219,145.66,145.66S226.1,291.319,145.66,291.319S0,226.1,0,145.66
		S65.21,0,145.659,0z"/>
	<path style="fill:#FFFFFF;" d="M218.089,133.324l-1.238-2.531l-2.048-1.602c-2.686-2.094-16.268,0.146-19.928-3.177
		c-2.595-2.376-3.004-6.664-3.787-12.454c-1.457-11.252-2.385-11.835-4.142-15.631c-6.4-13.537-23.752-23.706-35.677-25.117h-32.3
		c-25.408,0-46.174,20.729-46.174,46.047v53.694c0,25.263,20.766,45.956,46.174,45.956h53.066c25.418,0,46.056-20.684,46.192-45.956
		l0.282-37.198C218.507,135.354,218.089,133.324,218.089,133.324z M118.576,109.299h26.874c5.116,0,9.286,4.088,9.286,9.067
		c0,4.962-4.16,9.086-9.286,9.086h-26.874c-5.125,0-9.286-4.124-9.286-9.086C109.29,113.387,113.451,109.299,118.576,109.299z
		 M172.907,182.065H118.54c-5.107,0-9.249-4.169-9.249-9.122c0-5.025,4.142-9.113,9.249-9.113h54.367
		c5.08,0,9.195,4.088,9.195,9.113C182.102,177.896,177.987,182.065,172.907,182.065z"/>
</g>
</svg>
```
刷新页面
修改 app/views/application.html.erb
```erb
<!DOCTYPE html>
<html>
  <head>
    <title>Blog2</title>
    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= stylesheet_link_tag    'application', 'http://fonts.goodleapis.com/css?family=Raleway:400,700' %>

    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
    <%= csrf_meta_tags %>
  </head>

  <body>
    <div id="sidebar">
      <div id="logo">
        <%= link_to root_path do %>
          <%= image_tag "logo2.svg" %>
        <% end %>
      </div>

      <ul>
        <li class="category">Website</li>
        <li><%= link_to "Blog", root_path %></li>
        <li><%= link_to "About" %></li>
      </ul>

      <ul>
        <li class="category">Social</li>
        <li><a href="#">Twitter</a></li>
        <li><a href="#">Instagram</a></li>
        <li><a href="https://github.com/liubingjei">Github</a></li>
        <li><a href="#">Email</a></li>
      </ul>

      <p class="sign_in">Admin Login</p>
    </div>

    <div id="main_content">
      <div id="header">
        <p>All Posts</p>

        <div class="buttons">
          <button class="button"><%= link_to "New Post", new_post_path %></button>
          <button class="button">Log Out</button>
        </div>
      </div>

      <% flash.each do |name, msg| %>
        <%= content_tag(:div, msg, class: "alert")%>
      <% end %>

      <%= yield %>
    </div>

  </body>
</html>
```
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
修改posts/edit.html.erb
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
`git status``
`git add .`
`git commit -am "Styling and structure"`

修改model/post.rb
```rb
class Post < ActiveRecord::Base
  validates :title, presence: true, length: { minimum: 5 }
  validates :body, presence: true
end
```
修改posts.controller.rb
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
修改posts/new.html.erb
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
`git commit -am "add validation to post form"`

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
修改文件 post.controller
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
`git commit -am "edit and delete posts"`

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

实作devise
`gem 'devise'`
`bundle`
`rails g devise:install`
修改文件 config/environments/development.rb
`config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }`
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
