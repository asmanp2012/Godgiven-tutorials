# ساخت و ویرایش جدول - mssql-server 

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`

در ابتدا برای استفاده از یک دیتا بیس باید آن را ایجاد کنیم برای ایجاد دیتا بیس از دستور زیر استفاده میکنیم.

`````````sql
crate database Company;
`````````

بعد از این  دستور برای  کار روی محیط  این دیتا بیس باید از دستور پایین استفاده کنیم.

`````````sql
use Company;
`````````

حال برای ایجاد جدول از دستور `create table`  باید استفاده کنیم که فرمت کلی زیر آن به شکل زیر است.

`````````sql
CREATE TABLE [database_name.[owner].]table_name(
  <column name> <data type>
  [[DEFAULT <constant expression>]
    |[IDENTITY [(seed, increment) [NOT FOR REPLICATION]]]]
    [ROWGUIDCOL]
    [COLLATE <collation name>]
    [NULL|NOT NULL]
    [<column constraints>]
    |[column_name AS computed_column_expression]
    |[<table_constraint>]
    [,...n]
)
[ON {<filegroup>|DEFAULT}]
[TEXTIMAGE_ON {<filegroup>|DEFAULT}]
`````````

بر اساس فرمت کلی دستور میخواهیم جدول `S` را ایجاد کنیم

`````````sql
create table S
(
  S# char(5) not null primary key,
  sname varchar(15) not null,
  status_s int,
  city varchar(15)
)
`````````

همینطوری که میبینید ما  چهار فیلد را در دستور بالا ایجاد کرده ایم

- فیلد `S#` فیلدیست که 5 کارکتر را قبول میکند ( `char(5)` ) که مشخص شده است خالی و **null** نباشد ( `not null` ) از نوع کلید اصلی که با عبارت `primary key` مشخص میشود است
- فیلد `sname` فیلدیست که عباراتی از 0 تا 15 کارکتر را قبول میکنید ( `varchar(15)` ) و مشخص شده است که خالی و **null** نباشد ( `not null` )
- فیلد `status_s` فیلدیست از نوع عددی ( `int` )
- فیلد `city` فیلدیست که عبارتی از 0 تا 15 کارکتر را قبول میکند ( `varchar(15)` )

\
\
حال میخواهیم جدول دیگری را برای تمرین بیشتر ایجاد کنیم.

`````````sql
create table P
(
  p# char(5) not null primary key,
  pname varchar(15) not null,
  color varchar(15),
  weight_p float,
  city varchar(15)
)
`````````

در دستور بالا ما نوع `float` را داشتیم که مشخص کننده عددی ۸ بایتی میباشد

------------------------------------------

در دستور ایجاد جدول شما میتوانید ارتباطات فیلد ها را نیز مشخص کنید.
برای مثال ما جدول دیگری در دیتا بیس بالا  ایجاد میکنیم که به جدول P , S متصل است.

`````````sql
create table SP(
  sno char(5) not null,
  pno char(5) not null,
  Qty int not null,
  primary key(sno,pno),
  foreign key (sno) references S
    on delete cascade
    on update cascade,
  foreign key (pno) references P
    on delete cascade
    on update cascade
)
`````````

- همینطور که مشاهده میکنید بعد از تعریف کردن فیلد ها، فیلد `sno` و فیلد `pno` با عبارت `primary key(sno,pno),` به عنوان کلید اصلی شناخته میشوند
- سپس با دستور `foreign key (sno) references S` مشخص میکنیم که `sno` به `S` متصل است
- اگر دقت کنید میبینید که در پایین عباراتی که مشخص کننده ارتباط دو فیلد است عبارات `on delete cascade` و `on update cascade` نیز وجود دارد که نشان دهنده تغیر در زمان پاک شدن و ویرایش شدن میباشد

حال که آموختیم چگونه یک جدول ایجاد کنیم حال باید بیاموزیم چگونه آن را ویرایش کنیم برای ویرایش جداول از فرمت کلی زیر استفاده میکنیم

`````````sql
ALTER TABLE table_name
  {[ALTER COLUMN <column_name>
     { [<schema of new data type>].<new_data_type>
      [(precision [, scale])] max |
  <xml schema collection>
      [COLLATE <collation_name>]
      [NULL|NOT NULL]
      |[{ADD|DROP} ROWGUIDCOL] | PERSISTED}]
    |ADD
      <column name> <data_type>
       [[DEFAULT <constant_expression>]
      |[IDENTITY [(<seed>, <increment>) [NOT FOR REPLICATION]]]]
      [ROWGUIDCOL]
      [COLLATE <collation_name>]
        [NULL|NOT NULL]
      [<column_constraints>]
      |[<column_name> AS <computed_column_expression>]
    |ADD
      [CONSTRAINT <constraint_name>]
      {[{PRIMARY KEY|UNIQUE}
        [CLUSTERED|NONCLUSTERED]
        {(<column_name>[ ,...n ])}
        [WITH FILLFACTOR = <fillfactor>]
        [ON {<filegroup> | DEFAULT}]
        ]
         |FOREIGN KEY
          [(<column_name>[ ,...n])]
          REFERENCES <referenced_table> [(<referenced_column>[ ,...n])]
          [ON DELETE {CASCADE|NO ACTION}]
          [ON UPDATE {CASCADE|NO ACTION}]
           [NOT FOR REPLICATION]
        |DEFAULT <constant_expression>
          [FOR <column_name>]
        |CHECK [NOT FOR REPLICATION]
          (<search_conditions>)
      [,...n][ ,...n]
         |[WITH CHECK|WITH NOCHECK]
    | { ENABLE | DISABLE } TRIGGER
      { ALL | <trigger name> [ ,...n ] }
     |DROP
      {[CONSTRAINT] <constraint_name>
        |COLUMN <column_name>}[ ,...n]
      |{CHECK|NOCHECK} CONSTRAINT
         {ALL|<constraint_name>[ ,...n]}
      |{ENABLE|DISABLE} TRIGGER
        {ALL|<trigger_name>[ ,...n]}
      | SWITCH [ PARTITION <source partition number expression> ]
         TO [ schema_name. ] target_table
        [ PARTITION <target partition number expression> ]
  }
`````````

برای تمرین دستورات زیر را انجام بدهید.

`````````sql
alter table S add codemelli char(10);
alter table S alter column codemelli int;
alter table S drop column codemelli;
`````````

- کلید واژه **add** برای اضافه کردن فیلد.
- کلید واژه **alter** برای ویرایش فیلد.
- کلید واژه **drop** برای حذف فیلد میباشد.

در انتها برای پاک کردن  جدول ساخته شده شما میتوانید از دستور **DROP TABLE** استفاده کنید.

`````````sql
drop table sp;
drop table s;
drop table P;
`````````
