# نما ها - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`  

وقتی که ما جستجوی پیچیده ای را انجام میدهیم و بار ها از آن استفاده میکنیم میتوانیم به راحتی آن را تبدیل به یک نما بکنیم و از آن پس با یک جدول مجازی مواجه هستیم که پیچیدگی زیادی ندارد  

> برای تست و تمرین ما از دیتا بیس uni که در کتاب نیز ذکر شده است استفاده میکنیم اگر تا کنون با این دیتا بیس مواجه نشده اید لطفا به بخش دیتا بیس uni مراجعه کرده و این دیتا بیس را ایجاد کنید.

برای شروع و استفاده از دیتا بیس `uni` شما باید از `use uni` استفاده کنید.  

در جلسه گذشته نحوه جستجو پیشرفته را بررسی کرده ایم در این جلسه میخواهیم نحوه تبدیل جستجو ها به یک نما را توضیح بدهیم

ساختار کلی ساخت یک نما به شکل زیر میباشد

`````````sql
Create view [<database_name>.][<owner>.] view_name [(column[,...n])]
[with <view_attribute> [,...n]]
As
Select_statement
[with check option]

<view_atrribute> ::= { Encryption | schemabinding | view_metadata }
`````````

دلیل استفاده از نما ها میتواند موارد زیر باشد:

- تغیر در ساختار یا سفارشی کردن برای مجموعه ای از کاربران
- برای کنترل دسترسی به رکورد ها و فیلد ها
- برای جمع آوری داده و افزایش پرفورمنس

به مثال زیر دقت کنید  

`````````sql
create view hamshahri
as
select s1.stNo's1stno',s1.Fname's1fname',s1.Lname's1lname',city.cityName,
       s2.stNo's2stno',s2.Fname's2fname',s2.Lname's2lname'
from Student s1
inner join Student s2
on s1.cityCode=s2.cityCode
inner join city
on s1.cityCode=City.cityCode
where s1.stno>s2.stNo
/*************************************/
go
select * from hamshahri
where s1stno=10
`````````

در مثال بالا ما یک کوئری پیچیده را تبدیل به یک نما کرده ایم و از آن یک نما ساخته ایم اما اگر بخواهیم اطلاعات آن را با ساختار دیگری ارائه بدهیم به شکل زیر عمل میکنیم:  

`````````sql
create view gr_st_co(st,fn,ln,corn,nc,grad)
as
select grade.stNo,fname,lname,grade.CourseNo,Name,grade
from grade
inner join Student
on grade.stno=student.stno
inner join Course
on grade.CourseNo=Course.CourseNo
/*************************************/
go
select sum(grad)'sumgrade',count(*)'count',AVG(grad)'average' 
from gr_st_co
group by st
`````````

حال اگر بخواهیم این نما ها پاک کنیم به شکل زیر عمل میکنیم

`````````sql
drop view gr_st_co
go
drop view hamshahri
`````````
