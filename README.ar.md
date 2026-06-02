# صـفوف (Rows)

<div dir=rtl>

[[English]](README.md)

مكتبة للتعامل مع قواعد البيانات وتحقيق ال ORM (ربط الأغراض العلائقي) للغة الأسس.

## الإضافة إلى المشروع

يمكن تثبيت هذه المكتبة باستعمال التعليمات البرمجية التالية:

```
اشمل "مـحا"؛
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "<المشغل>" })؛
```

<div dir=ltr>

```
import "Apm";
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "<driver>" });
```

</div>

اسماء الملفات في المعطى الثاني يجب أن تشمل اسم المشغل المطلوب لقاعدة البيانات الخاصة بك. ما لم تكن
تنوي كتابة مشغلك الخاص فإنك ستحتاج لشمول أحد المشغلات المتوفرة في مكتبة صـفوف. مكتبة صـفوف لا تشمل
المشغلات تلقائيًا لأن كل مشغل يتطلب المكتبة الخاصة بقاعدة البيانات المعنية. مثلا، مشغل بوستغريس
يتطلب توفر مكتبة "libpq" في النظام بينما يتطلب مشغل مايسكويل مكتبة "libmysqlclient"، فتُرك شمول
المشغلات للمستخدم كي لا نجبره على تنصيب مكتبات لا يحتاج إليها في نظامه. 

### قاعدة بيانات بوستغرس (PostgreSQL)

```
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "مـشغلات/بـوستغرس.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_بوستغرس(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛
```

<div dir=ltr>

```
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "Drivers/Postgresql.alusus" });
use Rows;
def db: Db(PostgresqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

</div>

### قاعدة بيانات مايسكويل (MySQL)

```
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_مايسكويل(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "alusus"؛
    اسم_المستخدم = "alusus"؛
    كلمة_السر = "alusus"؛
    عنوان_الخادم = "0.0.0.0"؛
}))؛
```

<div dir=ltr>

```
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Rows;
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

</div>

### قاعدة بيانات سكويلايت (Sqlite)

```
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "مـشغلات/سـكويلايت.أسس" })؛
استخدم صـفوف؛
عرف قب: قـاعدة_بيانات(مـشغل_سكويلايت(مـعطيات_الاتصال().{
    اسم_قاعدة_البيانات = "الأسس.قب"؛
}))؛
```

<div dir=ltr>

```
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "Drivers/Sqlite.alusus" });
use Rows;
def db: Db(SqliteDriver(ConnectionParams().{
    dbName = "alusus.db";
}));
```

</div>

## مثال

### التعامل اليدوي مع قاعدة البيانات

```
اشمل "مـتم/نـظام"؛
اشمل "مـتم/نـص"؛
اشمل "مـتم/مـصفوفة"؛
اشمل "مـتم/تـطبيق"؛
اشمل "مـتم/سندات"؛
اشمل "مـحا"؛
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
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
عرف نتيجة: لـا_مضمون[صـحيح] = قب.نفذ(إنـشاء_جدول().{
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
    نـظام.فشل(1, نـص("فشلت العملية: ") + نتيجة.خطأ.هات_الرسالة())؛
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
عرف نتيجة: لـا_مضمون[مـصفوفة[مـصفوفة[نـص]]]؛
نتيجة = قب.نفذ(جـلب().{
    الجدول = نـص("users")؛
})؛
إذا !!نتيجة {
    نـظام.فشل(1, نـص("فشلت العملية: ") + نتيجة.خطأ.هات_الرسالة())؛
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

```
import "Srl/Possible";
import "Apm";
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "Drivers/Mysql.alusus" });
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
def res: Possible[Int] = db.exec(CreateTable().{
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
    System.fail(1, String("Query failed: ") + res.error.getMessage());
}

// ندخل بعض البيانات

db.exec("delete from roles");
db.exec("delete from users");

def names: Array[String]({ String("Sarmad"), String("Sleman"), String("Abdullah"), String("Salem") });
def addresses: Array[String]({ String("Canada"), String("Syria"), String("KSA"), String("KSA") });

def i: Int;
for i = 0, i < names.getLength(), ++i {
    def res: Possible[Int] = db.exec(Insert().{
        table = String("users");
        columns = Array[String]({ String("id"), String("name"), String("address") });
        data = Array[String]({ String("15") + i, names(i), addresses(i)  });
    });
    if !!res {
        System.fail(1, String("Query failed: ") + res.error.getMessage());
    }
}

// نقرأ البيانات

def data: Array[Array[String]];
def res: Possible[Array[Array[String]]];
res = db.exec(Select().{
    table = String("users");
});
if !!res {
    System.fail(1, String("Query failed: ") + res.error.getMessage());
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

```
اشمل "مـتم/نـص"؛
اشمل "مـتم/نـص"؛
اشمل "مـتم/مـصفوفة"؛
اشمل "مـتم/طـرفية"؛
اشمل "مـتم/نـظام"؛
اشمل "مـتم/سندات"؛
اشمل "مـحا"؛
مـحا.اشمل_حزمة("Alusus/Rows@0.4"، { "صـفوف.أسس"، "مـشغلات/مـايسكويل.أسس" })؛
استخدم مـتم؛
استخدم صـفوف؛

// نقوم بتحديد ما يقابل الجدول هنا، أي أننا نريد ما يلي في جدول باسم Cars
@جدول["سيارات"، 1]
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
قب.مهيكل[سـيارة].واكب()؛

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

```
import "Apm";
Apm.importPackage("Alusus/Rows@0.4", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Srl;
use Rows;

// Cars نقوم بتحديد ما يقابل الجدول هنا، أي أننا نريد ما يلي في جدول باسم
@model["cars", 1]
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

* إضافة المبدل `@جدول` (`@model`) إلى الصنف مع تحديد اسم الجدول كمعطى للمبدل وتحديد رقم الإصدار
  لذلك الجدول كمعطى ثاني.
* إضافة `عرف_أساسيات_الجدول[]` (`define_model_essentials`) في بداية صنف المستخدم.
* إضافة المبدل `@حقل` (`@column`) للمتغيرات المقابلة للحقول مع تحديد اسم الحقل كمعطى للمبدل.
* إضافة مبدل يقابل صنف بيانات الحقل للمتغيرات.
* إضافة المبدلات المتعلقة بالفهرسة والقيمة المبدئية والحقول المطلوبة إن وجد.

مثال: 

```
@جدول["سيارات"، 1]
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
    
    @مـحارف_مرنة["50"]
    @حقل
    عرف الاسم_الكامل: بـعدم[نـص]؛
}
```

<div dir=ltr>

```
@model["cars", 1]
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
    def fullName: Nullable[String];
}
```

</div>

بعد تعريف الصنف سيمكنك استخدامه في الدالات التي تتعامل مع الأصناف لقراءة وكتابة البيانات، مثل
دالة `قـاعدة_البيانات.من` (`Db.from`) أو دالة `قـاعدة_بيانات.احفظ` (`Db.save`) أو
دالة `مـهيكل.واكب` (`SchemaBuilder.migrate`).

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
* `@صـحيح_كبير` (`@BigInteger`)
* `@صـحيح_صغير` (`@SmallInteger`)
* `@صـحيح_ضئيل` (`@TinyInteger`)
* `@ثـنائية` (`@Boolean`)
* `@عـدد_حقيقي` (`@Real`)
* `@عـدد_عائم` (`@Float`)
* `@عـدد_عشري` (`@Decimal`)
* `@إكـسمل` (`@Xml`)
* `@مـحارف_مرنة` (`@VarChar`)
* `@مـحارف_محددة` (`@CharType`)
* `@نـص_كبير` (`@Text`)
* `@تـاريخ` (`@Date`)
* `@تـاريخ_ووقت` (`@DateTime`)

## الأصناف والتوابع

### الصنف مـعطيات_الاتصال (ConnectionParams)

```
صنف مـعطيات_الاتصال {
    عرف اسم_قاعدة_البيانات: نـص = ""؛
    عرف اسم_المستخدم: نـص = ""؛
    عرف كلمة_السر: نـص = ""؛
    عرف عنوان_الخادم: نـص = ""؛
    عرف المنفذ: نـص = ""؛
}
```

<div dir=ltr>

```
class ConnectionParams {
    def dbName: String = "";
    def userName: String = "";
    def password: String = "";
    def host: String = "";
    def port: int = 0;
}
```

</div>

يحتوي هذا الصنف على المعلومات اللازمة للاتصال بقاعدة البيانات.

####  اسم_قاعدة_البيانات (dbName)

```
عرف اسم_قاعدة_البيانات: نـص = ""
```

<div dir=ltr>

```
def dbName: String = ""
```

</div>

اسم قاعدة البيانات التي نرغب بالاتصال بها.

#### اسم_المستخدم (userName)

```
عرف اسم_المستخدم: نـص = ""
```

<div dir=ltr>

```
def userName: String = ""
```

</div>

اسم المستخدم الخاص بحسابنا في قاعدة البيانات.

#### كلمة_السر (password)

```
عرف كلمة_السر: نـص = ""
```

<div dir=ltr>

```
def password: String = ""
```

</div>

كلمة سر الحساب الخاص بنا في قاعدة البيانات.

#### عنوان_الخادم (host) 

```
  عرف عنوان_الخادم: نـص = ""
```

<div dir=ltr>

```
def host: String = ""
```

</div>

عنوان الخادم الذي تعمل قاعدة البيانات عليه.

####  المنفذ (port)

```
عرف المنفذ: صحيح = 0
```

<div dir=ltr>

```
def port: int = 0
```

</div>

المنفذ في الخادم الذي عن طريقه يمكن الوصول إلى قاعدة البيانات.

### الصنف إنـشاء_جدول (CreateTable)

```
صنف إنـشاء_جدول {
    عملية هذا.الاسم = نـص؛
    عملية هذا.جديد_فقط = ثـنائي؛
    عملية هذا.الحقول = تـطبيق[نـص، سـندنا[حـقل]]؛
    عملية هذا.الفهرس_الرئيسي = مـصفوفة[نـص]؛
    عملية هذا.فهارس_الوصل = مـصفوفة[سـندنا[فـهرس_وصل]]؛
}
```

<div dir=ltr>

```
class CreateTable {
    handler this.name = String;
    handler this.notExists = Bool;
    handler this.columns = Map[String, SrdRef[Column]];
    handler this.primaryKey = Array[String];
    handler this.foreignKeys = Array[SrdRef[ForeignKey]];
}
```

</div>

صنف يستعمل لإنشاء جدول حسب المعلومات التي يحويها.

#### الاسم (name)

```
عملية هذا.الاسم = نـص؛
```

<div dir=ltr>

```
handler this.name = String;
```

</div>

اسم الجدول.

#### جديد_فقط (notExists)

```
عملية هذا.جديد_فقط = ثـنائي؛
```

<div dir=ltr>

```
handler this.notExists = Bool;
```

</div>

متغير يحدد فيما إذا كنا نريد إنشاء الجدول فقط في حال لم يكن موجود مسبقاً.

#### الحقول (columns)

<div dir=rtl>

```
عملية هذا.الحقول = تـطبيق[نـص، سـندنا[حـقل]]؛
```

<div dir=ltr>

```
handler this.columns = Map[String, SrdRef[Column]];
```

</div>

تـطبيق يربط بين اسم حقل و المعلومات الخاص به ضمن الصنف `Column`.

#### الفهرس_الرئيسي (primaryKey)

<div dir=rtl>

```
عملية هذا.الفهرس_الرئيسي = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.primaryKey = Array[String];
```

</div>

الفهرس الرئيسي للجدول.

#### فهارس_الوصل (foreignKeys)

```
عملية هذا.فهارس_الوصل = مـصفوفة[سـندنا[فـهرس_وصل]]؛
```

<div dir=ltr>

```
handler this.foreignKeys = Array[SrdRef[ForeignKey]];
```

</div>

فهارس الوصل و هي تعرف ارتباطات الجدول مع الجداول الأخرى.

### الصنف حـذف (Delete)

<div dir=rtl>

```
صنف حـذف {
    عملية هذا.الجدول = نـص؛
    عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
}
```

<div dir=ltr>

```
class Delete {
    handler this.table = String;
    handler this.condition(statement: CharsPtr, args: ...any);
}
```

</div>
صنف يستعمل لحذف أسطر من جدول بناء على شرط معين.

#### الجدول (table)

<div dir=rtl>
```
عملية هذا.الجدول = نـص؛
```

<div dir=ltr>

```
handler this.table = String;
```

</div>

اسم الجدول.

#### الشرط (condition)

```
عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
```

<div dir=ltr>

```
handler this.condition(statement: CharsPtr, args: ...any);
```

</div>

الشرط الذي تم الحذف على أساسه.

### الصنف قـيمة (Value)

```
صنف قـيمة {
    عملية هذا~هيئ()؛
    عملية هذا~هيئ(ثـنائي)؛
    عملية هذا~هيئ(بـعدم[ثـنائي])؛
    عملية هذا~هيئ(صـحيح[64])؛
    عملية هذا~هيئ(بـعدم[صـحيح[32]])؛
    عملية هذا~هيئ(بـعدم[صـحيح[64]])؛
    عملية هذا~هيئ(عـائم[64])؛
    عملية هذا~هيئ(بـعدم[عـائم[32]])؛
    عملية هذا~هيئ(بـعدم[عـائم[64]])؛
    عملية هذا~هيئ(نـص)؛
    عملية هذا~هيئ(بـعدم[نـص])؛
    عملية هذا~هيئ(مـؤشر_محارف)؛
}
```

<div dir=ltr>

```
class Value {
    handler this~init();
    handler this~init(Bool);
    handler this~init(Nullable[Bool]);
    handler this~init(Int[64]);
    handler this~init(Nullable[Int[32]]);
    handler this~init(Nullable[Int[64]]);
    handler this~init(Float[64]);
    handler this~init(Nullable[Float[32]]);
    handler this~init(Nullable[Float[64]]);
    handler this~init(String);
    handler this~init(Nullable[String]);
    handler this~init(CharsPtr);
}
```

</div>

يستخدم هذا الصنف لتمرير بيانات من أي صنف كان لعمليتي `إدخـال` (`Insert`) و `تـحديث` (`Update`).

### الصنف إدخـال (Insert)

```
صنف إدخـال {
    عملية هذا.الجدول = نـص؛
    عملية هذا.البيانات = مـصفوفة[قـيمة]؛
    عملية هذا.الحقول = مـصفوفة[نـص]؛
}
```

<div dir=ltr>

```
class Insert {
    handler this.table = String;
    handler this.data = Array[Value];
    handler this.columns = Array[String];
}
```

</div>
يستعمل هذا الصنف لإضافة سطر إلى جدول.

#### الجدول (table)

```
عملية هذا.الجدول = نـص؛
```

<div dir=ltr>

```
handler this.table = String;
```

</div>

اسم الجدول الذي نريد الإضافة إليه.

#### البيانات (data)

```
عملية هذا.البيانات = مـصفوفة[قـيمة]؛
```

<div dir=ltr>

```
handler this.data = Array[Value];
```

</div>

البيانات الخاصة بالسطر المراد إضافته.

#### الحقول (columns)

```
عملية هذا.الحقول = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.columns = Array[String];
```

</div>

أسماء الأعمدة التي تتبع لها القيم في `البيانات`.

### الصنف جـلب (Select)

```
 صنف جـلب {
     عملية هذا.الجدول = مـصفوفة[نـص]؛
     عملية هذا.الحقول = مـصفوفة[نـص]؛
     عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
     عملية هذا.الترتيب = مـصفوفة[نـص]؛
}
```

<div dir=ltr>

```
class Select {
    handler this.table = Array[String];
    handler this.fields = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
    handler this.orderBy = Array[String];
}
```

</div>
يستعمل هذا الصنف لجلب أسطر من جدول في قاعدة البيانات.

#### الجدول (table)

```
عملية هذا.الجدول = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.table = Array[String];
```

</div>

اسم الجدول المراد جلب البيانات منه.

#### الحقول (fields)

```
عملية هذا.الحقول = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.fields = Array[String];
```

</div>

أسماء الأعمدة التي نرغب بجلب قيمها.

#### الشرط (condition)

```
عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
```

<div dir=ltr>

```
handler this.condition(statement: CharsPtr, args: ...any);
```

</div>

الشرط الذي نريد للأسطر أن تحققه حتى يتم جلبها.

#### الترتيب (orderBy)

<div dir=rtl>

```
عملية هذا.الترتيب = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.orderBy = Array[String];
```

</div>

الترتيب المراد تطبيقه على الأسطر.

### الصنف تـحديث (Update)

```
صنف تـحديث {
   عملية هذا.الجدول = نـص؛
    عملية هذا.البيانات = مـصفوفة[قـيمة]؛
    عملية هذا.الحقول = مـصفوفة[نـص]؛
    عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
}
```

<div dir=ltr>

```
class Update {
    handler this.table = String;
    handler this.data = Array[Value];
    handler this.columns = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
}
```

</div>

يستعمل هذا الصنف لتحديث سطر أو مجموعة أسطر في جدول في قاعدة البيانات.

#### الجدول (table)

```
عملية هذا.الجدول = نـص؛
```

<div dir=ltr>

```
handler this.table = String;
```

</div>

اسم الجدول المراد تحديث أسطر فيه.

#### البيانات (data)

```
عملية هذا.البيانات = مـصفوفة[قـيمة]؛
```

<div dir=ltr>

```
handler this.data = Array[Value];
```

</div>

القيم الجديدة التي نريد تحديث الأسطر بها.

#### الحقول (columns)

```
عملية هذا.الحقول = مـصفوفة[نـص]؛
```

<div dir=ltr>

```
handler this.columns = Array[String];
```

</div>

الأعمدة التي نريد تحديث قيمها حسب القيم في `البيانات`.

#### الشرط (condition)

```
عملية هذا.الشرط(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
```

<div dir=ltr>

```
handler this.condition(statement: CharsPtr, args: ...any);
```

</div>

الشرط الذي يجب على السطر تحقيقه حتى يتم تحديثه.

### الصنف حـقل (Column)

<div dir=rtl>

```
صنف حـقل {
    عملية هذا.الصنف = سـندنا[صـنف_البيانات]؛
    عملية هذا.إلزامي = ثـنائي؛
    عملية هذا.فريد = ثـنائي؛
    عملية هذا.القيمة_الافتراضية = نـص؛
    عملية هذا.التحقق(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
}
```

<div dir=ltr>

```
class Column {
    handler this.dataType = SrdRef[DataType];
    handler this.notNull = Bool;
    handler this.unique = Bool;
    handler this.default = String;
    handler this.check(statement: CharsPtr, args: ...any);
}
```

</div>

يحمل هذا الصنف المعلومات الخاصة بالحقل.

#### الصنف (dataType)

```
 عملية هذا.الصنف = سـندنا[صـنف_البيانات]؛
```

<div dir=ltr>

```
handler this.dataType = SrdRef[DataType];
```

</div>

نمط البيانات التي سيتم تخزينها في هذا الحقل.

#### إلزامي (notNull)

```
عملية هذا.إلزامي = ثـنائي؛
```

<div dir=ltr>

```
handler this.notNull = Bool;
```

</div>

هل من الضروري وجود قيمة لهذا الحقل أم من الممكن أن يكون خالٍ.

#### فريد (unique)

<div dir=rtl>

```
عملية هذا.فريد = ثـنائي؛
```

<div dir=ltr>

```
handler this.unique = Bool;
```

</div>

هل قيم هذا الحقل يحب أن تكون فريدة أم من الممكن أن يحمل سطران نفس القيمة في هذا العمود.

#### القيمة_الافتراضية (default)

<div dir=rtl>

```
عملية هذا.القيمة_الافتراضية = نـص؛
```

<div dir=ltr>

```
handler this.default = String;
```

</div>

القيمة الافتراضية لهذا الحقل في حال لم يتم إعطاء قيمة.

#### التحقق (check)

<div dir=rtl>

```
عملية هذا.التحقق(عبارة: مـؤشر_محارف، معطيات: ...أيما)؛
```

<div dir=ltr>

```
handler this.check(statement: CharsPtr, args: ...any);
```

</div>

التحقق الذي يجب تطبيقه قبل قبول قيمة لهذا الحقل.

### الصنف اسـتعلام (Query)

```
صنف اسـتعلام [جدول: صنف] {
   عملية [تركيب: شبم] هذا.رتب: سند[هذا_الصنف]؛
    @عضو ماكرو حيثما [هذا، شرط]؛
    @عضو ماكرو حدث [هذا، عبارة]؛
    عملية هذا.اجلب(): لـا_مضمون[مـصفوفة[سـندنا[جـدول]]]؛
    عملية هذا.احفظ(جدول: سند[جـدول]): لـا_مضمون[صـحيح]؛
    عملية هذا.احذف(جدول: سند[جـدول]): لـا_مضمون[صـحيح]؛
}
```

<div dir=ltr>

```
class Query [Model: type] {
    handler [exp: ast] this.order: ref[this_type];
    @member macro where [this, condition];
    @member macro update [this, expression];
    handler this.select(): Possible[Array[SrdRef[Model]]];
    handler this.save(model: ref[Model]): Possible[Int];
    handler this.delete(model: ref[Model]): Possible[Int];
}
```

</div>

يستخدم هذا الصنف لتمكين جمع الخصائص المختلفة للاستعلام قبل تنفيذه، وفي العادة لا يحتاج المستخدم
لإنشاء كائنات من هذا الصنف يدويًا إنما باستخدام `قـاعدة_بيانات.من` (`Db.from`). مثلا:

```
قب.من[مـستخدم].حيثما[الاسم == معطى1].حدث[العنوان = معطى2]؛
```

<div dir=ltr>

```
db.from[User].where[name == arg1].update[address = arg2];
```

</div>

#### رتب (order)

```
عملية [تركيب: شبم] هذا.رتب: سند[هذا_الصنف]؛
```

<div dir=ltr>

```
handler [exp: ast] this.order:ref[this_type];
```

</div>

يستعمل لوضع الترتيب الخاص باستعلام.

#### حيثما (where)

```
@عضو ماكرو حيثما [هذا، شرط]؛
```

<div dir=ltr>

```
@member macro where [this, condition];
```

</div>

ماكرو يستعمل لوضع الشرط الخاص باستعلام.

المعطيات:

* `هذا` (`this`) سند إلى غرض من النمط استعلام.
* `شرط` (`condition`) الشرط المطلوب تطبيقه.

#### حدث (update)

```
@عضو ماكرو حدث [هذا، عبارة]؛
```

<div dir=ltr>

```
@member macro update [this, expression];
```

</div>

ماكرو يستعمل لتنفيذ تعليمة تحديث على جدول.

المعطيات:

* `هذا` (`this`) سند إلى غرض من النمط استعلام.
* `عبارة` (`expression`) العبارة الخاصة بالتحديث.

#### اجلب (select)

```
عملية هذا.اجلب(): لـا_مضمون[مـصفوفة[سـندنا[جـدول]]]؛
```

<div dir=ltr>

```
handler this.select(): Possible[Array[SrdRef[Model]]];
```

</div>

يستعمل لجلب أسطر جدول ما.

#### احفظ (save)

```
عملية هذا.احفظ(جدول: سند[جـدول]): لـا_مضمون[صـحيح]؛
```

<div dir=ltr>

```
handler this.save(model: ref[Model]): Possible[Int];
```

</div>

يستعمل لحفظ سطر ما في الجدول.

المعطيات:

* `جدول` (`model`) سند إلى الجدول المراد حفظ السطر فيه.

#### احذف (delete)

<div dir=rtl>

```
عملية هذا.احذف(جدول: سند[جـدول]): لـا_مضمون[صـحيح]؛
```

<div dir=ltr>

```
handler this.delete(model: ref[Model]): Possible[Int];
```

</div>

يستعمل للحذف من الجدول.

المعطيات:

* `جدول` (`model`) سند إلى الجدول المراد حذف السطر فيه.

#### المطابقة الجزئية (LIKE)

لإجراء مطابقة جزئية (ما يعادل عبارة `LIKE` في SQL)، يمكن استخدام العامل `::` في شرط `حيثما` (`where`).
يتبع النمط صيغة SQL LIKE حيث يطابق `%` أي سلسلة من المحارف. ويمكن استخدام الرمز `!` لتمثيل الرموز
الخاصة نفسها (% و _ و !) ضمن نمط البحث نفسه.

```
د = قب.من[سـيارة].حيثما[الاسم :: "سيارة %"].اجلب()؛
```

<div dir=ltr>

```
d = db.from[Car].where[name :: "Car %"].select();
```

</div>

ملاحظة: بسبب أولوية العوامل في لغة الأسس، يجب استخدام الأقواس حول عبارة `::` عند دمجها مع شروط
أخرى في `حيثما` (`where`)، كما في المثال التالي:

```
د = قب.من[سـيارة].حيثما[(الاسم :: "سيارة %") و السعر < الحد_الأقصى].اجلب()؛
```

<div dir=ltr>

```
d = db.from[Car].where[(name :: "Car %") and price < maxPrice].select();
```

</div>

بدون الأقواس حول المؤثر `::` ستحصل على خطأ في الترجمة لأن المترجم سيحاول تنفيذ عملية `و` (`and`)
أولاً.

### الصنف مـهيكل (SchemaBuilder)

```
صنف مـهيكل [هيكل: سند_شبم] {
    عملية هذا.واكب(): سـندنا[خـطأ]؛
}
```

<div dir=ltr>

```
class SchemaBuilder [schema: ast_ref] {
    handler this.migrate(): SrdRef[Error];
}
```

</div>

يستخدم قالب الأصناف هذا لمواكبة (migrate) قاعدة البيانات مع أصناف البيانات (models) المعرفة
في الشفرة المصدرية. يجب أن يكون معطى القالب إشارة إلى صنف بيانات أو إلى قائمة من الأصناف التي
تتشكل منها قاعدة البيانات. يتولى المهيكل مسؤولية إنشاء قاعدة البيانات أو تحديثها إلى آخر
إصدار عبر تنفيذ دالات المواكبة المطلوبة.

#### واكب (migrate)

```
عملية هذا.واكب(): سـندنا[خـطأ]؛
```

<div dir=ltr>

```
handler this.migrate(): SrdRef[Error];
```

</div>

`واكب` هذه الدالة تواكب قاعدة البيانات مع أصناف البيانات المعرفة في الشفرة المصدرية، أو تنشئ
قاعدة البيانات إن لم تكون موجودة بالأساس.

عندما لا يكون الجدول موجودا في قاعدة البيانات ينشئه المهيكل من صنف البيانات مباشرة، وإن كان
الجدول موجودا يقارن رقم إصدار ذلك الجدول في قاعدة البيانات مع رقم الإصدار المعرف في الشفرة
المصدرية. إن لم يتطابق رقم الإصدار يبحث المهيكل عن دالات المواكبة المعرفة ضمن ذلك الصنف المطلوبة
لتحديث الجدول إلى الإصدار الأخير. هذه الدالات تلتزم التعريف التالي:

```
@مواكبة[1، 2]
دالة حدث_من_1_إلى_2(قب: سند[قـاعدة_بيانات]): سـندنا[خـطأ]؛
```

<div dir=ltr>

```
@migration[1, 2]
function migrateFromV1ToV2(db: ref[Db]): SrdRef[Error];
```

</div>

الدالة أعلاه تحدث الجدول من الإصدار 1 إلى الإصدار 2. يمكنك إضافة قائمة بالاعتماديات الخاصة بتلك
الدالة إلى المبدل `مواكبة` (`migration`) لضمان تنفيذ دالات المواكبة بالترتيب الصحيح. 

علي سبيل المثال :

```
@مواكبة[1، 2، { مـستخدم: 3 }]
دالة حدث_من_1_إلى_2(قب: سند[قـاعدة_بيانات]): سـندنا[خـطأ]؛
```

<div dir=ltr>

```
@migration[1, 2, { User: 3 }]
function migrateFromV1ToV2(db: ref[Db]): SrdRef[Error];
```

</div>

المثال أعلاه يخبر المهيكل أن ينفذ هذه الدالة عندما يكون الجدول `مـستخدم` (`User`) عن الإصدار 3.
يضمن هذا أن يؤخر تنفيذ هذه الدالة لحين تحديث الجدول `مـستخدم` إلى الإصدار 3، كما يضمن أن لا تُنفذ
أي دالة مواكبة تحدث الجدول `مـستخدم` من الإصدار 3 لحين تنفيذ هذه الدالة. إن تطلب تحديث جدول
معين إلى الإصدار الأخير تنفيذ عدة دالات مواكبة فسيفعل ذلك المهيكل مع ضمان تنفيذهم بالتسلسل
الصحيح.

إن شمل صنف بيانات دالة مواكبة تُحدث من الإصدار 0 فسيتجنب المهيكل إنشاء الجدول من الصفر ويترك ذلك
لدالة المواكبة تلك حتى لو كان يُنشئ قاعدة البيانات من الصفر.

### الصنف مـشغل (Driver)

```
صنف مـشغل {
    عملية هذا.اتصل(معطيات: سند[مـعطيات_الاتصال]): ثـنائي كمؤشر؛
    عملية هذا.افصل() كمؤشر؛
    عملية هذا.أمتصل(): ثـنائي كمؤشر؛
    عملية هذا.هل_بدئ_الاتصال(): ثـنائي كمؤشر؛
    عملية هذا.هات_معطيات_الاتصال(): مـعطيات_الاتصال كمؤشر؛
    عملية هذا.هات_آخر_خطأ(): نـص كمؤشر؛
}
```

<div dir=ltr>

```
class Driver {
    handler this.connect(parmas: ref[ConnectionParams]): Bool as_ptr;
    handler this.disconnect() as_ptr;
    handler this.isConnected(): Bool as_ptr;
    handler this.isConnectionEstablished(): Bool as_ptr;
    handler this.getConnectionParams(): ConnectionParams as_ptr;
    handler this.getLastError(): String as_ptr;
}
```

</div>

هذا هو الصنف الأساسي لجميع مشغلات قواعد البيانات، وهو المسؤول عن جميع الاتصالات
مع قاعدة البيانات. يحتوي على العديد من الوظائف المجردة التي يستعملها الصنف `قـاعدة_بيانات`
ولن يحتاج المستخدم في العادة للتعامل مع أي من وظائف هذا الصنف عدا الوظائف المتعلقة بالاتصال.

#### اتصل (connect)

```
عملية هذا.اتصل(معطيات: سند[مـعطيات_الاتصال]): ثـنائي كمؤشر؛
```

<div dir=ltr>

```
handler this.connect(parmas: ref[ConnectionParams]): Bool as_ptr;
```

</div>

الاتصال بقاعدة البيانات.

#### افصل (disconnect)

```
عملية هذا.افصل() كمؤشر؛
```

<div dir=ltr>

```
handler this.disconnect() as_ptr;
```

</div>

قطع الاتصال بقاعدة البيانات.

#### أمتصل (isConnected)

```
عملية هذا.أمتصل(): ثـنائي كمؤشر؛
```

<div dir=ltr>

```
handler this.isConnected(): Bool as_ptr;
```

</div>

التحقق من الاتصال بقاعدة البيانات.

#### هل_بدئ_الاتصال (isConnectionEstablished)

```
عملية هذا.هل_بدئ_الاتصال(): ثـنائي كمؤشر؛
```

<div dir=ltr>

```
handler this.isConnectionEstablished(): Bool as_ptr;
```

</div>

التحقق من أن الاتصال قد تم.

#### هات_معطيات_الاتصال (getConnectionParams)

```
عملية هذا.هات_معطيات_الاتصال(): مـعطيات_الاتصال كمؤشر؛
```

<div dir=ltr>

```
handler this.getConnectionParams(): ConnectionParams as_ptr;
```

</div>

جلب معطيات الاتصال.

#### هات_آخر_خطأ (getLastError)

```
عملية هذا.هات_آخر_خطأ(): نـص كمؤشر؛
```

<div dir=ltr>

```
handler this.getLastError(): String as_ptr;
```

</div>

جلب رسالة آخر خطأ.

### الصنف قـاعدة_بيانات (Db)

<div dir=rtl>

```
صنف قـاعدة_بيانات {
    عرف تدوين: ثـنائي = 1؛
    عرف انتظار_إعادة_الاتصال: طـبيعي = 2000000؛ // ثانيتان
    عرف عدد_محاولات_إعادة_الاتصال: صـحيح = 3؛

    عملية هذا~هيئ(م: سـندنا[مـشغل])؛
    عملية هذا~هيئ(مهيئ: مغلفة(سند[سـندنا[مـشغل]]))؛
    عملية هذا.هيئ(م: سـندنا[مـشغل])؛
    عملية هذا.هيئ(مهيئ: مغلفة(سند[سـندنا[مـشغل]]))؛
    عملية هذا.أمتصل(): ثـنائي؛
    عملية هذا.هات_آخر_خطأ(): نـص؛
    عملية هذا.نفذ(جـلب: سند[جـلب]): لـا_مضمون[مـصفوفة[مـصفوفة[نـص]]]؛
    عملية هذا.نفذ(تحديث: سند[تحديث]): لـا_مضمون[صـحيح]؛
    عملية هذا.نفذ(إدخـال: سند[إدخـال]): لـا_مضمون[صـحيح]؛
    عملية هذا.نفذ(حـذف: سند[حـذف]): لـا_مضمون[صـحيح]؛
    عملية هذا.نفذ(إنـشاء_جدول: سند[إنـشاء_جدول]): لـا_مضمون[صـحيح]؛
    عملية هذا.نفذ(عبارة: مـؤشر_محارف معطيات: ...أيما): لـا_مضمون[صـحيح]؛
    عملية هذا.نفذ_جلبا(عبارة: مـؤشر_محارف, معطيات: ...أيما): لـا_مضمون[مـصفوفة[مـصفوفة[بـعدم[نـص]]]]؛
    عملية [جـدول: صنف] هذا.من: اسـتعلام[جـدول]؛
    عملية [جـدول: صنف] هذا.احفظ(جدول: سند[جـدول])؛
    عملية [جـدول: صنف] هذا.مهيكل: مـهيكل[جـدول]؛
}
```

<div dir=ltr>

```
class Db {
    def logging: Bool = true;
    def reconnectionDelay: Word = 2000000; // 2 seconds
    def reconnectionAttemptCount: Int = 3;

    handler this~init(d: SrdRef[Driver]);
    handler this~init(initializer: closure(ref[SrdRef[Driver]]));
    handler this.init(d: SrdRef[Driver]);
    handler this.init(initializer: closure(ref[SrdRef[Driver]]));
    handler this.isConnected(): Bool;
    handler this.getLastError(): String;
    handler this.exec(select: ref[Select]): Possible[Array[Array[String]]];
    handler this.exec(update: ref[Update]): Possible[Int];
    handler this.exec(insert: ref[Insert]): Possible[Int];
    handler this.exec(delete: ref[Delete]): Possible[Int];
    handler this.exec(createTable: ref[CreateTable]): Possible[Int];
    handler this.exec(statement: CharsPtr, args: ...any): Possible[Int];
    handler this.execSelect(statement: CharsPtr, args: ...any): Possible[Array[Array[Nullable[String]]]];
    handler [Model: type] this.from: Query[Model];
    handler [Model: type] this.save(model: ref[Model]);
    handler [Model: type] this.schemaBuilder: SchemaBuilder[Model];
}
```

</div>

يستعمل هذا الصنف لتنظيم الوصول إلى قاعدة البيانات و تنفيذ مختلف العمليات عليها.

#### تدوين (logging)

```
قـاعدة_بيانات.تدوين: ثـنائي;
```

<div dir=ltr>

```
db.logging: Bool;
```

</div>

إعطاء هذا المتغير قيمة 1 يجعل المكتبة تطبع عبارات SQL التي يتم تنفيذها.

#### انتظار_إعادة_الاتصال (reconnectionDelay)

```
قـاعدة_بيانات.انتظار_إعادة_الاتصال: طـبيعي;
```

<div dir=ltr>

```
db.reconnectionDelay: Word;
```

</div>

الوقت بالمايكرو ثانية الذي تنتظره المكتبة عند انقطاع
الاتصال بالخادم قبل معاودة الاتصال.

#### عدد_محاولات_إعادة_الاتصال (reconnectionAttemptCount)

```
قـاعدة_بيانات.عدد_محاولات_إعادة_الاتصال: صـحيح;
```

<div dir=ltr>

```
db.reconnectionAttemptCount: Int;
```

</div>

عدد محاولات إعادة الاتصال قبل أن تتوقف المكتبة
عن المحاولة وترجع إشعار خطأ.

#### ~هيئ (init~)

```
عملية هذا~هيئ(م: سـندنا[مـشغل])؛
عملية هذا~هيئ(مهيئ: مغلفة(سند[سـندنا[مـشغل]]))؛
```

<div dir=ltr>

```
handler this~init(d: SrdRef[Driver]);
handler this~init(initializer: closure(ref[SrdRef[Driver]]));
```

</div>

يهيئ قاعدة البيانات بالمشغل المعطى. نسخة المغلفة من هذه الدالة تستخدم لتمكين استخدام
كائن `قـاعدة_بيانات` من مسالك متعددة. تُستدعى الدالة لتهيئة مشغل جديد لكل مسلك جديد يستخدم قاعدة
البيانات.

#### هيئ (init)

```
عملية هذا.هيئ(م: سـندنا[مـشغل])؛
عملية هذا.هيئ(مهيئ: مغلفة(سند[سـندنا[مـشغل]]))؛
```

<div dir=ltr>

```
handler this.init(d: SrdRef[Driver]);
handler this.init(initializer: closure(ref[SrdRef[Driver]]));
```

</div>

يهيئ قاعدة البيانات بالمشغل المعطى. نسخة المغلفة تستخدم لدعم المسالك المتعددة.

#### أمتصل (isConnected)

```
عملية هذا.أمتصل(): ثـنائي؛
```

<div dir=ltr>

```
handler this.isConnected(): Bool;
```

</div>

يستعمل للتحقق فيما إذا كان هناك اتصال مع قاعدة البيانات.

#### هات_آخر_خطأ (getLastError)

```
عملية هذا.هات_آخر_خطأ(): نـص؛
```

<div dir=ltr>

```
handler this.getLastError(): String;
```

</div>

يستعمل لجلب آخر خطأ.

#### نفذ (exec)

```
عملية هذا.نفذ(جـلب: سند[جـلب]): لـا_مضمون[مـصفوفة[مـصفوفة[نـص]]]؛
عملية هذا.نفذ(تحديث: سند[تحديث]): لـا_مضمون[صـحيح]؛
عملية هذا.نفذ(إدخـال: سند[إدخـال]): لـا_مضمون[صـحيح]؛
عملية هذا.نفذ(حـذف: سند[حـذف]): لـا_مضمون[صـحيح]؛
عملية هذا.نفذ(إنـشاء_جدول: سند[إنـشاء_جدول]): لـا_مضمون[صـحيح]؛
عملية هذا.نفذ(عبارة: مـؤشر_محارف معطيات: ...أيما): لـا_مضمون[صـحيح]؛
```

<div dir=ltr>

```
handler this.exec(select: ref[Select]): Possible[Array[Array[String]]];
handler this.exec(update: ref[Update]): Possible[Int];
handler this.exec(insert: ref[Insert]): Possible[Int];
handler this.exec(delete: ref[Delete]): Possible[Int];
handler this.exec(createTable: ref[CreateTable]): Possible[Int];
handler this.exec(statement: CharsPtr, args: ...any): Possible[Int];
```

</div>

`نفذ` (`exec`) يستعمل لتنفيذ استعلام ما، و له نسخة لكل نوع من أنواع الاستعلامات بالإضافة
إلى نسخة لتنفيذ عبارات SQL خام. نسخة تنفيذ عبارات SQL تفصل الهيكل الرئيسي لعبارة SQL
عن المعطيات التي يمررها المستخدم كمعطيات لاحظة لدالة `نفذ` بطريقة مشابهة لعمل دالة
الطباعة. تستبدل الدالة الرموز البادئة ب% في SQL بقيمة من قائمة المعطيات تطابق الصنف
المحدد. الرمز % يتبعه محرف يدل على صنف القيمة المعطاة، وهي كالتالي:

* ` مـؤشر_محارف` (`CharsPtr`): `%n`
* `نـص` (`String`): `%s`
* `مـؤشر_محارف` (`CharsPtr`): `%p`
* `صـحيح` (`Int`): `%i`
* `صـحيح[64]` (`Int[64]`): `%l`
* `عـائم` (`Float`): `%f`
* `عـائم[64]` (`Float[64]`): `%d`
* `مـصفوفة[نـص]` (`Array[String]`): `%as`
* `مـصفوفة[مـؤشر_محارف]` (`Array[CharsPtr]`): `%ap`
* `مـصفوفة[صـحيح]` (`Array[Int]`): `%ai`
* `مـصفوفة[صـحيح[64]]` (`Array[Int[64]]`): `%al`
* `مـصفوفة[عـائم]` (`Array[Float]`): `%af`
* `مـصفوفة[عـائم[64]]` (`Array[Float[64]]`): `%ad`

#### نفذ_جلبا (execSelect)

```
عملية هذا.نفذ_جلبا(عبارة: مـؤشر_محارف, معطيات: ...أيما): لـا_مضمون[مـصفوفة[مـصفوفة[بـعدم[نـص]]]]؛
```

<div dir=ltr>

```
handler this.execSelect(statement: CharsPtr, args: ...any): Possible[Array[Array[Nullable[String]]]];
```

</div>

`نفذ_جلبا` مشابهة لدالة `نفذ` الخاصة بتنفيذ عبارات SQL لكنها تستخدم مع العبارات
التي تجلب بيانات.

#### من (from)

```
عملية [جـدول: صنف] هذا.من: اسـتعلام[جـدول]؛
```

<div dir=ltr>

```
handler [Model: type] this.from: Query[Model];
```

</div>

يستعمل لإعادة استعلام بناء على المعلومات في هذا الصنف.

#### احفظ (save)
    
```
عملية [جـدول: صنف] هذا.احفظ(جدول: سند[جـدول])؛
```

<div dir=ltr>

```
handler [Model: type] this.save(model: ref[Model]);
```

</div>

يستعمل لاستدعاء الوظيفة `احفظ` في الصنف `اسـتعلام`.

#### مهيكل (schemaBuilder)

```
عملية [جـدول: صنف] هذا.مهيكل: مـهيكل[جـدول]؛
```

<div dir=ltr>

```
handler [Model: type] this.schemaBuilder: SchemaBuilder[Model];
```

</div>

يستعمل لإعادة مهيكل بناء على المعلومات في هذا الصنف.

### الدالة هات_اعتماديات_البناء (getBuildDependencies)

```
دالة هات_اعتماديات_البناء(): مـصفوفة[نـص]؛
```

<div dir=ltr>

```
func getBuildDependencies(): Array[String];
```

</div>

كل مشغل من مشغلات قواعد البيانات المختلفة يعرف هذه الدالة، والتي ترجع قائمة بالمكتبات الخارجية
المطلوبة لبناء نسخة تنفيذية تربط مع نوع قواعد البيانات الخاصة بهذا المشغل.

### أخـطاء (Errors)

الوحدة الفرعية `أخـطاء` تحتوي تعريفات لرموز الأخطاء التي يمكن لمكتبة صفوفة إرجاعها.

* `أخـطاء._قب_غير_مهيأة_` (`Errors.DB_NOT_INITIALIZED`): تُرجع عند استخدام الصنف `قـاعدة_بيانات` قبل تهيئته.
* `أخـطاء._قب_غير_متصلة_` (`Errors.DB_NOT_CONNECTED`): تُرجع عند استخدام الصنف `قـاعدة_بيانات` لكن الاتصال بقاعدة
  البيانات مفقود.
* `أخـطاء._خطأ_سكول_` (`Errors.SQL_ERROR`): تُرجع عند حصول خطأ أثناء تنفيذ عملية SQL.
* `أخـطاء._التصال_مفقود_` (`Errors.CONNECTION_MISSING`): تُرجع عند استدعاء دالتي الحفظ أو الحذف على متغير من
  صنف جدول (model) لكن ذلك المتغير غير مرتبط بكائن `قـاعدة_بيانات`، أي أنه لم يُحمل مسبقًا من قاعدة البيانات.

## الرخصة

حقوق النشر © 2026 سرمد خالد عبد الله

هذا المشروع مرخص بموجب رخصة غنو العمومية الصغرى الإصدار 3.0 (LGPL-3.0). راجع ملفات `COPYING` و `COPYING.LESSER` للحصول على التفاصيل.

</div>
