Caddy-authz
======

Caddy-authz is an authorization middleware for [Caddy](https://github.com/mholt/caddy), it's based on [https://github.com/casbin/casbin](https://github.com/casbin/casbin). It includes basicauth checks agains a bcrypt password file.

## Installation

    go get github.com/casbin/caddy-authz

## Caddyfile syntax

```
http://localhost:80 {
    authz "/folder/to/caddy_binary/authz_model.conf" /folder/to/caddy_binary/authz_policy.csv" AuthRealm authfile
    ...
}
```

The ``authz`` directive specifies the path to Casbin model file (.conf) and Casbin policy file (.csv). The Casbin model file describes access control models like ACL, RBAC, ABAC, etc. The Casbin policy file describes the authorization policy rules. For how to write these files, please refer to: https://github.com/casbin/casbin#get-started

## A working example

1. ``cd`` into the folder of ``caddy`` binary.

2. Put your Casbin model file [authz_model.conf](https://github.com/casbin/caddy-authz/blob/master/authz_model.conf) and Casbin policy file [authz_policy.csv](https://github.com/casbin/caddy-authz/blob/master/authz_policy.csv) into this folder.

3. Add ``authz`` directive to your Caddyfile like:

```
http://localhost:80 {
    authz "authz_model.conf" "authz_policy.csv" MyRealm bcrypt.pass
    root "/my-website.net"
}
```

4. Run ``caddy`` and enjoy.

Note: This plugin only supports HTTP basic authentication to get the logged-in user name, if you use other kinds of authentication like OAuth, LDAP, etc, you may need to customize this plugin.

## How to control the access

The authorization determines a request based on ``{subject, object, action}``, which means what ``subject`` can perform what ``action`` on what ``object``. In this plugin, the meanings are:

1. ``subject``: the logged-on user name
2. ``object``: the URL path for the web resource like "dataset1/item1"
3. ``action``: HTTP method like GET, POST, PUT, DELETE, or the high-level actions you defined like "read-file", "write-blog"


For how to write authorization policy and other details, please refer to [the Casbin's documentation](https://github.com/casbin/casbin).

## Getting Help

- [Casbin](https://github.com/casbin/casbin)

## License

This project is under Apache 2.0 License. See the [LICENSE](LICENSE) file for the full license text.
