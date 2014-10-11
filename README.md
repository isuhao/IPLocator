# 基于qqwry.dat查找ip归属地的java程序

## 使用：

__1. 直接调用（推荐，效率最高）：__

<!-- language: lang-java -->
	
	String ip = "202.108.22.5";
	QQWryFile qqWryFile = QQWryFile.getInstance();
	RandomAccessFile ipFile = qqWryFile.getIpFile();
	QQWryRecord record = qqWryFile.find(ip, ipFile);
	System.out.println(Utils.ipToStr(record.getBeginIP()));
	System.out.println(Utils.ipToStr(record.getEndIP()));
	System.out.println(record.getCountry());
	System.out.println(record.getArea());

__结果：__  <br/>
> 202.108.22.5 <br/>
北京市 <br/>
北京百度网讯科技有限公司联通节点 <br/>

__2.  PostgreSQL:__ <br/>
> 1. 在pg数据库里新建数据名为 *ipsdb* 的数据库（如需其他名字，在PostgreSQL.java文件内改相应的配置）<br/>
2. 调PostgreSQL.java里的storage方法，把ip库存进pg数据里<br/>
3. 查询：`select * from ips where ip_start <= inet '202.108.22.5' and ip_end >= inet '202.108.22.5';`<br/>

__3. mongodb:__ 
> 1. 在pg数据库里新建数据名为 *ipsdb* 的数据库（如需其他名字，在MongoDB.java文件内改相应的配置） <br/>
2. 调MongoDB.java里的storage()方法，把ip库存进mongo数据里<br/>
3. 先把ip按Utils文件里的ipToLong变成长整型，然后再去查：<br/>
`db.ips.find({ip_start : {$lte : NumberLong(3396077979)}, ip_end : {$gte : NumberLong(3396077979)}})`<br/>

__4. mysql:__
> 1. 在pg数据库里新建数据名为 *ipsdb* 的数据库（如需其他名字，在MySQL.java文件内改相应的配置） <br/>
2. 调MySQL.java里的storage()方法，把ip库存进mysql数据里<br/>
select * from ips where ip_start <= inet_aton('202.108.22.5') and ip_end >= inet_aton('202.108.22.5');
	
## 参考资料：

1. 纯真IP数据库格式详解：http://lumaqq.linuxsir.org/article/qqwry_format_detail.html
2. ruby程序的实现：https://github.com/hide2/ruby-qqwry
3. java程序的实现：http://www.ehelper.com.cn/blog/post/java05.html
4. 纯真IP数据库qqwry.dat更新：http://www.cz88.net/ 右上角有个IP数据库下载，里面的说明文件里说明了更新的方法。