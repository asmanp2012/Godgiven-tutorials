# قیدها - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`

Constraint به معنای قید و محدودیت است.

در اس کیو ال سرور constraint ها به ما اجازه میدهند قوانینی را تعیین کنیم که موتور پایگاه داده بطور اتوماتیک بر صحت اطلاعات وارد شده نظارت داشته باشد. و اجازه وارد کردن مقادیری که (integrity) (صحت ، جامعیت) دیتابیس را بر هم میزنند ندهد .

## انواع قید ها ( All constraint )

- قید **not null** : مشخص میکند که فیلد مقادیر null را نمی پذیرد .
- قید **Check** : جهت محدود کردن مقادیر ورودی در فیلد مثلا محدود کنیم که مقدار ورودی در فیلد بین 100 تا 2000 باشد.
- قید **UNIQUE** : مقدار فیلد مورد نظر باید در تمامی رکورد های یک جدول منحصر به فرد و یکتا باشد .
- قید **PRIMARY KEY** : کلید اصلی جدول در واقع با این قید مشخص میشود 
- قید **FOREIGN KEY** : رابطه (relationship ) بین جداول را مشخص میکند
- قید **Default** : تعیین مقدار پیشفرض برای فیلد . اگر مقداری در فیلد وارد نشد این مقدار بصورت اتوماتیک در فیلد درج میشود

حال در ادامه مثال ها و توضیحات بیشتری در مورد هر کدام از قید ها را، برای شما جمع آوری کرده ایم.

------------------------------------------

در ابتدا یک دیتا بیس ساخته

`````````sql
create database test
`````````

بعد از ساخت دیتا بیس باید برای استفاده از آن  از دستور `use` استفاده کنیم

`````````sql
use test
`````````

حال با دستور ایجاد جدول یک جدول با قید `not null` ایجاد میکنیم

`````````sql
create table EMP(
    empcode varchar(8) not null,
    salary decimal(18,0)
)
`````````

حال سعی میکنیم که مقدار null را به این جدول اضافه کنیم

`````````sql
INSERT INTO EMP([empcode],[salary]) VALUES(null,'2')
/*
خروجی به شکل زیر میباشد
Msg 515, Level 16, State 2, Line 1
Cannot insert the value NULL into column 'empcode', table 'test.dbo.EMP'; column does not allow nulls. INSERT fails.
The statement has been terminated.
حال دستور پایین را امتحان میکنیم
*/
INSERT INTO EMP([empcode],[salary]) VALUES('','2')
/*
خروجی به شکل زیر میباشد
(1 row affected)
پس دقت کنید که مقدار نال با رشته تهی متفاوت است
*/
`````````

حال میخواهیم رشته تهی را نیز انجین دیتا بیس چک کند پس مقادیر گذشته را پاک کرده و جدول را به شکل زیر تغیر مدهیم

`````````sql
DELETE FROM EMP
go
alter table EMP
    add constraint ec_checker check(empcode != '')
/*
خروجی به شکل زیر میباشد
(2 rows affected)
*/
`````````

حال سعی میکنیم همان دستور قبلی رو استفاده کنیم برای اضافه کردن ردیفی با empcode خالی

`````````sql
INSERT INTO EMP([empcode],[salary]) VALUES('','2')
/*
میبینید که خروجی به شکل زیر میشود

Msg 547, Level 16, State 0, Line 1
The INSERT statement conflicted with the CHECK constraint "ec_checker". The conflict occurred in database "test", table "dbo.EMP", column 'empcode'.
The statement has been terminated.
*/
`````````

با توجه به قیدی که در این جدول و فیلد ایجاد کرده اید sql-server به شما اجازه اضافه کردن چنین ردیفی را نمیدهد

------------------------------------------

حال برای این که کلید اصلی در این جدول داشته باشیم از قید `PRIMARY KEY` استفاده میکنیم<br/>

`````````sql
alter table EMP
add constraint pk_empcode primary key(empCode)
`````````

حال برای تست کلید اصلی دو ردیف با empCode تکراری اضافه میکنیم

`````````sql
INSERT INTO EMP([empcode],[salary]) VALUES('3','2')
go
INSERT INTO EMP([empcode],[salary]) VALUES('3','2')
/*
خروجی به شکل زیر میباشد
(1 row affected)
Msg 2627, Level 14, State 1, Line 3
Violation of PRIMARY KEY constraint 'pk_empcode'. Cannot insert duplicate key in object 'dbo.EMP'. The duplicate key value is (3).
The statement has been terminated.
*/
`````````

همینطوری که میبینید شما نمیتوانید مقدار تکراری وارد کنید چون `empCode` کلید اصلی جدول ما میباشد. <br/>
تا به اینجا سه قید را بررسی کردیم حال قید بعدی یعنی `UNIQUE` را بررسی میکنیم <br/>
ما فیلد دیگری به جدول خود اضافه کرده و آن را با قید `unique` مشخص میکنیم.

`````````sql
DELETE FROM EMP
go
alter table EMP
    add un_codemelli char(10) unique
`````````

در اینجا شما نمیتوانید کد ملی را تکراری وارد کنید و اگر قید `not null` و `check(un_codemelli != '')` را بگیرد مشابه کلید اصلی میشود.
بعد از قید `unique` ما قید `default` را داریم که مقدار پیش فرض فیلد ها را مشخص میکند

`````````sql
alter table EMP
    add constraint de_salary default 0 for salary
`````````

و در آخر قید   `foreign key`  میباشد که ارتباطات را بین جداول برقرار میکند

`````````sql
create table pm (
    fk_pm_emp char(10) not null  foreign key references EMP(codemelli)
        on delete cascade
        on update cascade,
    pm varchar(20),
)
`````````

آخرین قیدی که میتوان از آن استفاده کرد `default` میباشد این قید مقدار پیش فرضی برای فیلد مشخص میکند.

`````````sql
alter table EMP
    add constraint de_salary default 0 for salary
`````````
