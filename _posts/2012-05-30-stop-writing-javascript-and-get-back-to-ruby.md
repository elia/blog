# Stop writing JavaScript and get back to Ruby!

Remeber that feeling that you had after having tried [CoffeeScript](http://coffeescript.org) getting back to a 
project with plain [JavaScript](http://developer.yahoo.com/yui/theater/video.php?v=eich-yuiconf2009-harmony) and felt so constricted?

Well, after re-writing a couple of coffee classes with [OpalRuby](http://opalrb.org) I felt exactly that way, 
and with 14.9KB of footprint (min+gz, jQuery is 32KB) is a complete no brainer.

So let's gobble up our first chunk of code, that will salute from the browser console:

```ruby
puts 'hi buddies!'
```

You wonder how that gets translated?

Fear that it will look like this?

```js
'egin',cb='bootstrap',u='clear.cache.gif',z='content',bc='end',lb='gecko',mb='g
cko1_8',yb='gwt.hybrid',xb='gwt/standard/standard.css',E='gwt:onLoadErrorFn',B=
'gwt:onPropertyErrorFn',y='gwt:property',Db='head',qb='hosted.html?hello',Cb='h
ref',kb='ie6',ab='iframe',t='img',bb="javascript:''",zb='link',pb='loadExternal
Refs',v='meta',eb='moduleRequested',ac='moduleStartup',jb='msie',w='name',gb='o
pera',db='position:absolute;width:0;height:0;border:none',Ab='rel',ib='safari',
rb='selectingPermutation',x='startup',m='hello',Bb='stylesheet',ob='unknown',fb
='user.agent',hb='webkit';var fc=window,k=document,ec=fc.__gwtStatsEvent?functi
on(a){return fc.__gwtStatsEvent(a)}:null,zc,pc,kc,jc=l,sc={},Cc=[],yc=[],ic=[],
vc,xc;ec&&ec({moduleName:m,subSystem:x,evtGroup:cb,millis:(new Date()).getTime(
),type:nb});if(!fc.__gwt_stylesLoaded){fc.__gwt_stylesLoaded={}}if(!fc.__gwt_sc
riptsLoaded){fc.__gwt_scriptsLoaded={}}function oc(){var b=false;try{b=fc.exter
```

Nope! [it's Chuck Testa](http://youtu.be/LJP1DphOWPs)

```js
(function() {
  return this.$puts("hi buddies!")
}).call(Opal.top);
```

**But it will be obviously a mess to integrate it with existing javascript!!1**

Let's how to add `#next` and `#prev` to the `Element` class:

```ruby
class Element
  def next(selector = '')
    element = nil
    `element = jQuery(this.el).next(selector)[0] || nil;`
    Element.new element if element
  end
end
```


**But I'm on Rails!**

Here [you served](http://elia.github.com/opal-rails/):

```ruby
gem 'opal-rails'
```

Which il provide you a requirable libraries for `application.js`

```js
//= require opal
//= require rquery
```

and the `.opal` extension for your files

```ruby
# app/assets/javascripts/hi-world.js.opal

puts "G'day world!"
```



A template handler:

```ruby
# app/views/posts/show.js.opal.erb

Element.id('<%= dom_id @post %>').show
```


and a [Haml](http://haml-lang.com) filter!

```haml
-# app/views/posts/show.html.haml

%article= post.body

%a#show-comments Display Comments!

.comments(style="display:none;")
  - post.comments.each do |comment|
    .comment= comment.body

:opal
  Document.ready? do
    Element.id('show-comments').on :click do
      Element.find('.comments').first.show
      false # aka preventDefault
    end
  end
```


