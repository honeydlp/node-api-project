# 项目布局
├── InitData                        初始化数据
├── config                          运行配置
│   ├── default.js                  默认配置
│   └── development.js              开发环境
├── controller                      处理中心，负责路由及数据库的具体操作
├── logs                            日志文件
├── middlewares                     中间价
│   ├── check.js                    权限验证    
│   └── statistic.js                API数据统计
├── models                          模型(数据库)
├── mongodb                         连接数据库
│   └── db.js
├── prototype                       基础功能Class
├── public                          静态资源目录
├── routes                          路由配置
├── views   
├── .babelrc 
├── .gitignore
├── API.md                          接口文档
├── app.js                          基础配置
├── COPYING                         GPL协议
├── index.js                        入口文件
├── package.json
├── README.md                

#package
  config-lite       config-lite 是一个轻量的读取配置文件的模块。config-lite 会根据环境变量（NODE_ENV）的不同从当前执行进程目录下的 config 目录加载不同的配置文件。如果不设      NODE_ENV，则读取默认的 default 配置文件，如果设置了 NODE_ENV，则会合并指定的配置文件和 default 配置文件作为配置，config-lite 支持 .js、.json、.node、.yml、.yaml 后缀的文件。
  省的每次清儒config里的配置文件;
  winston           日志
  express-winston   express日志插件
    #usage
      import winston from 'winston';
      import expressWinston from 'express-winston';
      app.use(expressWinston.logger({
          transports: [
              new (winston.transports.Console)({
                json: true,
                colorize: true
              }),
              new winston.transports.File({
                filename: 'logs/success.log'
              })
          ]
      }));
      // notice how the router goes after the logger.
      //使用路由，在logger之后，errorLogger之前
      app.use(expressWinston.errorLogger({
          transports: [
              new winston.transports.Console({
                json: true,
                colorize: true
              }),
              new winston.transports.File({
                filename: 'logs/error.log'
              })
          ]
      }));
  morgan        命令行log显示,app.use(morgan('dev'));// 命令行中显示程序运行日志,便于bug调试
  connectMongo  该模块用于将session存入mongo中
    import cookieParser from 'cookie-parser'
    import session from 'express-session';
    import connectMongo from 'connect-mongo';
    const MongoStore = connectMongo(session);
    app.use(cookieParser());
    app.use(session({
                      name: config.session.name,
                      secret: config.session.secret,
                      resave: true,
                      saveUninitialized: false,
                      cookie: config.session.cookie,
                      store: new MongoStore({
                        url: config.url
                  })
    }))
  connect-history-api-fallback  import history from 'connect-history-api-fallback';
                                app.use(history());
  time-formater   时间格式插件
  formidable      post方法body体里，解析json,缺点要指定格式application/x-form--...,可以解析图片等信息
                  var form = new formidable.IncomingForm();
                  form.parse(req, function (err, fields, files) {}) 
  body-parser     // 解析body字段模块
                  app.use(bodyParser.urlencoded({ extended: false }));
                  app.use(bodyParser.json()); // 调用bodyParser模块以便程序正确解析body传入值
  gm              图片剪切处理插件
  socket.io       websocket
                  //socket.io公式：
                  var http = require('http').Server(app);
                  var io = require('socket.io')(http);
                  //server
                  io.on("connection",function(socket){
                    //监听客户端
                    socket.on("liaotian",function(msg){
                      //把接收到的msg原样广播 
                      io.emit("liaotian",msg);
                    });
                  });
                  //client
                  var socket = io();
                  //发送server端
                  socket.emit("liaotian",{
                    "neirong" : $("#neirong").val(),
                    "ren" : $("#yonghu").html()
                  });
                  //监听server端
                  socket.on("liaotian",function(msg){
                    $(".liebiao").prepend("<li><b>" + msg.ren + "：</b>"+ msg.neirong + "</li>");
                  });
  crpto           //支持md5/sha1/sha256... 用于用户模块
  bcrypt-nodejs   bcrypt加密
                  bcrypt.hash(user.password, null, null, function (err, hash){  //存储
                    if (err) {
                        return next(err)
                    } 
                    user.password = hash
                    next() 	
                  });
                  bcrypt.compare(passw, this.password, (err, isMatch) => {      //校验
                      if (err) {
                          return cb(err);
                      }
                      cb(null, isMatch);
                  });
  passport        //用户认证模块passport,与下面模块配合使用
                    app.use(passport.initialize());// 初始化passport模块
                    require('passport-http-bearer').Strategy;// token验证模块
                    //demo
                    passport.use(new Strategy(
                      function(token, done) {
                          User.findOne({
                              token: token
                          }, function(err, user) {
                              if (err) {
                                  return done(err);
                              }
                              if (!user) {
                                  return done(null, false);
                              }
                              return done(null, user);
                          });
                      }
                    ));
  jsonwebtoken     var token = jwt.sign({name: user.name}, config.secret,{
                    expiresIn: 10080
                  });
                  user.token = token;
                  //获取用户信息
                  // passport-http-bearer token 中间件验证
                  // 通过 header 发送 Authorization -> Bearer  + token
                  // 或者通过 ?access_token = token
                  passport.authenticate('bearer', { session: false })






