## Cheat Sheet
	- https://devhints.io/phoenix
	- > `Phoenix` is a web development framework written in [[Elixir]] which implements the server-side ((64d2313f-9a80-46ac-9609-c3de14989a40)) pattern
- ## Prerequisites
	- [[Elixir]]
		- ```bash
		  # On MacOs
		  brew install elixir
		  ```
	- [[Hex]]
		- ```bash
		  mix local.hex
		  ```
	- [[Erlang]]
		- is generally automatically installed when installing [[Elixir]]. If need to install it manually :
			- [Install erlang](https://elixir-lang.org/install.html#installing-erlang)
	- A Database (not mandatory, it depends on what the application will be for)
- Once the prerequisites are installed, we install the [[Phoenix]] application generator
	- ```bash 
	  mix archive.install hex phx_new
	  ```
- ----
- ## Up and Running
	- ```bash 
	  # To boostrap the Phoenix application
	  mix phx.new *app_name*
	  
	  # If no need to store data
	  mix phx.new *app_name* --no-ecto
	  
	  cd *app_name*
	  
	  # If using database
	  mix ecto.create
	  
	  # If not, run the server
	  mix phx.server
	  
	  # Run the server with iex
	  iex -S mix phx.server
	  ```
- ---
- ## Directory structure
	- Assuming the application name is :   ***hello***
	- ```bash 
	  ├── _build
	  ├── assets
	  ├── config
	  ├── deps
	  ├── lib
	  │   ├── hello
	  │   ├── hello.ex
	  │   ├── hello_web
	  │   └── hello_web.ex
	  ├── priv
	  └── test
	  ```
		- `_build` - created by the `mix` command line tool that ships as part of [[Elixir]] that holds all compilation artifacts. `Mix` is used to compile code, create databases, run servers, and more.
		- `assets` - keeps source code for front-end ( JS, CSS )
		- `config` - holds the project configuration
			- `config/config.exs` - entry point for configuration, at the end of the [[file]], it imports the environment specifig configuration, found in `config/dev.exs`, `config/test.exs` and `config/prod.exs`
			- `config/runtime.exs` - is executed and it is the best place to read secrets and other dynamic configuration
		- `deps` - contains all `Mix` dependencies
		- `lib` - holds the application source code. Is broken into two subdirectories
			- `lib/hello` - host all business logic and business domain. It typically interacts with the database. It is the ((64d2302b-7a31-499f-8834-5cf5ccf0792c)) on the [[MVC]] architecture
			- `lib/hello_web` - responsible for exposing the business domain to the world, through a web application. It hols the View and the Controller from [[MVC]]
		- `priv` - keeps all the ressources that are necessary in production but are not directly part of the source code like `databases scripts`, `translation files`, `images`, and more. Generated assets, created from files in the `assets` directory, are placed in `priv/static/assets` by default
		- `test` - directory with all of application tests. It often mirrors the same structure found in `lib`
	-
	- ### `lib_hello_web`
		- ```bash 
		  lib/hello_web
		  ├── controllers
		  │   ├── page_controller.ex
		  │   ├── page_html.ex
		  │   ├── error_html.ex
		  │   ├── error_json.ex
		  │   └── page_html
		  │       └── home.html.heex
		  ├── components
		  │   ├── core_components.ex
		  │   ├── layouts.ex
		  │   └── layouts
		  │       ├── app.html.heex
		  │       └── root.html.heex
		  ├── endpoint.ex
		  ├── gettext.ex
		  ├── router.ex
		  └── telemetry.ex
		  ```
- ---
- ## Adding a new page
	- To add a new page,  the first task is to add a new `route`
		- Routes are defined in `lib/hello_web/router.ex`
			- ```elixir 
			  scope "/", HelloWeb do
			    pipe_through :browser
			  
			    get "/", PageController, :home
			    
			    # Add a new route here !
			    get "/hello", HelloController, :index
			  end
			  ```
			- Our route specifies that we need a `HelloWeb.HelloController` module with an `index/2` function.
		- Next step, is to add a new `controller`, to do that, create `lib/hello_web/controllers/hello_controller.ex`
			- ```elixir 
			  defmodule HelloWeb.HelloController do
			    use HelloWeb, :controller
			  
			    def index(conn, _params) do
			      render(conn, :index)
			    end
			  end
			  ```
			- > All controller actions take two arguments
		- And now, we need to create the new `view`, to do that, create `lib/hello_web/controllers/hello_html.ex`
			- ```elixir 
			  defmodule HelloWeb.HelloHTML do
			    use HelloWeb, :html
			  end
			  ```
		- Now, if we want to add a template to this view, we need to modify the `lib/hello_web/controllers/hello_html.ex` [[file]]
			-
			- ```elixir 
			  defmodule HelloWeb.HelloHTML do
			    use HelloWeb, :html
			  
			    # Template to the view as function components
			    def index(assigns) do
			      ~H"""
			      Hello!
			      """
			    end
			  
			    # Template to the view from a file
			    embed_templates "hello_html/*"
			  end
			  ```
			- In case we choose a template from a file, we need to create the file where the template will be called from. To to that, create directory `lib/hello_web/controllers/hello_html`.
			- Note the controller name (`HelloController`), the view name (`HelloHTML`), and the template directory (`hello_html`) all follow the same naming convention and are named after each other. They are also collocated together in the directory tree:
				- ```elixir 
				  lib/hello_web
				  ├── controllers
				  │   ├── hello_controller.ex
				  │   ├── hello_html.ex
				  │   ├── hello_html
				  |       ├── index.html.heex
				  ```
			- > A template file has the following structure: `NAME.FORMAT.TEMPLATING_LANGUAGE`. In our case, let's create an `index.html.heex` file at `lib/hello_web/controllers/hello_html/index.html.heex`:
				- ```heex 
				  <section>
				    <h2>Hello World, from Phoenix!</h2>
				  </section>
				  ```
- ---
- ### Plug
	- > [Plug](https://github.com/elixir-lang/plug) is a specification for composable modules in between web applications. It is also an abstraction layer for connection adapters of different web servers. The basic idea of Plug is to unify the concept of a "connection" that we operate on. The Plug specification comes in two flavors: *function plugs* and *module plugs*.
	- #### Function plugs
		- In order to act as a plug, a function needs to:
			- 1. accept a connection struct `%Plug.Conn{}` as its first argument, and connection options as its second one
			  2. return a connection struct
			- Example :
				- ```elixir
				  def introspect(conn, _opts) do
				    IO.puts """
				    Verb: #{inspect(conn.method)}
				    Host: #{inspect(conn.host)}
				    Headers: #{inspect(conn.req_headers)}
				    """
				  
				    conn
				  end
				  ```
			- This function does the following :
				- 1. It receives a connection and options (that we do not use)
				  2. It prints some connection information to the terminal
				  3. It returns the connection
			- To see this function in action, we need to add in `lib/hello_web/endpoint.ex` :
				- ```elixir 
				  defmodule HelloWeb.Endpoint do
				    ...
				  
				    plug :introspect
				    plug HelloWeb.Router
				  
				    def introspect(conn, _opts) do
				      IO.puts """
				      Verb: #{inspect(conn.method)}
				      Host: #{inspect(conn.host)}
				      Headers: #{inspect(conn.req_headers)}
				      """
				  
				      conn
				    end
				  end
				  ```
				- When fetching `http://localhost:4000` we can see the informations on the shell
			- To learn about all fields available in the connection and all of the functionality associated to it, see the [documentation for `Plug.Conn`](https://hexdocs.pm/plug/Plug.Conn.html)
	- #### Module plugs
		- > Module plugs let us define a connection transformation in a module. The module only needs to implement two functions :
			- 1. `init/1` which initializes any arguments or options to be passed to [`call/2`](https://hexdocs.pm/plug/1.14.0/Plug.html#c:call/2)
			  2. `call/2` which carries out the connection transformation. [`call/2`](https://hexdocs.pm/plug/1.14.0/Plug.html#c:call/2) is just a function plug that we saw earlier
-
- ## [[Commands]]
	- `mix ecto.gen.migration create_users`
		- Generates a migration to be modified
	- `mix phx.gen.html Accounts User users name:string age:integer `
		- Generates controller, HTML views, a migration, and context for an HTML resource
			- - 1st argument : context module
			- - 2nd argument : schema module
			- - 3th argument : schema table name (plural of schema module)
			- - tables names
	- `mix docs --format html --open`
		- Open project docs
- -----
- ## Phoenix-MVC
  id:: 64d2313f-9a80-46ac-9609-c3de14989a40
	- `Model` -> Schema + Context
	- `View` -> View + Template
	- `Controller` -> Router + Controller
	-
	- `Migrations` -> defines a table structure for a database, here you can create/modify/delete a table, add fields and unique indexes
	- ### Model
	  id:: 64d2302b-7a31-499f-8834-5cf5ccf0792c
		- `Schema` -> create a data structure related to a table (defined on migrations), and define functions for validations using a changeset
		- `Context` -> a context uses a schema and a Repo to define functions and pipelines to transform and persist data
	- ### View
		- `View` -> defines specific functions for parsing and filtering data to be used from a Template, and will render a specified Template
		- `Template` -> the actual HTML code and front-end logic
	- ### Controller
		- `Router` -> receive request and select correct controller
		- `Controller` -> the controller will access the context asking for functions and pipelines, and will ask view to render the correct related page
- ----
- ## call, cast or info ?
	- `handle_call/3` must be used for synchronous requests. This should be the default choice as waiting for the server reply is a useful back-pressure mechanism.
	  logseq.order-list-type:: number
	- `handle_cast/2` must be used for asynchronous requests, when you don't care about a reply. A cast does not guarantee the server has received the message and, for this reason, should be used sparingly.
	  logseq.order-list-type:: number
	- `handle_info/2` must be used for all other messages a server may receive that are not sent via [`GenServer.call/2`](https://hexdocs.pm/elixir/1.16/GenServer.html#call/2) or [`GenServer.cast/2`](https://hexdocs.pm/elixir/1.16/GenServer.html#cast/2), including regular messages sent with [`send/2`](https://hexdocs.pm/elixir/1.16/Kernel.html#send/2). The monitoring `:DOWN` messages are an example of this.
	  logseq.order-list-type:: number
	- #### Cheat sheet
		- https://elixir-lang.org/downloads/cheatsheets/gen-server.pdf
	-
- -----
- ## Many to many associations
	- [Undersand many to many associations](https://dev.to/ricardoruwer/many-to-many-associations-in-elixir-and-phoenix-21pm)
	- [Examples from the official documentation](https://hexdocs.pm/ecto/associations.html#many-to-many)
- -----
- # Tips
- `__MODULE__`
	- returns the module name for more flexible code