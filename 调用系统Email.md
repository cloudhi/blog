#����ϵͳEmail

һ���Ƚϼ򵥵Ĺ��߷���
```java
        Intent data = new Intent(Intent.ACTION_SENDTO);
        data.setData(Uri.parse("mailto:kymjs123@gmail.com"));
        data.putExtra(Intent.EXTRA_SUBJECT, "this is title");
        data.putExtra(Intent.EXTRA_TEXT, "this is content");
        startActivity(data);
```
