ERb標籤

	<%= %> 會輸出到html
	<% %> 不會輸出到html

##常用helper	

###link_to
會產生a tag

	<%= link_to "Profile", profile_path(@profile) %>
	# => <a href="/profiles/1">Profile</a>

	<%= link_to "Profile", @profile%>
	# => <a href="/profiles/1">Profile</a>

You can use a block as well if your link target is hard to fit into the name parameter. ERB example:

	<%= link_to(@profile) do %>
 	 <strong><%= @profile.name %></strong> -- <span>Check it out!</span>
	<% end %>
	# => <a href="/profiles/1">
	       <strong>David</strong> -- <span>Check it out!</span>
	     </a>

Classes and ids for CSS are easy to produce:

	link_to "Articles", articles_path, id: "news", class: "article"
	# => <a href="/articles" class="article" id="news">Articles</a>

You can also use custom data attributes using the :data option:

	link_to "Visit Other Site", "http://www.rubyonrails.org/", data: { confirm: "Are you sure?" }
	# => <a href="http://www.rubyonrails.org/" data-confirm="Are you sure?">Visit Other Site</a>	

###button_to
	<%= button_to "New", action: "new" %>
	# => "<form method="post" action="/controller/new" class="button_to">
	#      <div><input value="New" type="submit" /></div>
	#    </form>"

	<%= button_to "New", new_articles_path %>
	# => "<form method="post" action="/articles/new" class="button_to">
	#      <div><input value="New" type="submit" /></div>
	#    </form>"

	<%= button_to [:make_happy, @user] do %>
	  Make happy <strong><%= @user.name %></strong>
	<% end %>
	# => "<form method="post" action="/users/1/make_happy" class="button_to">
	#      <div>
	#        <button type="submit">
	#          Make happy <strong><%= @user.name %></strong>
	#        </button>
	#      </div>
	#    </form>"

	<%= button_to "New", { action: "new" }, form_class: "new-thing" %>
	# => "<form method="post" action="/controller/new" class="new-thing">
	#      <div><input value="New" type="submit" /></div>
	#    </form>"

	<%= button_to "Create", { action: "create" }, remote: true, form: { "data-type" => "json" } %>
	# => "<form method="post" action="/images/create" class="button_to" data-remote="true" data-type="json">
	#      <div>
	#        <input value="Create" type="submit" />
	#        <input name="authenticity_token" type="hidden" value="10f2163b45388899ad4d5ae948988266befcb6c3d1b2451cf657a0c293d605a6"/>
	#      </div>
	#    </form>"

	<%= button_to "Delete Image", { action: "delete", id: @image.id },
	                                method: :delete, data: { confirm: "Are you sure?" } %>
	# => "<form method="post" action="/images/delete/1" class="button_to">
	#      <div>
	#        <input type="hidden" name="_method" value="delete" />
	#        <input data-confirm='Are you sure?' value="Delete Image" type="submit" />
	#        <input name="authenticity_token" type="hidden" value="10f2163b45388899ad4d5ae948988266befcb6c3d1b2451cf657a0c293d605a6"/>
	#      </div>
	#    </form>"

	<%= button_to('Destroy', 'http://www.example.com',
	          method: "delete", remote: true, data: { confirm: 'Are you sure?', disable_with: 'loading...' }) %>
	# => "<form class='button_to' method='post' action='http://www.example.com' data-remote='true'>
	#       <div>
	#         <input name='_method' value='delete' type='hidden' />
	#         <input value='Destroy' type='submit' data-disable-with='loading...' data-confirm='Are you sure?' />
	#         <input name="authenticity_token" type="hidden" value="10f2163b45388899ad4d5ae948988266befcb6c3d1b2451cf657a0c293d605a6"/>
	#       </div>
	#     </form>"

###表單輔助方法

對網頁應用程式來說，表單是非常重要的用戶輸入介面。Rails在這方面也提供了很多好用的Helper方法。基本上，Rails處理表單分成兩種類型：

一種是對應到Model物件的新增、修改，我們會使用form_for這個Helper。它的好處在於透過傳入Model物件，可以在修改的時候自動幫你將預設值帶入。例如我們已經在Part1使用過的event表單：

	<%= form_for @event do |f| %>
	    <%= f.text_field :name %>
	    <%= f.submit %>
	<% end %>
另一種是就是沒有對應Model的表單，我們使用form_tag這個方法。例如：

	<%= form_tag "/search" do %>
	    <%= text_field_tag :keyword %>
	    <%= submit_tag %>
	<% end %>

和form_tag有些類似，但是其中不需要傳Block變數f，其中的欄位Helper需要多加_tag結尾。不像form_for的欄位名稱一定要是Model的屬性之一，在form_tag之中的欄位名稱則完全不受限。

幾個常用的表單欄位輔助方法：

* text_field
* text_area
* radio_button
* check_box
* select
* select_date, select_datetime
* submit


###局部樣板Partials

局部樣板可以將Template中重複的程式碼抽出來，例如我們在Part1中示範過的新增和編輯的表單。Partial Template的命名慣例是底線開頭，但是呼叫時不需加上底線，例如：

	<%= render :partial => "common/nav" %>
這樣便會使用app/views/common/_nav.html.erb這個樣板。如果使用Partial的樣板和Partial所在的目錄相同，可以省略第一段的common路徑。

在Partial樣板中是可以直接使用實例變數的(也就是@開頭的變數)。不過好的實務作法是透過:locals明確傳遞區域變數，這樣程式會比較清楚，Partial樣板也比較容易被重複使用：

	<%= render :partial => "common/nav", :locals => { :a => 1, :b => 2 } %>
這樣在partial樣板中，就可以存取到區域變數a和b。




###reference
[ihower-actionview-helpers](http://ihower.tw/rails3/actionview-helpers.html)

[api action view helpers](http://api.rubyonrails.org/classes/ActionView/Helpers.html)