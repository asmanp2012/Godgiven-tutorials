# ساختار های منطقی - Stored procedure - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`  

در sql server ما میتوانیم تا حدودی برنامه نویسی بکنیم در این مقاله ما به ابتدایی ترین ساختار های آن میپردازیم

> برای تست و تمرین ما از دیتا بیس uni که در کتاب نیز ذکر شده است استفاده میکنیم اگر تا کنون با این دیتا بیس مواجه نشده اید لطفا به بخش دیتا بیس uni مراجعه کرده و این دیتا بیس را ایجاد کنید.

برای شروع و استفاده از دیتا بیس `uni` شما باید از `use uni` استفاده کنید.  

-

در ابتدا شما باید بدانید که ما میتوانیم در sql server `متغیر` تعریف کنیم به آن مقدار بدهیم و از آن استفاده کنیم.  

-

برای تعریف متغیر ما به شکل زیر عمل میکنیم:

`````````sql
DECLARE @variable_name datatype [ = initial_value ],
        @variable_name datatype [ = initial_value ],
        ...;
`````````

- عبارت **variable_name** : نام متغیر میباشد
- عبارت **datatype** : نوع متغیر میباشد
- عبارت **initial_value** : مقدار اولیه ای اگر بخواهیم ست کنیم

به مثال زیر توجه کنید

`````````sql
DECLARE @test VARCHAR(50);
SET @techonthenet = 'Example showing how to declare variable';
`````````

همانطور که میبیند در بالا برای مقدار دهی دوباره به متغیر از عبارت `set` استفاده کرده ایم.  

تعریف متغیر و استفاده از آن در تمام قسمت های sql server یکسان و درست میباشد و هیچ گونه تفاوتی ندارد.  

## ساختار Stored procedure

حال که تعریف متغیر را متوجه شده اید میخواهیم ‍‍`Stored procedure` را بررسی بکنیم  

معنی لغوی این عبارت ذخیره روند میباشد در w3schools به این شکل معرفی شده است: یک روش ذخیره سازی کد که میتوانید کد هایی که بارها و بارها استفاده میکنید به صورت یک `Stored procedure` ذخیره کنید.  

> توجه کنید که `Stored procedure` فقط میتوانید به شما نتیجه یک جستجو را خروجی بدهد و توانایی دادن خروجی به عنوان یک جدول را ندارد.

ساختار ایجاد `Stored procedure` به شکل زیر میباشد.

`````````sql
create procedure procedure_name[;number]
[{@parameter data_type} [ varying ] [=default] [output] ] [,...n]
[with {recompile | encryption | recompile,encryption } ]
[for replication ]
as
sql_statement [,...n]
`````````

به مثال زیر توجه کنید

`````````sql
create procedure proc_select_grade
as
select stNo,sum(grade)'sumgrade',avg(grade)'avggrade'
from Grade
group by stno
go
exec proc_select_grade
`````````

در مثال بالا میبیند که برای اجرای ‍`Stored procedure` از عبارت کلیدی ‍‍`exec` استفاده میکنیم.  

اما متغیری تعریف نکرده ایم و ورودی را نیز مشخص نکرده ایم برای دریافت ورودی از یک ‍`Stored procedure` به شکل زیر عمل میکنیم.

`````````sql
create procedure proc_select_grade_hav
@x int
as
select stNo,count(*)'count',sum(grade)'sumgrade',avg(grade)'avggrade'
from Grade
group by stno
having count(*)>@x
go
exec proc_select_grade_hav 1
`````````

در انتها برای پاک کردن آن ها به شکل زیر عمل میکنیم

`````````sql
drop procedure proc_select_grade_hav
go
drop procedure proc_select_grade
`````````
