---
id: 5895f70df9fc0f352b528e68
title: 身份验证策略
challengeType: 2
forumTopicId: 301547
dashedName: authentication-strategies
---

# --description--

策略是认证用户的一种方式。 如果你让用户在注册时填写了用户信息，那你就可以基于这些信息进行验证。或者也可以引入第三方登录，如 Google 或者 Github。 在这个项目中，我们将使用 Passport 中间件。 Passport 提供了一套全面的策略，支持使用用户名和密码、GitHub、谷歌等进行认证。

`passport-local@~1.0.0` has already been added as a dependency, so add it to your server as follows: `const LocalStrategy = require('passport-local');`

然后，需要让 passport **使用**一个实例化的 LocalStrategy 对象，这个对象的一些设置已完成。 请注意，接下来的所有代码都应写在连接数据库的回调中，因为它们的执行都依赖数据库。

```js
passport.use(new LocalStrategy(
  function(username, password, done) {
    myDataBase.findOne({ username: username }, function (err, user) {
      console.log('User '+ username +' attempted to log in.');
      if (err) { return done(err); }
      if (!user) { return done(null, false); }
      if (password !== user.password) { return done(null, false); }
      return done(null, user);
    });
  }
));
```

这就是我们的用户验证逻辑： 首先根据用户输入的用户名在数据库中寻找用户；然后检查密码是否匹配，最后如果没有发生错误（比如密码错误），那么就会返回 `user` 对象并通过验证。

很多策略的配置都不同，但是，一般来说，根据策略仓库中的 README 来进行配置就足够了。 一个很好的例子是 GitHub 策略。在该策略中，我们不需要写用户名或密码的相关验证逻辑，因为用户将被引导到 GitHub 页面进行验证。 只要用户登录并同意，GitHub 就会返回用户的个人信息供我们使用。

在下一个挑战中，我们会基于表单数据调用上面写好的验证策略。

完成上述要求后，请提交你的页面链接。 如果你在运行时遇到错误，可以点击此处查看<a href="https://gist.github.com/camperbot/53b495c02b92adeee0aa1bd3f3be8a4b" target="_blank" rel="noopener noreferrer nofollow">已执行项目的当前进度</a>。

# --hints--

需要使用 passport-local 作为依赖。

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/_api/package.json').then(
    (data) => {
      var packJson = JSON.parse(data);
      assert.property(
        packJson.dependencies,
        'passport-local',
        'Your project should list "passport-local " as a dependency'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

应该正确地引入和配置 passport-local。

```js
(getUserInput) =>
  $.get(getUserInput('url') + '/_api/server.js').then(
    (data) => {
      assert.match(
        data,
        /require.*("|')passport-local("|')/gi,
        'You should have required passport-local'
      );
      assert.match(
        data,
        /new LocalStrategy/gi,
        'You should have told passport to use a new strategy'
      );
      assert.match(
        data,
        /findOne/gi,
        'Your new local strategy should use the findOne query to find a username based on the inputs'
      );
    },
    (xhr) => {
      throw new Error(xhr.statusText);
    }
  );
```

# --solutions--

```js
/**
  Backend challenges don't need solutions, 
  because they would need to be tested against a full working project. 
  Please check our contributing guidelines to learn more.
*/
```
