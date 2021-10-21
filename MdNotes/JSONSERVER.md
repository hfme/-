在package.json里可以自定义启动命令

启动的时候，npm run 自定义启动命令名称

启动之后，jsonserver就配置好了 

 Resources
  http://localhost:3000/users
  http://localhost:3000/companies



.json文件里的属性和值都必须是双引号







使用postman请求json数据接口





获取一页中只有两条数据：?_page=1& _limit=2

http://localhost:3000/users?_page=1&_limit=3



升序排序： ?_sort=name& _order=asc(根据名字排序，顺序为升序)

http://localhost:3000/users?_sort=name&_order=asc



降序： _order=desc





通过postman可以给jsonserver post数据



更新： PATCH？ 不是PUT？