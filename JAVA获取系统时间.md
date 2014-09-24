一. 获取当前系统时间和日期并格式化输出:
```java
	import java.util.Date;
	import java.text.SimpleDateFormat;
	public class NowString {
		public static void main(String[] args) {
		SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
		System.out.println(df.format(new Date()));// new Date()为获取当前系统时间
	}
}
```
二. 在数据库里的日期只以年-月-日的方式输出，可以用下面两种方法：
1、用convert()转化函数：
```java
	String sqlst = "select convert(varchar(10),bookDate,126) as convertBookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
	System.out.println(rs.getString("convertBookDate"));
```
2、利用SimpleDateFormat类：
先要输入两个java包：
```java
	import java.util.Date;
	import java.text.SimpleDateFormat;
	//定义日期格式：
	SimpleDateFormat sdf = new SimpleDateFormat(yy-MM-dd);
	//sql语句为：
	String sqlStr = "select bookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
	//输出：
	System.out.println(df.format(rs.getDate("bookDate")));
```
************************************************************
java中获取当前日期和时间的方法
 ```java
import java.util.Date;
import java.util.Calendar;

import java.text.SimpleDateFormat;

public class TestDate{
	public static void main(String[] args){
		Date now = new Date();
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");//可以方便地修改日期格式

		String hehe = dateFormat.format( now );
		System.out.println(hehe);

		Calendar c = Calendar.getInstance();//可以对每个时间域单独修改

		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH);
		int date = c.get(Calendar.DATE);
		int hour = c.get(Calendar.HOUR_OF_DAY);
		int minute = c.get(Calendar.MINUTE);
		int second = c.get(Calendar.SECOND);
		System.out.println(year + "/" + month + "/" + date + " " +hour + ":" +minute + ":" + second);
	}
}
	//有时候要把String类型的时间转换为Date类型，通过以下的方式，就可以将你刚得到的时间字符串转换为Date类型了。
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
	java.util.Date time = null;
	try {
	   time = sdf.parse(sdf.format(new Date()));
	} catch (ParseException e) {
	   e.printStackTrace();
	}
```