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
