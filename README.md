Phoenix is a Rails like MVC framework for the Erlang based Elixir functional programming language. While Phoenix aims for very high developer productiveness along with application performance ([why is this uncommon]), it is based on a middleware called Plug, which is somewhat analagous to Ruby's Rack middleware:

```
####################
#     Phoenix      #
####################
#     Plug         #
####################
```

I think it's worth spending some time understanding Plug in the same way understanding Rack can help illuminate what Rails does under the hood. So what is Plug? Well, the Plug [guides](https://github.com/elixir-lang/plug) define it as follows:

```
1. A specification for composable modules in between web applications
2. Connection adapters for different web servers in the Erlang VM
```

I think it's worth taking a moment to break down the above statements. Plug allows us to do two things:

### (1) A Specification for Composable Modules

Plug allows us to create distinct modules that we can use (which is why they are called plugs) that expect something and return something else ([link]https://github.com/elixir-lang/plug/blob/master/lib/plug.ex#L3-L21 to plug specs). There are two types of plugs: (a) function plugs; and (b) module plugs.

##### (a) Function plugs

The specs for a function plug:

```elixir
(Plug.Conn.t, Plug.opts) :: Plug.Conn.t
```

As we can see above, a function plug takes in a connection `Plug.Conn.t` and some options `Plug.opts` and returns a connection `Plug.Conn.t`. The options that the function plug can receive are a set of tuple, atom, integer, or float.

The following is an example of a function plug:

```elixir
def json_header_plug(conn, opts) do
  conn |> put_resp_content_type("application/json")
end
```

##### (b) Module plugs

A module plug must export two functions, a `call/2` function that is like the function plug above and a `init/1` plug that takes in a set of options to initialize.

THe following is an example of a module plug:

```elixir
defmodule JSONHeaderPlug do
  def init(opts) do
    opts
  end
  def call(conn, _opts) do
    conn |> put_resp_content_type("application/json")
  end
end
```

### (2) Connection Adapters for Different Web Servers in the Erlang VM

Plug's connection adapters allow it to interface with HTTP servers like [Cowboy](https://github.com/ninenines/cowboy).

## Walk through Hello World

## Walk through something a bit more interesting

##### More reading
* Plug's [plug](https://github.com/elixir-lang/plug/blob/master/lib/plug.ex#L3-L21) specifications
* [Web Development Using Elixir](https://leanpub.com/web-development-using-elixir/read) by Shane Logsdon
