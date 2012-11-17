# eredis_pool

eredis_pool is Pool of Redis clients, using eredis and poolboy.

eredis:
[[https://github.com/wooga/eredis]]

poolboy:
[[https://github.com/devinus/poolboy]]

## Setup

- git clone git://github.com/hiroeorz/eredis_pool.git
- cd eredis_pool
- make get-deps
- make

## Testing

make check

## Settings

edit src/eredis_pool.app.src

```erlang
 {application, eredis_pool,
  [
   {description, ""},
   {vsn, "1"},
   {registered, []},
   {applications, [
                   kernel,
                   stdlib
                  ]},
   {mod, { eredis_pool_app, []}},
   {env, [
           {pools, [
                    {default, [
                               {size, 10},
                               {max_overflow, 20}
                              ]}
                   ]}
         ]}
  ]}.
```

add new pools.

```erlang
  {env, [
          {pools, [
                   {default, [
                              {size, 10},
                              {max_overflow, 20}
                             ]},
                   {pool1, [
                              {size, 30},
                              {max_overflow, 20},
                              {host, "127.0.0.1"},
                              {port, 6379}
                             ]},
                   {pool2, [
                              {size, 20},
                              {max_overflow, 20},
                              {host, "127.0.0.1"},
                              {port, 6379},
                              {database, "user_db"},
                              {password, "abc"},
                              {reconnect_sleep, 100}
                             ]}
                  ]}
        ]}
```


## Examples

application start.
```erlang
 eredis_pool:start().
 ok
```

key-value set and get
```erlang
 eredis_pool:q({global, dbsrv}, ["SET", "foo", "bar"]).
 {ok,<<"OK">>}
 
 eredis_pool:q({global, dbsrv}, ["GET", "foo"]).       
 {ok,<<"bar">>}
```
 
create new pool with default settings.
```erlang
 eredis_pool:create_pool(pool1, 10).
 {ok,<0.64.0>}
```

and omissible argments(host, port, database, password reconnect_sleep).
```erlang
 eredis_pool:create_pool(pool1, 10, "127.0.0.1", 6379, "user_db", "abc", 100).
 {ok,<0.64.0>}
```

using new pool
```erlang
 eredis_pool:q(pool1, ["GET", "foo"]).
 {ok,<<"bar">>}
```

delete pool
```erlang
 eredis_pool:delete_pool(pool1).    
 ok
```

Other commands are here.
http://redis.io/commands
