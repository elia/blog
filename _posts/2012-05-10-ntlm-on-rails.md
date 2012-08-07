Just for information I got a decent setup in which the whole authentication is handled by IIS.
Adapting from [these instructions](http://blog.trekforth.com/2011/06/installing-spree-on-windows-2003-server.html) I got IIS to act as a reverse proxy for Rails server (thin, webrick, whatever…) on a *nix machine (a Mac with Pow! in development).

Here's the `iirf.ini` file that I used for development:

```apache
# NOTE: 
# This file should be placed in the IIS document root 
# for the application


# Put the following linw in windows etc/hosts file
# 
#   172.18.27.252 intranet.dev
# 

StatusInquiry ON
RewriteLogLevel 3
RewriteLog ..\..\TEMP\iirf
RewriteCond %{REQUEST_FILENAME} -f
RewriteRule ^.*$ - [L]
ProxyPass ^/(.*)$ http://intranet.dev:80/$1
ProxyPassReverse / http://intranet.dev/

# Disable HTTPS
# 
#   RewriteCond %{HTTPS} on
#   RewriteHeader X_FORWARDED_PROTO ^$ https
# 
```

With this setup you can rely on the fact that the authentication is performed by IIS and you only get authenticated request containing a Type-1 ntlm message (or Type-3, I'm not completely sure about this). I also removed `rack-ntlm` and kept only `net-ntlm` and used it to extract the username this way:

```ruby
require 'kconv'
require 'net/ntlm'

if /^(NTLM|Negotiate) (.+)/ =~ env["HTTP_AUTHORIZATION"]
  encoded_message = $2
  message = Net::NTLM::Message.decode64(encoded_message)
  user = Net::NTLM::decode_utf16le(message.user)
end
```


