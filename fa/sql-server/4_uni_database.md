# ساخت دیتا بیس uni - mssql-server

==========================================

آزمایشگاه پایگاه داده

------------------------------------------

تهیه شده توسط `محمد خدادادی` زیر نظر `استاد فلاح` در دانشکده `شهید محمد منتظری`

در این قسمت ما دیتا بیس uni را که در کتاب معرفی شده است و تمارین بر اساس این دیتا بیس میباشد را ایجاد میکنیم.

## جدول ها

- جدول **City**
- جدول **groupUni**
- جدول **Student**
- جدول **Teacher**
- جدول **TypeCourse**
- جدول **Course**
- جدول **Grade**

برای سادگی کار ما اسکریپت زیر را که در تمامی ورژن های sql server اجرا میشود را آماده کرده ایم و شما باید آن را در یک کوئری ساده اجرا کنید

`````````sql
/************************************************************************************/
/*********************************  ساخت دیتا بیس  *********************************/
/************************************************************************************/
CREATE DATABASE uni
  COLLATE  Persian_100_CI_AI;
GO
/************************************************************************************/
/**************************** وارد شدن به محیط دیتا بیس  **************************/
/************************************************************************************/
use uni 
GO
/************************************************************************************/
/********************************** ایجاد جدول شهر ها  *****************************/
/************************************************************************************/
CREATE TABLE City
(
  CityCode varchar(4) NOT NULL,
  CityName varchar(30) NOT NULL,
  CONSTRAINT PK_City PRIMARY KEY CLUSTERED( CityCode ASC )
)
GO
/************************************************************************************/
/************************** ایجاد جدول گروه های دانشگاه  **************************/
/************************************************************************************/
CREATE TABLE groupUni
(
  GroupCode varchar(3) NOT NULL,
  GroupName varchar(30) NOT NULL,
  CONSTRAINT PK_Group PRIMARY KEY CLUSTERED ( GroupCode ASC )
)
GO
/**********************************************************************************/
/****************************** ایجاد جدول دانشجو ها  ***************************/
/**********************************************************************************/
CREATE TABLE Student(
  stNo      varchar(10) NOT NULL,
  fname     varchar(20) NOT NULL,
  lname     varchar(20) NOT NULL,
  id1       varchar(10) NULL,
  birthDate datetime NULL,
  sex       bit NULL,
  Age       int NULL,
  GroupCode varchar(3) NOT NULL,
  cityCode  varchar(4) NOT NULL,
  Address   varchar(255) NULL,
  Tel       varchar(11) NULL,
  Notes     varchar(max) NULL,
  picture   image NULL,
  CONSTRAINT PK_Student PRIMARY KEY CLUSTERED ( stNo ASC )
)
GO
/************************************************************************************/
/************************ ایجاد ارتباطات جدول دانشجو ها  **************************/
/************************************************************************************/
ALTER TABLE Student  WITH CHECK ADD  CONSTRAINT FK_Student_City FOREIGN KEY(cityCode)
  REFERENCES City(CityCode)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Student CHECK CONSTRAINT FK_Student_City
GO
ALTER TABLE Student  WITH CHECK ADD  CONSTRAINT FK_Student_Group FOREIGN KEY(GroupCode)
  REFERENCES groupUni (GroupCode)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Student CHECK CONSTRAINT FK_Student_Group
GO
/**********************************************************************************/
/******************************** ایجاد جدول اساتید  *****************************/
/**********************************************************************************/
CREATE TABLE Teacher
(
  teacherCode varchar(10) NOT NULL,
  fname       varchar(20) NOT NULL,
  lname       varchar(25) NOT NULL,
  Rank        varchar(30) NULL,
  Address     varchar(1024) NULL,
  Tel         varchar(11) NULL,
  cityCode    varchar(4) NULL,
  CONSTRAINT PK_Teacher PRIMARY KEY CLUSTERED ( teacherCode ASC )
)
GO
/************************************************************************************/
/************************** ایجاد ارتباطات جدول اساتید  ***************************/
/************************************************************************************/
ALTER TABLE Teacher  WITH CHECK ADD  CONSTRAINT FK_Teacher_City FOREIGN KEY(cityCode)
  REFERENCES City (CityCode)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Teacher CHECK CONSTRAINT FK_Teacher_City
GO
/**********************************************************************************/
/***************************** ایجاد جدول نوع واحد ها  **************************/
/**********************************************************************************/
CREATE TABLE TypeCourse
(
  typeCode varchar(3) NOT NULL,
  Name varchar(30) NOT NULL,
  CONSTRAINT PK_TypeCourse PRIMARY KEY CLUSTERED ( typeCode ASC )
)
GO
/**********************************************************************************/
/******************************** ایجاد جدول درس ها  *****************************/
/**********************************************************************************/
CREATE TABLE Course
(
  CourseNo varchar(6) NOT NULL,
  Name varchar(30) NOT NULL,
  UnitT tinyint NOT NULL,
  UnitP tinyint NOT NULL,
  TypeCode varchar(3) NOT NULL,
  CONSTRAINT PK_Course PRIMARY KEY CLUSTERED ( CourseNo ASC )
)
GO
/************************************************************************************/
/************************** ایجاد ارتباطات جدول درس ها  ***************************/
/************************************************************************************/
ALTER TABLE Course ADD  CONSTRAINT DF_Course_UnitT  DEFAULT ((0)) FOR UnitT
GO
ALTER TABLE Course ADD  CONSTRAINT DF_Course_UnitP  DEFAULT ((0)) FOR UnitP
GO
ALTER TABLE Course  WITH CHECK ADD  CONSTRAINT FK_Course_TypeCourse FOREIGN KEY(TypeCode)
  REFERENCES TypeCourse (typeCode)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Course CHECK CONSTRAINT FK_Course_TypeCourse
GO
/**********************************************************************************/
/******************************* ایجاد جدول نمره ها  *****************************/
/**********************************************************************************/
CREATE TABLE Grade
(
  stNo        varchar(10) NOT NULL,
  Term        varchar(5) NOT NULL,
  CourseNO    varchar(6) NOT NULL,
  Grade       real NOT NULL,
  teacherCode varchar(10) NOT NULL,
  CONSTRAINT PK_Grade PRIMARY KEY CLUSTERED ( stNo ASC, Term ASC,  CourseNO ASC )
)
GO
/************************************************************************************/
/************************ ایجاد ارتباطات جدول نمره ها  ****************************/
/************************************************************************************/
ALTER TABLE Grade  WITH CHECK ADD  CONSTRAINT FK_Grade_Course FOREIGN KEY(CourseNO)
  REFERENCES Course (CourseNo)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Grade CHECK CONSTRAINT FK_Grade_Course
GO
ALTER TABLE Grade  WITH CHECK ADD  CONSTRAINT FK_Grade_Student FOREIGN KEY(stNo)
  REFERENCES Student (stNo)
    ON UPDATE CASCADE
    ON DELETE CASCADE
GO
ALTER TABLE Grade CHECK CONSTRAINT FK_Grade_Student
GO
ALTER TABLE Grade  WITH CHECK ADD  CONSTRAINT FK_Grade_Teacher FOREIGN KEY(teacherCode)
  REFERENCES Teacher (teacherCode)
GO
ALTER TABLE Grade CHECK CONSTRAINT FK_Grade_Teacher
GO
`````````

حال دیتا بیس شما ساخته شده است و میتوانید به آن اطلاعات اضافه کنید ما برای شما دیتا های آماده ای را نیز آماده کرده ایم

`````````sql
/**********************************************************************************************************/
/**********************************************************************************************************/
/***************************************** اطلاعات جدول شهر ها ********************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT City (CityCode, CityName) VALUES (N'011', N'مازندران')
GO
INSERT City (CityCode, CityName) VALUES (N'013', N'گيلان')
GO
INSERT City (CityCode, CityName) VALUES (N'017', N'گلستان')
GO
INSERT City (CityCode, CityName) VALUES (N'021', N'تهران')
GO
INSERT City (CityCode, CityName) VALUES (N'023', N'سمنان')
GO
INSERT City (CityCode, CityName) VALUES (N'024', N'زنجان')
GO
INSERT City (CityCode, CityName) VALUES (N'025', N'قم')
GO
INSERT City (CityCode, CityName) VALUES (N'026', N'البرز')
GO
INSERT City (CityCode, CityName) VALUES (N'028', N'قزوين')
GO
INSERT City (CityCode, CityName) VALUES (N'031', N'اصفهان')
GO
INSERT City (CityCode, CityName) VALUES (N'034', N'کرمان')
GO
INSERT City (CityCode, CityName) VALUES (N'035', N'يزد')
GO
INSERT City (CityCode, CityName) VALUES (N'038', N'چهارمحال بختياري')
GO
INSERT City (CityCode, CityName) VALUES (N'041', N'آذربايجان شرقي')
GO
INSERT City (CityCode, CityName) VALUES (N'044', N'آذربايجان غربي')
GO
INSERT City (CityCode, CityName) VALUES (N'045', N'اردبيل')
GO
INSERT City (CityCode, CityName) VALUES (N'051', N'مشهد')
GO
INSERT City (CityCode, CityName) VALUES (N'054', N'سيستان و بلوچستان')
GO
INSERT City (CityCode, CityName) VALUES (N'056', N'خراسان جنوبي')
GO
INSERT City (CityCode, CityName) VALUES (N'058', N'خراسان شمالي')
GO
INSERT City (CityCode, CityName) VALUES (N'061', N'خوزستان')
GO
INSERT City (CityCode, CityName) VALUES (N'066', N'لرستان')
GO
INSERT City (CityCode, CityName) VALUES (N'071', N'فارس')
GO
INSERT City (CityCode, CityName) VALUES (N'074', N'کهگيلويه و بويراحمد')
GO
INSERT City (CityCode, CityName) VALUES (N'076', N'هرمزگان')
GO
INSERT City (CityCode, CityName) VALUES (N'077', N'بوشهر')
GO
INSERT City (CityCode, CityName) VALUES (N'081', N'همدان')
GO
INSERT City (CityCode, CityName) VALUES (N'083', N'کرمانشاه')
GO
INSERT City (CityCode, CityName) VALUES (N'0841', N'ايلام')
GO
INSERT City (CityCode, CityName) VALUES (N'086', N'مرکزي')
GO
INSERT City (CityCode, CityName) VALUES (N'087', N'سنندج')
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/**************************************** اطلاعات جدول گروه ها ********************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT groupUni (GroupCode, GroupName) VALUES (N'154', N'کامپيوتر_نرم‌افزار')
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/*************************************** اطلاعات جدول دانشجو ها ******************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'1', N'رويا', N'رضواني', N'123', NULL, 0, NULL, N'154', N'051', NULL, N'3265341', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'10', N'رضا', N'رضوي', N'421', NULL, 1, NULL, N'154', N'021', NULL, N'3525555', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'11', N'مينا', N'علي‌نيا', N'125', NULL, 0, NULL, N'154', N'026', NULL, NULL, NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'12', N'عين‌اله', N'اکبرپور', N'621', NULL, 1, NULL, N'154', N'041', NULL, NULL, NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'13', N'سپيده', N'رضواني', N'127', NULL, 0, NULL, N'154', N'013', NULL, N'3600002', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'14', N'زهرا', N'عباسي', N'128', NULL, 0, NULL, N'154', N'035', NULL, N'3260002', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'15', N'کامبيز', N'مسيبي', N'921', NULL, 1, NULL, N'154', N'013', NULL, N'2600004', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'17', N'مجيد', N'حسين‌زاده', N'234', NULL, 1, NULL, N'154', N'031', NULL, NULL, NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'18', N'فتح‌الله', N'محمدي', N'532', NULL, 1, NULL, N'154', N'025', NULL, N'34568254', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'19', N'نيما', N'صديقي', N'236', NULL, 1, NULL, N'154', N'031', NULL, NULL, NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'2', N'علي', N'عليزاده', N'732', NULL, 1, NULL, N'154', N'051', NULL, N'3677742', NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'3', N'محمد', N'محمدي', N'864', NULL, 1, NULL, N'154', N'021', NULL, NULL, NULL, NULL)
GO
INSERT Student (stNo, fname, lname, id1, birthDate, sex, Age, GroupCode, cityCode, Address, Tel, Notes, picture) VALUES (N'4', N'کريم', N'کريمي', N'738', NULL, 1, NULL, N'154', N'051', NULL, NULL, NULL, NULL)
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/***************************************** اطلاعات جدول اساتید ********************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'1', N'علي', N'احمدي', N'دکترا', NULL, N'2277702', N'051')
GO
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'2', N'رضا', N'علوي', N'کارشناسي ارشد', NULL, N'2260771', N'021')
GO
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'3', N'مهران', N'رضايي', N'کارشناسي ارشد', NULL, N'3250660', N'051')
GO
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'4', N'مهدي', N'محمدي', N'دکترا', NULL, N'8081555', N'026')
GO
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'5', N'رضا', N'رضوي', N'کارشناسي ارشد', NULL, N'5646156', N'051')
GO
INSERT Teacher (teacherCode, fname, lname, Rank, Address, Tel, cityCode) VALUES (N'6', N'علي', N'محمدي', N'کارشناسي ارشد', NULL, N'1561625', N'011')
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/**************************************** اطلاعات جدول نوع درس ********************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT TypeCourse (typeCode, Name) VALUES (N'1', N'عمومي')
GO
INSERT TypeCourse (typeCode, Name) VALUES (N'2', N'تخصصي')
GO
INSERT TypeCourse (typeCode, Name) VALUES (N'3', N'اصلي')
GO
INSERT TypeCourse (typeCode, Name) VALUES (N'4', N'پايه')
GO
INSERT TypeCourse (typeCode, Name) VALUES (N'5', N'اختياري')
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/***************************************** اطلاعات جدول درس ها ********************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1292', N'رياضي عمومي', 3, 0, N'4')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1293', N'فيزيك الكتريسيته ومغناطيس', 2, 0, N'4')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1294', N'رياضي كاربردي', 2, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1295', N'آمارواحتمالات', 2, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1296', N'شيوه ارائه نوشتاري وگفتاري', 1, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1297', N'سخت افزاركامپيوتر 2', 2, 1, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1298', N'مباني الكترونيك', 2, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1299', N'كارگاه مباني الكترونيك', 1, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1300', N'زبان فني', 2, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1301', N'آزمايشگاه نرم‌افزارهاي گرافيکي', 1, 0, N'3')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1302', N'سيستم عامل  2', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1303', N'كارگاه سيستم عامل  2', 1, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1304', N'برنامه سازي پيشرفته  1', 2, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1305', N'شبكه هاي محلي كامپيوتري', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1306', N'كارگاه شبكه هاي محلي كامپيوتري', 1, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1307', N'زبان ماشين واسمبلي', 2, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1308', N'برنامه سازي پيشرفته  2', 2, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1309', N'مباني مهندسي نرم افزار', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1310', N'پايگاه داده ها', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1311', N'آزمايشگاه پايگاه داده ها', 1, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1312', N'ذخيره وبازيابي اطلاعات', 3, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1313', N'مباني اينترنت', 1, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1314', N'ساختمان داده ها', 2, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1315', N'برنامه نويسي مبتني بروب', 1, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1316', N'مباحث ويژه', 1, 1, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1317', N'اصول سرپرستي', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1318', N'پروژه', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1320', N'كارآموزي', 2, 0, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1321', N'محيطهاي چندرسانه اي', 1, 1, N'5')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1322', N'نرم افزار رياضي وآمار', 1, 1, N'5')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'1323', N'گرافيک کامپيوتري', 1, 1, N'5')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9101', N'زبان خارجي', 3, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9102', N'انديشه اسلامي(1) (مبدأ و معاد)', 2, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9108', N'آيين زندگي (اخلاق کاربردي)', 2, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9118', N'زبان فارسي', 3, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9120', N'تربيت بدني 1', 1, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9126', N'كارآفريني', 1, 2, N'2')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9128', N'دانش خانواده و جمعيت', 2, 0, N'1')
GO
INSERT Course (CourseNo, Name, UnitT, UnitP, TypeCode) VALUES (N'9129', N'آشنايي با ارزشهاي دفاع مقدس', 2, 0, N'5')
GO
/**********************************************************************************************************/
/**********************************************************************************************************/
/**************************************** اطلاعات جدول نمره ها *******************************************/
/**********************************************************************************************************/
/**********************************************************************************************************/
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'1', N'2', N'1292', 12, N'1')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'1', N'2', N'1304', 17, N'4')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'10', N'1', N'1292', 18, N'1')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'10', N'1', N'1304', 16, N'4')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'10', N'2', N'1293', 8, N'2')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'10', N'2', N'1313', 10, N'3')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'18', N'1', N'1292', 15, N'1')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'18', N'1', N'1295', 12, N'1')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'18', N'1', N'1304', 16, N'4')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'2', N'3', N'1303', 19, N'2')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'2', N'3', N'1312', 9, N'3')
GO
INSERT Grade (stNo, Term, CourseNO, Grade, teacherCode) VALUES (N'2', N'4', N'1297', 14, N'3')
GO
`````````

در درس های آینده از این دیتا بیس برای آموزش استفاده میشود پس لطفا آن را ایجاد کنید.
