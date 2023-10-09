
# ساخت و ویرایش پایگاه های داده - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`

## در mssql-server هر دیتابیس چند فایل دارد که به صورت پایین میباشد.

- فایل **mdf: محل ذخیره سازی اطلاعات** اصلی و تیبل ها و ارتباطات و .... هست.
- فایل **ldf: صرفا لاگ تغییراتی هست** که بر روی mdf انجام میشود. (که قابل تنظیم هست که از چه روشی برای لاگ استفاده کند)
- فایل **ndf: این فایل توسط کاربر ایجاد میشود** و ایجاد آن اختیاری میباشد به آن پرونده ثانویه میگویند

ساخت پایگاه داده با دستور `create database` با فرمت کلی زیر است.

`````````sql
CREATE DATABASE <database name>
[ON [PRIMARY]
  ([NAME = <’logical file name’>,]
  FILENAME = <’file name’>
    [, SIZE = <size in kilobytes, megabytes, gigabytes, or terabytes>]
    [, MAXSIZE = size in kilobytes, megabytes, gigabytes, or terabytes>]
    [, FILEGROWTH = <kilobytes, megabytes, gigabytes, or
   terabytes|percentage>])]
    [LOG ON
    ([NAME = <’logical file name’>,]
  FILENAME = <’file name’>
    [, SIZE = <size in kilobytes, megabytes, gigabytes, or terabytes>]
    [, MAXSIZE = size in kilobytes, megabytes, gigabytes, or terabytes>]
    [, FILEGROWTH = <kilobytes, megabytes, gigabytes, or terabytes|percentage>])]
[ CONTAINMENT = OFF|PARTIAL ]
[ COLLATE <collation name> ]
[ FOR ATTACH [WITH <service broker>]| FOR ATTACH_REBUILD_LOG|
  WITH DB_CHAINING ON|OFF | TRUSTWORTHY ON|OFF]
[AS SNAPSHOT OF <source database name>]
[;]
`````````

در کد پایین نحوه ایجاد یک دیتا بیس به همراه مشخص کردن مسیر هر کدام از فایل ها را مشاهده میکنید

`````````sql
create database Company
on primary(
  name='test_m', 
  filename='C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\test.mdf',
  size=100,
  maxsize=1gb,
  filegrowth=50%
),
(
  name='test_n',
  filename='C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\test.ndf',
  size=100,
  maxsize=unlimited,
  filegrowth=50%
)
log on(
  name='test_l',
  filename='C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\test.ldf',
  size=50,
  maxsize=500,
  filegrowth=20
)
`````````

اگر بخواهیم چندین فایل از فایل های بالا را ایجاد کنیم  به شکل زیر عمل میکنیم

`````````sql
create database test2
on primary(
  name='test_m1',
  filename='C:\db\test1.mdf',
    size=100,
    maxsize=1gb,
    filegrowth=50%
),
(
  name='test_m2',
  filename='C:\db\test2.mdf',
  size=100,
  maxsize=1gb,
  filegrowth=50%
),
(
  name='test_n1',
  filename='C:\db\test1.ndf',
  size=100,
  maxsize=1gb,
  filegrowth=50%
),
(
  name='test_n2',
  filename='C:\db\test2.ndf',
  size=100,
  maxsize=unlimited,
  filegrowth=50%
)
log on(
  name='test_l1',
  filename='C:\db\test1.ldf',
  size=50,
  maxsize=500,
  filegrowth=20
),
(
  name='test_l2',
  filename='C:\db\test2.ldf',
  size=50,
  maxsize=500,
  filegrowth=20
),
(
  name='test_l3',
  filename='C:\db\test3.ldf',
  size=50,
  maxsize=unlimited,
  filegrowth=20
)
`````````

حال که ایجاد دیتا بیس را فرا گرفته ایم باید بتوانیم آنرا ویرایش کنیم ویرایش دیتا بیس با استفاده از دستور `alter` با فرمت کلی زیر استفاده میشود

`````````sql
ALTER DATABASE <database name>
  ADD FILE(
    [NAME = <’logical file name’>,]
    FILENAME = <’file name’>
    [, SIZE = <size in KB, MB, GB or TB>]
    [, MAXSIZE = < size in KB, MB, GB or TB >]
    [, FILEGROWTH = <No of KB, MB, GB or TB |percentage>]
  ) [,...n] [ TO FILEGROUP filegroup_name] [, OFFLINE ]
  |ADD LOG FILE(
    [NAME = <’logical file name’>,]
    FILENAME = <’file name’>
    [, SIZE = < size in KB, MB, GB or TB >]
    [, MAXSIZE = < size in KB, MB, GB or TB >]
    [, FILEGROWTH = <No KB, MB, GB or TB |percentage>])
  |REMOVE FILE <logical file name> [WITH DELETE]
  |ADD FILEGROUP <filegroup name>
  |REMOVE FILEGROUP <filegroup name>
  |MODIFY FILE <filespec>
  |MODIFY NAME = <new dbname>
  |MODIFY FILEGROUP <filegroup name> 
    {
      <filegroup property>
      |NAME = <new filegroup name>
    }
  |SET <optionspec> [,...n ][WITH <termination>]
  |COLLATE <collation name>
`````````

برای **اضافه کردن فایل** به فایل های قبلی کافیست از دستور `alter` به همراه `add file` به شکل زیر استفاده کنیم.

`````````sql
alter database Company
add file(
  name='test_n2',
  filename='C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\testn2.mdf',
  size=100,
  maxsize=unlimited,
  filegrowth=50%
)
`````````

وقتی که میخواهیم فایل های قبلی را **ویرایش** کنیم از دستور `alter` به همراه `modify file` به شکل زیر استفاده کنیم

`````````sql
alter database Company
modify file(
  name='test_n',
  filename='C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\DATA\test.ndf',
  size=300,
  maxsize=1gb,
  filegrowth=150
)
`````````

حالا که ما ساختار فایل ها را متوجه و آنها را اضافه و تغیر دادیم باید بتوانیم فایل های مختلف را از دیتا بیس خود کم و پاک کنیم.
برای **پاک کردن** فایل از دیتا بیس  از دستور `alter` به همراه `remove file` به شکل زیر استفاده کنیم

`````````sql
alter database Company
 remove file test_n2
`````````

شما برای تغیر نام دیتا بیس باید به شکل فرمت کلی زیر عمل کنید

`````````sql
sp_renamedb [ @dbname = ] 'old_name' , [ @newname = ] 'new_name' 
`````````

که با توجه به دستورات قبلی ما تکه کد زیر را اجرا میکنیم، برای تغیر نام دیتا بیس از `company` به `test`

`````````sql
sp_renamedb 'Company','test'
`````````

و در انتها برای پاک کردن دیتا بیس از دستور `drop` به شکل زیر استفاده میکنیم.

`````````sql
drop database test2
go
drop database test
`````````
