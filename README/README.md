### 1. 간단한 게시판 만들기

> 1. Scaffold로 Post(title:string, content:text) 생성
> 2. Comment 모델 생성
> 3. 두개의 관계 설정 Post has many Comment, Comment belongs_to Post
> 4. Post의 Index의 뷰에서 제목(title) 옆에 각각의 Comment의 갯수 보여주기

### 2. 조인을 통해서 DB 성능 향상시키기

> 특정 포스트와 관계 되어있는 특정 모델에 해당되는 애를 조인하자
>
> - preload, includes, eager_load
>
> ```
> def index
>  @posts = Post.all.includes(:comments)
> end
>
> $ Post.includes(:tags).where(:tags => {content: 'faker'})
> ```

### 3. 마크다운 사용하기 !

> 1. 스크립트 추가하기
>
> ```
> <!-- application.html.erb -->
> <link rel="stylesheet" href="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.css">
> <script src="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.js"></script>
> ```
>
> 1. views/posts/_form.html.erb 수정하기
>
> ```
> <%= form_for(@post) do |f| %>
>   ...
> <% end %>
> <script>
> var simplemde = new SimpleMDE({ element: $("#MyID")[0] });
> </script>
> ```
>
> 1. GemFile 추가하기
>
> ```
> gem 'redcarpet'
> ```
>
> - bundle install
>
> ```
> $ bundle install
>
> ```
>
> 1. controllers/posts_controller.rb 수정하기
>
> ```
> def show
>  @markdown = Redcarpet::Markdown.new(Redcarpet::Render::HTML, extensions = {})
> end
> ```
>
> 1. views/show.html.erb 수정하기
>
> ```
> <p>
>  <strong>Content:</strong>
>  <%= @markdown.render(@post.content).html_safe %>
> </p>
> ```