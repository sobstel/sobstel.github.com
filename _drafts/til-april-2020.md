---
title: "TIL: April 2020"
---

- JS: `typeof null` => `object` (why: <https://2ality.com/2013/10/typeof-null.html>)

- Deploying Rails with webpack to Heroku (from [Rails 5.1 loves Javascript](https://medium.com/@hpux/rails-5-1-loves-javascript-a1d84d5318b)):

```ruby
heroku create
heroku buildpacks:add --index 1 heroku/nodejs
heroku buildpacks:add --index 2 heroku/ruby
git push heroku master
```

En passant
https://en.wikipedia.org/wiki/En_passant
