# redis-auth-server

This is a small server for use with Nginx's [auth_request module](http://nginx.org/en/docs/http/ngx_http_auth_request_module.html).

It's meant for a quick and simple auto-auth - basically, the first login will
create and save a user in redis. Subsequent logins will actually auth against
the stored password hash.

It requires Nginx compiled with Lua support, such as found in OpenResty. It
also requires the following lua modules:

* luaposix
* etlua
* luafilesystem

## install + use

Just git clone this repository somewhere.

From there, you can install the needed lua modules however you regularly do.

If you install them into a folder named `lua_modules`, then the script
`bin/redis-auth-server` will use (and *only* use) modules found under that
folder. For example, after cloning you could run:

```bash
luarocks install --tree lua_modules luaposix
luarocks install --tree lua_modules etlua
luarocks install --tree lua_modules luafilesystem
```

Make a copy of `etc/config.lua.example` to `etc/config.lua` and edit
as-needed, then run `bin/redis-auth-server`.

By default, all temp files, compiled config files, etc are placed at
`$HOME/.redis-auth-server` - this can be changed by setting the `work_dir`
variable in `etc/config.lua`

To run as a service, there's an example systemd unit file at
`misc/redis-auth-server.service`:

```bash
sudo cp misc/redis-auth-server.service /etc/systemd/system/redis-auth-server.service
# edit /etc/systemd/system/redis-auth-server.service as needed
sudo systemctl daemon-reload
sudo systemctl enable redis-auth-server.service
sudo systemctl start redis-auth-server.service
```

## License

Released under an MIT-style license. See the file `LICENSE` for details.

The file `resty/redis.lua` also has an MIT-style license, see the file
`LICENSE-lua-resty-redis` for details.
