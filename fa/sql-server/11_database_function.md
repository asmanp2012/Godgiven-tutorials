# ساختار های منطقی - function - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`  

در جلسه قبل نجوه ایجاد متغیر و پروسیجر را بررسی کرده ایم در این بخش ما به بررسی `trigger` میپردازیم

> برای تست و تمرین ما از دیتا بیس uni که در کتاب نیز ذکر شده است استفاده میکنیم اگر تا کنون با این دیتا بیس مواجه نشده اید لطفا به بخش دیتا بیس uni مراجعه کرده و این دیتا بیس را ایجاد کنید.

برای شروع و استفاده از دیتا بیس `uni` شما باید از `use uni` استفاده کنید.  

تابع یا Function یک تکه کد است که برای اجرای عملیات مجزا بکار می رود. پس از اجرای تکلیف و عملیات محول شده به آن، می توان به نتایج یا خروجی تابع دسترسی داشت. در SQL نیز توابع کاربردی مشابه به توابع در دیگر زبان ها دارند، بدین معنی که می توان از آن ها برای اجرای عملیات مختلف بهره گرفت.  

در زبان Transact-SQL، تابع یک شی محسوب می شود. پس از ایجاد یا تعریف آن، تابع در پایگاه داده ذخیره می شود، سپس می توان آن را در صورت نیاز و هر زمان که لازم بود صدا زده و اجرا کرد.  

برای ایجاد تابع ما به شکل زیر عمل میکنیم

`````````sql
CREATE FUNCTION [<schema name>.]<function name>
  ( [ <@parameter name> [AS] [<schema name>.]<data type>
  [ = <default value> [READONLY]]
  [ ,...n ] ] )
RETURNS {<scalar type>|TABLE [(<table definition>)]}
  [ 
    WITH [ENCRYPTION]|[SCHEMABINDING]|
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ] |
    [EXECUTE AS { CALLER|SELF|OWNER|<’user name’>} ]
  ]
[AS] { EXTERNAL NAME <external method> |
BEGIN
  [<function statements>]
  {RETURN <type as defined in RETURNS clause>|RETURN (<SELECT statement>)}
END }[;]
`````````

به مثال زیر توجه کنید:

`````````sql
create Function CoursePass()
returns Table
as
  return (
    select s.stNo,Fname,Lname,Name,Grade
    from Grade g,Student s, Course c
    where grade>=10 and c.CourseNo=g.CourseNo and s.stNo=g.stNo
  )
GO
`````````

در مثال بالا میبیند که یک تابع با نام `CoursePass` تعریف شده است که نوع خروجی آن بعد از عبارت کلیدی `returns` از نوع `Table` مشخص شده و در خط بعد با عبارت کلیدی `as` دستورات و عملیات های مورد نظر نوشته شده است.  

حال میتوانید از این تابع به عنوان یک جدول در پرس و جوی خود استفاده کنیم.

`````````sql
select * from CoursePass()
go
`````````

اگر بخواهیم فانکشن تعریف شده را ویرایش کنیم از دستور `alter` استفاده میکنیم.  

`````````sql
Alter Function CoursePass()
returns Table
as
    return (
        select s.stNo,Fname,Lname,Name,Grade
        from Grade g,Student s, Course c
        where grade>=10 and c.CourseNo=g.CourseNo and s.stNo=g.stNo
    )
go
`````````

در فانکشن ها میتوان ورودی دریافت کرد برای دریافت ورودی کافیست میان پرانتز بعد از نام تابع ورودی و نوع ان را مشخص کنیم  

به مثال زیر توجه کنید:  

`````````sql
create function notPass(@score real)
returns Table
As
return(select Student.stno,Lname,Course.Name,grade
  from grade,student,Course
  where grade<@score and course.CourseNo=Grade.CourseNo
    and Student.stno=grade.stNo)
`````````

در اینجا ما میتوانیم با ارسال پارامتر `score` به تابع  `notPass` متوجه شویم چه کسانی چه درس هایی را کمتر از نمره مورد نظر ما گرفته اند:  

`````````sql
select * from notpass(10)
`````````

در انتها نیز برای پاک کردن فانکشن ها به شکل زیر عمل میکنیم  

`````````sql
drop function CoursePass
drop function notPass
`````````
