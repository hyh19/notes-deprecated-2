# [Class SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)

- **解析日期字符串**

```java
import java.text.SimpleDateFormat;
import java.util.Date;

public class HelloWorld {

    public static void main(String[] args) throws Exception {

        String dateString1 = "2018-05-24";
        String dateString2 = "2018-05-21";

        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");

        // 解析日期字符串，返回日期对象。
        Date date1 = simpleDateFormat.parse(dateString1);
        Date date2 = simpleDateFormat.parse(dateString2);

        System.out.println(date1);
        System.out.println(date2);
    }
}
```

输出结果

```bash
Thu May 24 00:00:00 CST 2018
Mon May 21 00:00:00 CST 2018
```