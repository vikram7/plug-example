#### Phoenix

* An elixir based web development framework
* MVC Design Pattern
* Similar to Ruby on Rails
* Aims for high productivity and application performance; also handles realtime events well with channels


####################
#     Phoenix      #
####################
#     Plug         #
####################


Plug is:
```
A specification for composable modules in between web applications
Connection adapters for different web servers in the Erlang VM
```

A simple `Hello world` app written in Plug:

```elixir
defmodule MyPlug do
  import Plug.Conn

  def init(options) do
    # initialize options

    options
  end

  def call(conn, _opts) do
    conn
    |> put_resp_content_type("text/plain")
    |> send_resp(200, "Hello world")
  end
end
```

Here we define a `plug`, which is two things: (1) a function that receives a connection and a set of options as arguments and returns a connection; or (2) a module that provides an `init/1` function to initialize options and implement the `call/2` function (receiving the connection and options) returning the connection.

It's important to keep in mind that a connection is immutable. Each manipulation returns a new copy of the connection. What is a connection? It is a *direct interface to the underlying server*.

## The Plug Router

```elixir
defmodule AppRouter do
  use Plug.Router

  plug :match
  plug :dispatch

  get "/hello" do
    send_resp(conn, 200, "world")
  end

  forward "/users", to: UsersRouter

  match _ do
    send_resp(conn, 404, "oops")
  end
end
```

The Plug router is both a plug and contains its own plug pipeline. In the above example, when the router is invoked, it invokes the `:match` plug (a `match/2` function) and then calls the `:dispatch` plug which executes the matched code.


`plug Plug.Logger` allows logging on the router.

