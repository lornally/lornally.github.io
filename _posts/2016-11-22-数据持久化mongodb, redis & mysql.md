

### mongodb

工具

1. 3T MongoChef
2. mongobooster
3. robomongo
4. 命令行永远是一个选项
5. http://mongodb-tools.com

工具中的命令

```
db.crawler.find({'_id': {$lte: 10} } )
可以使用findOne, 只查询一个比较省时间.
limit, skip
```

laravel中的命令

```php
//得到一些结果集, 返回数组.
$r=Crawler::limit(2)->get();
$r=Crawler::skip(2)->limit(2)->get();
$r=Crawler::skip(10)->take(2)->get();

//http://stackoverflow.com/questions/15229303/is-there-a-way-to-limit-the-result-with-eloquent-orm-of-laravel
Game::take(30)->skip(30)->get();
Game::limit(30)->offset(30)->get();
//分页方式
$games = Game::paginate(30);
// $games->results = the 30 you asked for
// $games->links() = the links to next, previous, etc pages
联表查询, 据说mongodb不行.
//http://stackoverflow.com/questions/18204342/laravel-4-how-to-limit-take-and-skip-for-eloquent-orm


//得到结果, 就是一条数据, 不是数组.
$r=Crawler::first();
//用id删除
$id = new ObjectID($item->_id);//方法一
dd( Crawler::where('_id',$id)->get());

Crawler::find($item->_id)->delete();//方法二
//新建一条记录
$da['country']=$item->country;
$da['ctime'] = get_time();
News::create($da); //news增加一个.
```

更有效率的查询

1. 关键是要控制结果集的尺寸.

```
工具robomongo里面. 
1. findOne 0.5s
2. find.limit(10)  12s
3. find.limit(100)显示6条 38s
4. find({})显示6条 39s
5. db.crawler.remove({}) 移除
6. count() 计数
7. db.posts.createIndex( { author_name : 1 } )建立索引
```



### redis

### mysql

sequel pro