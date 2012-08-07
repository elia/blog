# “Use Node.js FOR SPEED” — @RoflscaleTips

I obeyed, therefore I wrote an [`opal`](http://search.npmjs.org/#/opal) NPM package.

Now I can trash Rails and [`opal-rails`](http://elia.schito.me/post/24087273859/stop-writing-javascript-and-get-back-to-ruby) and start working on Node only!


```ruby
server = Server.new 3000
server.start do
  [200, {'Content-Type' => 'text/plain'}, ["Hello World!\n"]]
end
```

For it I had to write the `Server` class:

```ruby
class Server
  def initialize port
    @http = `require('http')`
    @port = port
  end

  def start &block
    %x{
      this.http.createServer(function(req, res) {
        var rackResponse = (block.call(block._s, req, res));
        res.end(rackResponse[2].join(' '));
      }).listen(this.port);
    }
  end
end
```

Put them in `app.opal` and then start the server:

```bash
$ npm install -g opal      # <<< install the NPM package first!
$ opal-node app
```

![The running app](http://f.cl.ly/items/352N3A2U0c013I391i0Q/Schermata%2006-2456088%20alle%206.18.09%20pm.png)


### YAY! now I can roflSCALE all of my RAils apps!!1
