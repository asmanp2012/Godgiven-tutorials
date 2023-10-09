# شاخص ها - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`

کلمه index به معنای شاخص و علامت است.  

ایندکس ها باعث میشوند موتور پایگاه داده کل جدول را برای پیدا کردن یک ایتم نگردد. همانند ایندکس های موجود در یک دیکشنری که براحتی می توان به کمک ان به سراغ کلمه مورد نظر که با آن حرف شروع می شوند رفت بدون اینکه کل کتاب را خواند .  

## انواع شاخص ها ( All index )

- شاخص **Clustered index** : یک کلاستر ایندکس مشخص میکند رکورد های یک جدول چگونه (با چه ترتیبی) بطور فیزیکی در هارد دیسک ذخیره شوند بنابراین یک جدول تنها یک کلاستر ایندکس خواهد داشت . هنگامی که یک کلید اصلی primary key می سازیم اس کیو ال سرور بصورت اتوماتیک یک کلاستر ایندکس برای ان می سازد .
- شاخص **Nonclustered index** :این نوع ایندکس دخالتی در ترتیب ذخیره رکورد ها ندارد و تنها یک مقدار و اشاره گر (pointer) به رکوردی که حاوی ان مقدار می باشد را ذخیره میکنند .  

>همانطور که در جلسه پیش ساخت دیتا بیس uni را بررسی کرده ایم و این دیتا بیس که در کتاب معرفی شده است را ساخته ایم از این درس به بعد از آن به عنوان مرجع استفاده کرده دستورات را بر روی این دیتا بیس اجرا میکنیم

برای شروع و استفاده از دیتا بیس `uni` شما باید از `use uni` استفاده کنید.  

-

فرمت کلی ایجاد ایندکس به صورت زیر میباشد

`````````sql
CREATE [UNIQUE] [CLUSTERED|NONCLUSTERED]
INDEX <index name> ON <table or view name>(<column name> [ASC|DESC] [,...n])
  INCLUDE (
    <column name> [, ...n]
    [WHERE <condition>]
  )
  [ WITH
  [PAD_INDEX = { ON | OFF }]
  [[,] FILLFACTOR = <fillfactor>]
  [[,] IGNORE_DUP_KEY = { ON | OFF }]
  [[,] DROP_EXISTING = { ON | OFF }]
  [[,] STATISTICS_NORECOMPUTE = { ON | OFF }]
  [[,] SORT_IN_TEMPDB = { ON | OFF }]
  [[,] ONLINE = { ON | OFF }
  [[,] ALLOW_ROW_LOCKS = { ON | OFF }
  [[,] ALLOW_PAGE_LOCKS = { ON | OFF }
  [[,] MAXDOP = <maximum degree of parallelism>
  [[,] DATA_COMPRESSION = { NONE | ROW | PAGE}
  ]
  [ON {<filegroup> | <partition scheme name> | DEFAULT }]
`````````

در ابتدا ما باید بدانیم که چطور میتوانیم یک سلکت را مرتب کنیم
برای مرتب کردن سلکت به صورت زیر عمل میکنیم

`````````sql
select stno,fname,lname
  from Student
  order by lname
`````````

در حالت بالا ما برای جستجو به صورت طبیعی و حالت معمول بسنده کرده ایم اما اگر بخواهیم سرعت جستجو بر روی فیلد نام خانوادگی را بیشتر کنیم به صورت زیر عمل میکنیم  

در ابتدا ایندکسی با عبارت کلیدی `create index` ساخته و بعد با توجه به آن ایندکس جستجوی خود را نمایش میدهیم

`````````sql
create index idx_lname on student(lname)
go
select stno,fname,Lname
  from Student
  with(index(idx_lname))
`````````

اگر بخواهیم چندین فیلد را با هم ایندکس گذاری کنیم کافیست به شکل زیر عمل کنیم

`````````sql
create index idx_lname_fname
  on student(lname,fname desc)
go
select stno,fname,Lname
  from Student
  with(index(idx_lname_fname))
`````````

و در انتها با کلمه `drop index` میتوانیم ایندکس ایجاد شده را پاک کنیم

`````````sql
drop index Student.idx_lname
go
drop index Student.idx_lname
`````````
