# صـفوف (Rows)
[[English]](readme.md)

مكتبة للتعامل مع قواعد البيانات وتحقيق ال ORM (ربط الأغراض العلائقي) للغة الأسس.

## الإضافة إلى المشروع

يمكن تثبيت هذه المكتبة باستعمال التعليمات البرمجية التالية:

<div dir=rtl>

```
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "<المشغل>" })؛
```

</div>

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "<driver>" });
```

اسماء الملفات في المعطى الثاني يجب أن تشمل اسم المشغل المطلوب لقاعدة البيانات الخاصة بك. ما لم تكن
تنوي كتابة مشغلك الخاص فإنك ستحتاج لشمول أحد المشغلات المتوفرة في مكتبة صـفوف. مكتبة صـفوف لا تشمل
المشغلات تلقائيًا لأن كل مشغل يتطلب المكتبة الخاصة بقاعدة البيانات المعنية. مثلا، مشغل بوستغريس
يتطلب توفر مكتبة "libpq" في النظام بينما يتطلب مشغل مايسكويل مكتبة "libmysqlclient"، فتُرك شمول
المشغلات للمستخدم كي لا نجبره على تنصيب مكتبات لا يحتاج إليها في نظامه. 

### قاعدة بيانات بوستغرس (PostgreSQL)

<div dir=rtl>

```
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "مـشغلات/بـوستغرس.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_بوستغرس(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛
```

</div>

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Postgresql.alusus" });
use Rows;
def db: Db(PostgresqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

### قاعدة بيانات مايسكويل (MySQL)

<div dir=rtl>

```
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_مايسكويل(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛
```

</div>

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Rows;
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

### قاعدة بيانات سكويلايت (Sqlite)

<div dir=rtl>

```
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "مـشغلات/سـكويلايت.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_سكويلايت(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "الأسس.قب"؛
}))؛
```

</div>

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Sqlite.alusus" });
use Rows;
def db: Db(SqliteDriver(ConnectionParams().{
    dbName = "alusus.db";
}));
```

## مثال

### التعامل اليدوي مع قاعدة البيانات

<div dir=rtl>

```
اشمل "مـتم/نـظام"؛
اشمل "مـتم/نـص"؛
اشمل "مـتم/مـصفوفة"؛
اشمل "مـتم/تـطبيق"؛
اشمل "مـتم/سندات"؛
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
استخدم مـتم؛
استخدم صـفوف؛

// متغير من الصنف قـاعدة_البيانات و نضع ضمنه معلومات قاعدة البيانات.
عرف قب: قـاعدة_بيانات(مـشغل_مايسكويل(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛

// نتحقق فيما إذا كان الاتصال قد تم.
إذا !قب.أمتصل() {
    نـظام.فشل(1, نـص("فشل الاتصال بقاعدة البيانات: ") + قب.هات_آخر_خطأ())؛
}

// ننشئ جدولا

إذا !!قب.نفذ(إنـشاء_جدول().{
    // اسم الجدول
    الاسم = نـص("users")؛
    // نريد إنشاءه فقط في حال لم يكن موجود مسبقاً
    جديد_فقط = 1؛
    // معلومات الحقول
    الحقول = تـطبيق[نـص, سـندنا[حـقل]]().{
        حدد(نـص("id"), حـقل().{
            الصنف = عـدد_صحيح()؛
            إلزامي = 1؛
            فريد = 1؛
        })؛
        حدد(نـص("name"), حـقل().{
            الصنف = مـحارف_مرنة(90)؛
            إلزامي = 1؛
            فريد = 1؛
            القيمة_الافتراضية = نـص("me")؛
        })؛
        حدد(نـص("address"), حـقل().{
            الصنف = مـحارف_مرنة(200)؛
        })؛
    }؛
    // الفهرس الرئيسي
    الفهرس_الرئيسي = نـص("id")؛
}) {
    نـظام.فشل(1, نـص("فشلت العملية: ") + قب.هات_آخر_خطأ())؛
}

// ننشئ جدولا آخر

// هذا الجدول مرتبط بالجدول الأول عبر فهرس_وصل
عرف نتيجة: نـتيجة = قب.نفذ(إنـشاء_جدول().{
    الاسم = نـص("roles")؛
    جديد_فقط = 1؛
    الحقول = تـطبيق[نـص, سـندنا[حـقل]]().{
        حدد(نـص("id"), حـقل().{
            الصنف = عـدد_صحيح()؛
            إلزامي = 1؛
            فريد = 1؛
        })؛
        حدد(نـص("name"), حـقل().{
            الصنف = مـحارف_مرنة(90)؛
            إلزامي = 1؛
            فريد = 1؛
            القيمة_الافتراضية = نـص("viewer")؛
        })؛
        حدد(نـص("userId"), حـقل().{
            الصنف = عـدد_صحيح()؛
        })؛
    }؛
    الفهرس_الرئيسي = نـص("id")؛
    فهارس_الوصل = مـصفوفة[سـندنا[فـهرس_وصل]]({
        فـهرس_وصل(نـص("userId"), نـص("users"), نـص("id"))
    })؛
})؛
إذا ليس نتيجة {
    نـظام.فشل(1, نـص("فشلت العملية: ") + نتيجة.خطأ)؛
}

// ندخل بعض البيانات

قب.نفذ("delete from users")؛

عرف أسماء: مـصفوفة[نـص]({ نـص("سرمد"), نـص("سليمان"), نـص("عبد الله"), نـص("سالم") })؛
عرف عناوين: مـصفوفة[نـص]({ نـص("كندا"), نـص("سوريا"), نـص("السعودية"), نـص("السعودية") })؛

عرف ع: صحيح؛
لكل ع = 0, ع < أسماء.هات_الطول(), ++ع {
    إذا !!قب.نفذ(إدخـال().{
        الجدول = نـص("users")؛
        الحقول = مـصفوفة[نـص]({ نـص("id"), نـص("name"), نـص("address") })؛
        البيانات = مـصفوفة[نـص]({ نـص("15") + ع, أسماء(ع), عناوين(ع)  })؛
    }) {
        نـظام.فشل(1, نـص("فشلت العملية: ") + قب.هات_آخر_خطأ())؛
    }
}

// نقرأ البيانات

عرف بيانات: مـصفوفة[مـصفوفة[نـص]]؛
عرف نتيجة: نـتيجة[مـصفوفة[مـصفوفة[نـص]]]؛
نتيجة = قب.نفذ(جـلب().{
    الجدول = نـص("users")؛
})؛
إذا !!نتيجة {
    نـظام.فشل(1, نـص("فشلت العملية: ") + نتيجة.خطأ)؛
}
بيانات = نتيجة؛

عرف ص: صحيح = 0؛
عرف ع: صحيح = 0؛
لكل ص = 0, ص < بيانات.هات_الطول(), ص = ص + 1 {
    لكل ع = 0, ع < بيانات(ص).هات_الطول(), ع = ع + 1 {
        طـرفية.اطبع(بيانات(ص)(ع))؛
        طـرفية.اطبع("    ")؛
    }
    طـرفية.اطبع("\ج")؛
}
```

</div>

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Srl;
use Rows;

// متغير من الصنف قـاعدة_البيانات و نضع ضمنه معلومات قاعدة البيانات.
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));

// نقوم بالتحقق فيما إذا كان الاتصال قد تم.
if !db.driver.isConnected() {
    System.fail(1, String("Error connecting to DB: ") + db.getLastError());
}

// ننشئ جدولا
if !!db.exec(CreateTable().{
    name = String("users");  // اسم الجدول
    notExists = true;  // نريد إنشاءه فقط في حال لم يكن موجود مسبقاً
    // معلومات الحقول
    columns = Map[String, SrdRef[Column]]().{
        set(String("id"), Column().{
            dataType = Integer();
            notNull = true;
            unique = true;
        });
        set(String("name"), Column().{
            dataType = VarChar(90);
            notNull = true;
            unique = true;
            default = String("me");
        });
        set(String("address"), Column().{
            dataType = VarChar(200);
        });
    };
    primaryKey = String("id");  // الفهرس الرئيسي
}) {
    System.fail(1, String("Query failed: ") + db.getLastError());
}

// ننشئ جدولا آخر
// هذا الجدول مرتبط بالجدول الأول عبر فهرس_وصل
def res: Result = db.exec(CreateTable().{
    name = String("roles");
    notExists = true;
    columns = Map[String, SrdRef[Column]]().{
        set(String("id"), Column().{
            dataType = Integer();
            notNull = true;
            unique = true;
        });
        set(String("name"), Column().{
            dataType = VarChar(90);
            notNull = true;
            unique = true;
            default = String("viewer");
        });
        set(String("userId"), Column().{
            dataType = Integer();
        });
    };
    primaryKey = String("id");
    foreignKeys = Array[SrdRef[ForeignKey]]({
        ForeignKey(String("userId"), String("users"), String("id"))
    });
});
if not res {
    System.fail(1, String("Query failed: ") + res.error);
}

// ندخل بعض البيانات

db.exec("delete from roles");
db.exec("delete from users");

def names: Array[String]({ String("Sarmad"), String("Sleman"), String("Abdullah"), String("Salem") });
def addresses: Array[String]({ String("Canada"), String("Syria"), String("KSA"), String("KSA") });

def i: Int;
for i = 0, i < names.getLength(), ++i {
    def res: Result = db.exec(Insert().{
        table = String("users");
        columns = Array[String]({ String("id"), String("name"), String("address") });
        data = Array[String]({ String("15") + i, names(i), addresses(i)  });
    });
    if !!res {
        System.fail(1, String("Query failed: ") + res.error);
    }
}

// نقرأ البيانات

def data: Array[Array[String]];
def res: Result[Array[Array[String]]];
res = db.exec(Select().{
    table = String("users");
});
if !!res {
    System.fail(1, String("Query failed: ") + res.error);
}
data = res;

def i: int = 0;
def j: int = 0;
for i = 0, i < data.getLength(), i = i + 1 {
    for j = 0, j < data(i).getLength(), j = j + 1 {
        Console.print(data(i)(j));
        Console.print("    ");
    }
    Console.print("\n");
}

```


### استعمال خاصية المطابقة (ORM)

<div dir=rtl>

```
اشمل "مـتم/نـص"؛
اشمل "مـتم/نـص"؛
اشمل "مـتم/مـصفوفة"؛
اشمل "مـتم/طـرفية"؛
اشمل "مـتم/نـظام"؛
اشمل "مـتم/سندات"؛
اشمل "مـحا"؛
مـحا.اشمل_ملف("Alusus/Rows"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
استخدم مـتم؛
استخدم صـفوف؛

// نقوم بتحديد ما يقابل الجدول هنا، أي أننا نريد ما يلي في جدول باسم Cars
@جدول["سيارات"]
صنف سـيارة {
    عرف_أساسيات_الجدول[]؛

    // عمود المعرف
    // مبدل قيمة غير فارغة
    @إلزامي 
    // مبدل الفهرس الرئيسي
    @فهرس_رئيسي
    // مبدل عدد صحيح، لتحديد أن الحقل هنا له النمط عدد صحيح،
    @عـدد_صحيح
    // مبدل يحدد أنه لدينا حقل
    @حقل
    عرف المعرف: صحيح؛

    // اسم السيارة
    // مبدل لتحديد أن الحقل هنا هو سلسلة من المحارف متغيرة الطول بطول أعظمي 50
    @مـحارف_مرنة["50"]
    //  مبدل لتحديد القيمة الافتراضية في حال عدم وجود قيمة
    @مبدئي["سيارتي"]
    @حقل
    عرف الاسم: نـص؛

    // مبدل لتحديد أن قيم الحقل هي من النمط عـائم
    @عـدد_عائم
    @حقل["سعر_السيارة"]
    عرف السعر: عائم؛
}

// نقوم بالاتصال بقاعدة البيانات
عرف قب: قـاعدة_بيانات(مـشغل_مايسكويل(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛
إذا !قب.أمتصل() {
    نـظام.فشل(1، نـص("فشل الاتصال بقاعدة البيانات: ") + قب.هات_آخر_خطأ())؛
}

// نقوم باستدعاء `مهيكل` على الصنف السابق ليقوم ببناء الجدول 
// بناءً على ما حددناه سابقاً بواسطة المبدلات
قب.مهيكل[سـيارة].أنشئ()؛

// نقوم بحذف أي بيانات موجودة مسبقاً (في حال نفذنا الكود أكثر من مرة)
قب.من[سـيارة].احذف()؛

عرف ع: صحيح=1
لكل ع = 1، ع < 30، ع = ع+1 {
    عرف س: سـيارة؛
    س.المعرف = ع
    س.الاسم = نـص("سيارة ") + ع؛
    س.السعر = 200.0 * ع
    // بعد إعطاء قيم للغرض السابق نقوم بإستدعاء الدالة `احفظ` لإضافة سطر إلى الجدول 
    قب.احفظ[سـيارة](س)؛
}
طـرفية.اطبع("\ج")؛

// نقوم بطباعة محتويات الجدول للتأكد أن كل شيء كما نريد.
دالة اطبع_حقولا(ح: مـصفوفة[سـندنا[سـيارة]]) {
    عرف ك: صحيح = 0؛
    لكل ك = 0، ك < ح.هات_الطول()، ك = ك+1 {
        طـرفية.اطبع(ح(ك).المعرف)؛
        طـرفية.اطبع("\t\t")؛
        طـرفية.اطبع(ح(ك).الاسم)؛
        طـرفية.اطبع("\t\t")؛
        طـرفية.اطبع(ح(ك).السعر)؛
        طـرفية.اطبع("\ج")؛
    }
}
```

</div>

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Srl;
use Rows;

// Cars نقوم بتحديد ما يقابل الجدول هنا، أي أننا نريد ما يلي في جدول باسم
@model["cars"]
class Car {
    define_model_essentials[];

    // عمود المعرف
    @notNull  // مبدل قيمة غير فارغة
    @primaryKey  // مبدل الفهرس الرئيسي
    @Integer  // مبدل عدد صحيح، لتحديد أن الحقل هنا له النمط عدد صحيح،
    @column  // مبدل يحدد أنه لدينا حقل
    def id: Int;

    // اسم السيارة 
    @VarChar["50"]  // مبدل لتحديد أن الحقل هنا هو سلسلة من المحارف متغيرة الطول بطول أعظمي 50
    @defult["My Car"]  //  مبدل لتحديد القيمة الافتراضية في حال عدم وجود قيمة
    @column
    def name: String;

    @Float  // مبدل لتحديد أن قيم الحقل هي من النمط عـائم
    @column
    def price: Float;
}

// نقوم بالاتصال بقاعدة البيانات
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
if !db.isConnected() {
    System.fail(1, String("Error connecting to DB: ") + db.getLastError());
}

// على الصنف السابق ليقوم ببناء الجدول schemaBuilder نقوم باستدعاء 
// بناءً على ما حددناه سابقاً بواسطة المبدلات
db.schemaBuilder[Car].create();

// نقوم بحذف أي بيانات موجودة مسبقاً (في حال نفذنا الكود أكثر من مرة)
db.from[Car].delete();

def i: int=1
for i = 1, i < 30, i = i+1 {
    def c: Car;
    c.id = i
    c.name = String("Car ") + i;
    c.price = 200.0 * i
    // لإضافة سطر إلى الجدول save بعد إعطاء قيم للغرض السابق نقوم بإستدعاء الدالة
    db.save[Car](c);
}
Console.print("\n");


// نقوم بطباعة محتويات الجدول للتأكد أن كل شيء كما نريد.
func printRows (r: Array[SrdRef[Car]]) {
    def k: int=0;
    for k = 0, k < r.getLength(), k = k+1 {
        Console.print(r(k).id);
        Console.print("\t\t");
        Console.print(r(k).name);
        Console.print("\t\t");
        Console.print(r(k).price);
        Console.print("\n");
    }
}

```

## خاصية المطابقة

يمكن لمكتبة صفوف مطابقة جداول قاعدة البيانات مع أصناف المستخدم للتحويل التلقائي من بيانات الجدول
إلى صنف المستخدم أو العكس. لتمكين هذه الخاصية يحتاج المستخدم لتعديل أصنافه كما يلي:

* إضافة المبدل `@جدول` (`@model`) إلى الصنف مع تحديد اسم الجدول كمعطى للمبدل.
* إضافة `عرف_أساسيات_الجدول[]` (`define_model_essentials`) في بداية صنف المستخدم.
* إضافة المبدل `@حقل` (`@column`) للمتغيرات المقابلة للحقول مع تحديد اسم الحقل كمعطى للمبدل.
* إضافة مبدل يقابل صنف بيانات الحقل للمتغيرات.
* إضافة المبدلات المتعلقة بالفهرسة والقيمة المبدئية والحقول المطلوبة إن وجد.

مثال:

<div dir=rtl>

```
@جدول["سيارات"]
صنف سـيارة {
    عرف_أساسيات_الجدول[]؛

    @إلزامي
    @فهرس_رئيسي
    @عـدد_صحيح
    @حقل
    عرف المعرف: صحيح؛

    @مـحارف_مرنة["50"]
    @مبدئي["سيارتي"]
    @حقل
    عرف الاسم: نـص؛

    @عـدد_عائم
    @حقل["سعر_السيارة"]
    عرف السعر: عائم؛
}
```

</div>

```
@model["cars"]
class Car {
    define_model_essentials[];

    @notNull
    @primaryKey
    @Integer
    @column
    def id: int;

    @VarChar["50"]
    @defult["My Car"]
    @column
    def name: String;

    @Float
    @column["the_price"]
    def price: Float;

    @VarChar["50"]
    @column
    def fullName: String;
}
```

بعد تعريف الصنف سيمكنك استخدامه في الدالات التي تتعامل مع الأصناف لقراءة وكتابة البيانات، مثل
دالة `قـاعدة_البيانات.من` (`Db.from`) أو دالة `قـاعدة_بيانات.احفظ` (`Db.save`) أو
دالة `مـهيكل.أنشئ` (`SchemaBuilder.create`).

### مبدلات الجداول والحقول

* `@جدول` (`@model`)
* `@حقل` (`@column`)
* `@فهرس_رئيسي` (`@primaryKey`)
* `@فهرس_وصل` (`@foreignKey`)
* `@إلزامي` (`@notNull`)
* `@فريد` (`@unique`)
* `@مبدئي` (`@default`): لتحديد القيمة المبدئية للحقل.
* `@تحقق` (`@check`): لتحديد شرط يطبق على الحقل.

### مبدلات أصناف البيانات

* `@عـدد_صحيح` (`@Integer`)
* `@صـحيح_كبير` (`@BitInteger`)
* `@صـحيح_صغير` (`@SmallInteger`)
* `@صـحيح_ضئيل` (`@TinyInteger`)
* `@عـدد_حقيقي` (`@Real`)
* `@عـدد_عائم` (`@Float`)
* `@عـدد_عشري` (`@Decimal`)
* `@إكـسمل` (`@Xml`)
* `@مـحارف_مرنة` (`@VarChar`)
* `@مـحارف_محددة` (`@CharType`)
* `@نـص_كبير` (`@Text`)
* `@تـاريخ` (`@Date`)

## الأصناف والتوابع

### الصنف مـعطيات_الاتصال (ConnectionParams)

<div dir=rtl>

```
صنف مـعطيات_الاتصال {
    عرف اسم_قاعدة_البيانات: نـص = ""؛
    عرف اسم_المستخدم: نـص = ""؛
    عرف كلمة_السر: نـص = ""؛
    عرف عنوان_الخادم: نـص = ""؛
    عرف المنفذ: نـص = ""؛
}
```

</div>

```
class ConnectionParams {
    def dbName: String = "";
    def userName: String = "";
    def password: String = "";
    def host: String = "";
    def port: int = 0;
}
```
يحتوي هذا الصنف على المعلومات اللازمة للاتصال بقاعدة البيانات.

`اسم_قاعدة_البيانات` (`dbName`) اسم قاعدة البيانات التي نرغب بالاتصال بها.

`اسم_المستخدم` (`userName`) اسم المستخدم الخاص بحسابنا في قاعدة البيانات.

`كلمة_السر` (`password`) كلمة سر الحساب الخاص بنا في قاعدة البيانات.

`عنوان_الخادم` (`host`) عنوان الخادم الذي تعمل قاعدة البيانات عليه.

`المنفذ` (`port`) المنفذ في الخادم الذي عن طريقه يمكن الوصول إلى قاعدة البيانات.

### الصنف إنـشاء جدول (CreateTable)

<div dir=rtl>

```
صنف إنـشاء_جدول {
    عرف الاسم = نـص؛
    عرف جديد_فقط = ثـنائي؛
    عرف الحقول = تـطبيق[نـص، سـندنا[حـقل]]؛
    عرف الفهرس_الرئيسي = مـصفوفة[نـص]؛
    عرف فهارس_الوصل = مصفوفة[سـندنا[فـهرس_وصل]]؛
}
```

</div>

```
class CreateTable {
    handler this.name = String;
    handler this.notExists = Bool;
    handler this.columns = Map[String, SrdRef[Column]];
    handler this.primaryKey = Array[String];
    handler this.foreignKeys = Array[SrdRef[ForeignKey]];
}
```
صنف يستعمل لإنشاء جدول حسب المعلومات التي يحويها.

`الاسم` (`name`) اسم الجدول

`جديد_فقط` (`notExists`) متغير يحدد فيما إذا كنا نريد إنشاء الجدول فقط في حال لم يكن موجود مسبقاً.

`الحقول` (`columns`) تـطبيق يربط بين اسم حقل و المعلومات الخاص به ضمن الصنف `Column`.

`الفهرس_الرئيسي` (`primaryKey`) الفهرس الرئيسي للجدول.

`فهارس_الوصل` (`foreignKeys`) فهارس الوصل و هي تعرف ارتباطات الجدول مع الجداول الأخرى.


### الصنف حـذف (Delete)

<div dir=rtl>

```
صنف حـذف {
    عرف الجدول = نـص؛
    عرف الشرط(عبارة: مؤشر[مـحرف]، معطيات: ...أي_معطيات_أخرى)؛
}
```

</div>

```
class Delete {
    handler this.table = String;
    handler this.condition(statement: CharsPtr, args: ...any);
}
```
صنف يستعمل لحذف أسطر من جدول بناء على شرط معين.

`الجدول` (`table`) اسم الجدول.

`الشرط` (`condition`) الشرط الذي تم الحذف على أساسه.

### الصنف إدخـال (Insert)

<div dir=rtl>

```
صنف إدخـال {
    عرف الجدول = نـض؛
    عرف البيانات = مـصفوفة[نـص]؛
    عرف الحقول = مـصفوفة[نـص]؛
}
```

</div>

```
class Insert {
    handler this.table = String;
    handler this.data = Array[String];
    handler this.columns = Array[String];
}
```
يستعمل هذا الصنف لإضافة سطر إلى جدول.

`الجدول` (`table`) اسم الجدول الذي نريد الإضافة إليه.

`البيانات` (`data`) البيانات الخاصة بالسطر المراد إضافته.

`الحقول` (`columns`) أسماء الأعمدة التي تتبع لها القيم في `البيانات`.


### الصنف جـلب (Select)

<div dir=rtl>

```
 صنف جـلب {
    عرف الجدول = مـصفوفة[نـص]؛
    عرف الحقول = مـصفوفة[نـص]؛
    عرف الشرط(عبارة: مؤشر[مـحرف]، معطيات: ...أي_معطيات_أخرى)؛
    عرف الترتيب = مـصفوفة[نـص]؛
}
```

</div>

```
class Select {
    handler this.table = Array[String];
    handler this.fields = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
    handler this.orderBy = Array[String];
}
```
يستعمل هذا الصنف لجلب أسطر من جدول في قاعدة البيانات.

`الاسم` (`name`) اسم الجدول المراد جلب البيانات منه.

`الحقول` (`columns`) أسماء الأعمدة التي نرغب بجلب قيمها.

`الشرط` (`condition`) الشرط الذي نريد للأسطر أن تحققه حتى يتم جلبها.

`الترتيب` (`orderBy`) الترتيب المراد تطبيقه على الأسطر.

### الصنف تـحديث (Update)

<div dir=rtl>

```
صنف تـحديث {
    عرف الجدول = نـص؛
    عرف البيانات = مـصفوفة[نـص]؛
    عرف الحقول = مـصفوفة[نـص]؛
    عرف الشرط(عبارة: مؤشر[مـحرف]، معطيات: ...أي_معطيات_أخرى)؛
}
```

</div>

```
class Update {
    handler this.table = String;
    handler this.data = Array[String];
    handler this.columns = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
}
```

يستعمل هذا الصنف لتحديث سطر أو مجموعة أسطر في جدول في قاعدة البيانات.

`الاسم` (`name`) اسم الجدول المراد تحديث أسطر فيه.

`البيانات` (`data`) القيم الجديدة التي نريد تحديث الأسطر بها.

`الحقول` (`columns`) الأعمدة التي نريد تحديث قيمها حسب القيم في `البيانات`.

`الشرط` (`condition`) الشرط الذي يجب على السطر تحقيقه حتى يتم تحديثه.

### الصنف حـقل (Column)

<div dir=rtl>

```
صنف حـقل {
    عرف الصنف = سـندنا[نـمط_البيانات]؛
    عرف إلزامي = ثـنائي؛
    عرف فريد = ثـنائي؛
    عرف القيمة_الافتراضية = نـص؛
    عرف التحقق(عبارة: مؤشر[مـحرف]، معطيات: ...أي_معطيات_أخرى)؛
}
```

</div>

```
class Column {
    handler this.dataType = SrdRef[DataType];
    handler this.notNull = Bool;
    handler this.unique = Bool;
    handler this.default = String;
    handler this.check(statement: CharsPtr, args: ...any);
}
```

يحمل هذا الصنف المعلومات الخاصة بالحقل.

`الصنف` (`dataType`) نمط البيانات التي سيتم تخزينها في هذا الحقل.

`إلزامي` (`notNull`) هل من الضروري وجود قيمة لهذا الحقل أم من الممكن أن يكون خالٍ.

`فريد` (`unique`) هل قيم هذا الحقل يحب أن تكون فريدة أم من الممكن أن يحمل سطران نفس القيمة في هذا العمود.

`القيمة_الافتراضية` (`default`) القيمة الافتراضية لهذا الحقل في حال لم يتم إعطاء قيمة.

`التحقق` (`check`) التحقق الذي يجب تطبيقه قبل قبول قيمة لهذا الحقل.

### الصنف اسـتعلام (Query)

<div dir=rtl>

```
صنف اسـتعلام {
    عرف رتب: سند[نمط_هذا]؛
    عرف حيثما: ماكرو[هذا، شرط]؛
    عرف حدث: ماكرو[هذا، عبارة]؛
    عرف اجلب(): نـتيجة[مـصفوفة[سـندنا[جـدول]]]؛
    عرف احفظ(جدول: سند[جـدول]): نـتيجة؛
    عرف احذف(جدول: سند[جـدول]): نـتيجة؛
}
```

</div>

```
class Query [Model: type] {
    handler [exp: ast] this.order:ref[this_type];
    @member macro where [this, condition];
    @member macro update [this, expression];
    handler this.select(): Result[Array[SrdRef[Model]]];
    handler this.save(model: ref[Model]): Result;
    handler this.delete(model: ref[Model]): Result;
}
```

`رتب` (`order`) يستعمل لوضع الترتيب الخاص باستعلام.

`حيثما` (`where`) ماكرو يستعمل لوضع الشرط الخاص باستعلام.

المعطيات:

* `هذا` (`this`) سند إلى غرض من النمط استعلام.
* `شرط` (`condition`) الشرط المطلوب تطبيقه.

`حدث` (`update`) ماكرو يستعمل لتنفيذ تعليمة تحديث على جدول.

المعطيات:

* `هذا` (`this`) سند إلى غرض من النمط استعلام.
* `عبارة` (`expression`) العبارة الخاصة بالتحديث.

`اجلب` (`select`) يستعمل لجلب أسطر جدول ما.

`احفظ` (`save`) يستعمل لحفظ سطر ما في الجدول.

المعطيات:

* `جدول` (`model`) سند إلى الجدول المراد حفظ السطر فيه.

`احذف` (`delete`) يستعمل للحذف من الجدول.

المعطيات:

* `جدول` (`model`) سند إلى الجدول المراد حذف السطر فيه.

يستخدم هذا الصنف لتمكين جمع الخصائص المختلفة للاستعلام قبل تنفيذه، وفي العادة لا يحتاج المستخدم
لإنشاء كائنات من هذا الصنف يدويًا إنما باستخدام `قـاعدة_بيانات.من` (`Db.from`). مثلا:

<div dir=rtl>

```
قب.من[مـستخدم].حيثما[الاسم = معطى1].حدث[العنوان = معطى2]؛
```

</div>

```
db.from[User].where[name = arg1].update[address = arg2];
```
 
### الصنف مـهيكل (SchemaBuilder)

<div dir=rtl>

```
صنف مـهيكل {
    عرف أنشئ(): نـتيجة؛
}
```

</div>

```
class SchemaBuilder [Model: type] {
    handler this.create(): Result;
}
```

يستعمل هذا الصنف كقالب لإنشاء الجداول. معطى القالب صنف جدول معلّم بالمبدلات المطلوبة كما هو مذكور
في فصل المطابقة.

`أنشئ` (`create`) إنشاء جدول من صنف المستخدم.

### الصنف قـاعدة_البيانات (Db)

<div dir=rtl>

```
صنف قـاعدة_بيانات {
    عرف أمتصل(): ثـنائي؛
    عرف هات_آخر_خطأ(): نـص؛
    عرف نفذ(اجلب: سند[اجـلب]): نـتيجة[مـصفوفة[مـصفوفة[نـص]]]؛
    عرف نفذ(تحديث: سند[تـحديث]): نـتيجة؛
    عرف نفذ(أدخل: سند[احـشر]): نـتيجة؛
    عرف نفذ(احذف: سند[احـذف]): نـتيجة؛
    عرف نفذ(أنشئ_جـدول: سند[أنـشئ_جدول]): نـتيجة؛
    عرف نفذ(عبارة: مؤشر[مـحرف]، معطيات: ...أي_معطيات_أخرى): نـتيجة؛
    عرف [جـدول: نمط] من: اسـتعلام[جـدول]؛
    عرف [جـدول: نمط] احفظ(جدول: سند[جـدول])؛
    عرف [جـدول: نمط] مهيكل: مـهيكل[جـدول]؛
}
```

</div>

```
class Db {
    handler this.isConnected(): Bool;
    handler this.getLastError(): String;
    handler this.exec(select: ref[Select]): Result[Array[Array[String]]];
    handler this.exec(update: ref[Update]): Result;
    handler this.exec(insert: ref[Insert]): Result;
    handler this.exec(delete: ref[Delete]): Result;
    handler this.exec(createTable: ref[CreateTable]): Result;
    handler this.exec(statement: CharsPtr, args: ...any): Result;
    handler [Model: type] this.from: Query[Model];
    handler [Model: type] this.save(model: ref[Model]);
    handler [Model: type] this.schemaBuilder: SchemaBuilder[Model];
}
```

يستعمل هذا الصنف لتنظيم الوصول إلى قاعدة البيانات و تنفيذ مختلف العمليات عليها.

`أمتصل` (`isConnected`) يستعمل للتحقق فيما إذا كان هناك اتصال مع قاعدة البيانات.

`هات_آخر_خطأ` (`getLastError`) يستعمل لجلب آخر خطأ.

`نفذ` (`exec`) يستعمل لتنفيذ استعلام ما، و له نسخة لكل نوع من أنواع الاستعلامات بالإضافة
إلى نسخة لتنفيذ عبارات SQL خام. نسخة تنفيذ عبارات SQL تفصل الهيكل الرئيسي لعبارة SQL
عن المعطيات التي يمررها المستخدم كمعطيات لاحظة لدالة `نفذ` بطريقة مشابهة لعمل دالة
الطباعة. تستبدل الدالة الرموز البادئة ب% في SQL بقيمة من قائمة المعطيات تطابق الصنف
المحدد. الرمز % يتبعه محرف يدل على صنف القيمة المعطاة، وهي كالتالي:
* نـص (String): %s
* مـؤشر_محارف (CharsPtr): %p
* صـحيح (Int): %i
* صـحيح[64] (Int[64]): %l
* عـائم (Float): %f
* عـائم[64] (Float[64]): %d
* مـصفوفة[نـص] (Array[String]): %as
* مـصفوفة[مـؤشر_محارف] (Array[CharsPtr]): %ap
* مـصفوفة[صـحيح] (Array[Int]): %ai
* مـصفوفة[صـحيح[64]] (Array[Int[64]]): %al
* مـصفوفة[عـائم] (Array[Float]): %af
* مـصفوفة[عـائم[64]] (Array[Float[64]]): %ad

آخر نسخة من هذه الدالة تقوم باستقبال شفرة sql خام و تقوم بتنفيذ هذه الشفرة.
من المفيد وجود هذه الدالة لتنفيذ الشفرات المعقدة.

`من` (`from`) يستعمل لإعادة استعلام بناء على المعلومات في هذا الصنف.

`احفظ` (`save`) يستعمل لاستدعاء الوظيفة `احفظ` في الصنف `اسـتعلام`.

`مهيكل` (`schemaBuilder`) يستعمل لإعادة مهيكل بناء على المعلومات في هذا الصنف.

### الصنف نـتيجة (Result)

<div dir=rtl>

```
صنف نـتيجة {
    عرف بيانات: نـمط_البيانات؛
    عرف خطأ: نـص؛
    عرف نجاح(بيانات: سند[نـمط_البيانات]): نـتيجة[نـمط_البيانات]؛
    عرف نجاح(): نـتيجة[نـمط_البيانات]؛
    عرف فشل(خطأ: نـص): نـتيجة[نـمط_البيانات]؛
}
```

</div>

```
class Result [DataType: type = Int] {
    def data: DataType;
    def error: String;

    func success (d: ref[DataType]): Result[DataType];
    func success (): Result[DataType];
    func failure (err: String): Result[DataType];
}
```

`بيانات` (`data`) متغير يحمل البيانات لهذه النتيجة الناتجة عن تطبيق استعلام ما.

`خطأ` (`error`) متغير يحمل الخطأ الناتج أثناء محاولة تطبيق استعلام ما.

`نجاح` (`success`) تستعمل لإعادة النتيجة مع بيانات أو بدون بيانات.

`فشل` (`failure`) تستعمل لإعادة النتيجة مع رسالة خطأ.