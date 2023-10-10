# ساختار های منطقی - trigger - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`  

در جلسه قبل نجوه ایجاد متغیر و پروسیجر را بررسی کرده ایم در این بخش ما به بررسی `trigger` میپردازیم

> برای تست و تمرین ما از دیتا بیس uni که در کتاب نیز ذکر شده است استفاده میکنیم اگر تا کنون با این دیتا بیس مواجه نشده اید لطفا به بخش دیتا بیس uni مراجعه کرده و این دیتا بیس را ایجاد کنید.

برای شروع و استفاده از دیتا بیس `uni` شما باید از `use uni` استفاده کنید.  

در [وب سایت مایکروسفات](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-trigger-transact-sql?view=sql-server-ver15) `trigger` را اینگونه معرفی کرده است:  

نوعی پروسیجر که هنگام وقوع یک رویداد در سرور پایگاه داده به طور خودکار اجرا میشود.
برای ایجاد یک `trigger` ما به شکل زیر عمل میکنیم.  

`````````sql
CREATE TRIGGER <trigger name>
    ON [<schema name>.]<table or view name>
    [WITH ENCRYPTION | EXECUTE AS <CALLER | SELF | <user> >]
    {{{FOR|AFTER} <[DELETE] [,] [INSERT] [,] [UPDATE]>} |INSTEAD OF}
    [NOT FOR REPLICATION]
AS
    < <sql statements> | EXTERNAL NAME <assembly method specifier> >
`````````

در ابتدا برای بررسی تریگر ما نیاز به یک جدول با نام `Log_City` داریم  

برای ایجاد این جدول دستورات زیر را اجرا کنید  

`````````sql
CREATE TABLE Log_City(
  DateAction time(7) NOT NULL,
  ActionC varchar(10) NOT NULL,
  cityCode varchar(3) NOT NULL
)
GO
`````````

حال میخواهیم تریگری ایجاد کنیم که در زمان عملیات افزودن به پایگاه داده اجرا شود به شکل زیر عمل میکنیم.  

`````````sql
create trigger Insert_Trigger
  on City
  for insert
as
  declare @cityCode varchar(3)
  declare @action varchar(10)
  select @cityCode=cityCode, @action='insert' from inserted
  insert into Log_City(DateAction,ActionC,cityCode)
  values(GETDATE(),@action,@cityCode)
`````````

همینطور که میبینید ما با کلمه `on`  مشخص کرده ایم که بر روی چه جدولی این تریگر ایجاد شود  

با کلمه کلیدی `for` مشخص کرده ایم با انجام چه عملیاتی این تریگر اجرا شود.  

و در اخر نیز با کلمه کلیدی `as` مشخص کرده ایم که چه عملیات  هایی در این تریگر انجام بشوند.  

>نکته اگر دقت بکنید میتوانید ببینید برای گرفتن مقادر از عملیات افزودن در پرس و جو از عبارت `inserted` استفاده شده است

حال برای تست این تریگر کافیست کوئری زیر را اجرا کنید.

`````````sql
insert into City
values('001','تست')
`````````

اما اگر بخواهیم این عملیات ها در زمان بروزرسانی انجام شود به شکل زیر عمل میکنیم

`````````sql
create trigger Update_Trigger
on City
for update
as
declare @cityCode varchar(3)
declare @action varchar(10)
select @cityCode=cityCode, @action='update' from city 
insert Log_City(DateAction,ActionC,cityCode)
values(GETDATE(),@action,@cityCode)
`````````

همینطور که میبیند بعد از کلمه کلیدی `for`  کلمه  `update` است.  

حال میخواهیم تریگر خود را تست کنیم برای تست کوئری زیر را اجرا میکنیم  

`````````sql
update City
set cityName='تست اشتباه'
where cityCode='001'
`````````

اگر مشاهده کنید خروجی آن صحیح نمیباشد چرا که ما در تریگر پرسوجوی خود را از جدول city انجام داده ایم  

در تریگر ها برای گرفتن مقادیر عملیات انجام شده باید از مقادیر منطقی `inserted` و `DELETED` استفاده کنید.  

اگر از جداول پرس و جوی خود را انجام بدهیم معمولا آخرین رکورد ان جدول در نظر گرفته میشود.  

برای تصحیح تریگر بالا آن را به شکل زیر ویرایش میکنیم  

`````````sql
alter trigger Update_Trigger
  on City
  for update
as
  declare @cityCode varchar(3)
  declare @action varchar(10)
  select @cityCode=cityCode, @action='update' from inserted
  insert Log_City(DateAction,ActionC,cityCode)
  values(GETDATE(),@action,@cityCode)
`````````

حال دوباره تریگر خود را با کوئری پایین امتحان میکنیم

`````````sql
update City
set cityName='تست تریگر درست'
where cityCode='001'
`````````

در انتها میخوایم که تریگری بنویسیم که از حذف شهر ها جلوگیری کنه برای این کار به این صورت شروع میکنیم.

`````````sql
create trigger Delete_Trigger
  on City
  for delete
as
  print('Can not delete')
`````````

حالا برای امتحان این تریگر به شکل زیر عمل میکنیم

`````````sql
delete from City where cityCode='001'
`````````

اگر کوئری را اجرا کرده باشید متوجه میشید که رکورد حذف شده و شما با این روش نتونستید که از عملیات حذف جلوگیری کنید  

بله برای جلوگیری از عملیات ها به جای استفاده از کلمه `for` از کلمه `instead` باید استفاده کنیم
در کوئری پایین از `instead`  استفاده شده است.  

`````````sql
alter trigger Delete_Trigger
    on City
    instead of delete
as
    print('Can not delete')
`````````

برای امتحان آن نیز کوئری پایین را اجرا میکنیم

`````````sql
delete from city where citycode='013'
`````````
