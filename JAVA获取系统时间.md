һ. ��ȡ��ǰϵͳʱ������ڲ���ʽ�����:
```java
	import java.util.Date;
	import java.text.SimpleDateFormat;
	public class NowString {
		public static void main(String[] args) {
		SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//�������ڸ�ʽ
		System.out.println(df.format(new Date()));// new Date()Ϊ��ȡ��ǰϵͳʱ��
	}
}
```
��. �����ݿ��������ֻ����-��-�յķ�ʽ������������������ַ�����
1����convert()ת��������
```java
	String sqlst = "select convert(varchar(10),bookDate,126) as convertBookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
	System.out.println(rs.getString("convertBookDate"));
```
2������SimpleDateFormat�ࣺ
��Ҫ��������java����
```java
	import java.util.Date;
	import java.text.SimpleDateFormat;
	//�������ڸ�ʽ��
	SimpleDateFormat sdf = new SimpleDateFormat(yy-MM-dd);
	//sql���Ϊ��
	String sqlStr = "select bookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
	//�����
	System.out.println(df.format(rs.getDate("bookDate")));
```
************************************************************
java�л�ȡ��ǰ���ں�ʱ��ķ���
 ```java
import java.util.Date;
import java.util.Calendar;

import java.text.SimpleDateFormat;

public class TestDate{
	public static void main(String[] args){
		Date now = new Date();
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");//���Է�����޸����ڸ�ʽ

		String hehe = dateFormat.format( now );
		System.out.println(hehe);

		Calendar c = Calendar.getInstance();//���Զ�ÿ��ʱ���򵥶��޸�

		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH);
		int date = c.get(Calendar.DATE);
		int hour = c.get(Calendar.HOUR_OF_DAY);
		int minute = c.get(Calendar.MINUTE);
		int second = c.get(Calendar.SECOND);
		System.out.println(year + "/" + month + "/" + date + " " +hour + ":" +minute + ":" + second);
	}
}
	//��ʱ��Ҫ��String���͵�ʱ��ת��ΪDate���ͣ�ͨ�����µķ�ʽ���Ϳ��Խ���յõ���ʱ���ַ���ת��ΪDate�����ˡ�
	SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
	java.util.Date time = null;
	try {
	   time = sdf.parse(sdf.format(new Date()));
	} catch (ParseException e) {
	   e.printStackTrace();
	}
```