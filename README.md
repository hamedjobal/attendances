# attendances
نظام حضور وانصراف (Attendance System) بشكل مبسط ومنظم وكأنه دراسة تحليلية للنظام:

🎯 الهدف من النظام

متابعة حضور وانصراف الموظفين أو الطلاب بشكل دقيق، مع استخراج تقارير تساعد الإدارة على مراقبة الالتزام وضبط ساعات العمل/الدراسة.

🧩 مكونات النظام الأساسية
1. المدخلات (Inputs)

بيانات الموظف/الطالب (الاسم، الرقم الوظيفي/الرقم الجامعي، القسم).

وقت الحضور (Check-in).

وقت الانصراف (Check-out).

أيام العطلات الرسمية.

جداول العمل (دوام صباحي، مسائي، ورديات).

إجازات الموظفين.

2. المعالجة (Processing)

مقارنة وقت الدخول والخروج مع مواعيد العمل الرسمية.

حساب ساعات الحضور وساعات التأخير/الخروج المبكر.

حساب الغياب (في حال عدم تسجيل حضور).

تسجيل الإجازات المصرح بها وربطها بسجلات الحضور.

معالجة الاستثناءات (مثل صيانة جهاز البصمة، دخول يدوي).

3. المخرجات (Outputs)

تقارير يومية/أسبوعية/شهرية:

تقرير الحضور والانصراف.

تقرير التأخير والغياب.

تقرير العمل الإضافي (Overtime).

تنبيهات:

تأخر موظف.

عدم تسجيل انصراف.

تجاوز عدد أيام الغياب المسموح بها.

واجهات للموظف:

الاطلاع على سجله.

طلب إجازة أو تصحيح خطأ.

🛠️ مكونات النظام التقنية

أجهزة إدخال:

جهاز بصمة (Finger print / Face recognition).

أو تطبيق موبايل GPS/QR.

أو تسجيل يدوي في حالات الطوارئ.

قاعدة بيانات:

جدول الموظفين.

جدول الحضور والانصراف.

جدول الإجازات.

جدول العطلات.

البرمجيات:

واجهة ويب/تطبيق لمتابعة التقارير.

لوحة تحكم للإدارة.

المستخدمين:

موظف عادي.

مدير مباشر.

إدارة الموارد البشرية.

🔑 العمليات الرئيسية في النظام

تسجيل الدخول/الخروج (Check-in/Check-out).

إدارة الجداول (الورديات/أوقات الدوام).

إدارة الإجازات (طلب، اعتماد، رفض).

إدارة العطلات الرسمية.

توليد التقارير.

صلاحيات المستخدمين (موظف، مدير، HR، Admin).

📊 مثال لتصميم قاعدة بيانات مبسطة
جدول الموظفين (Employees)
EmployeeID	Name	Department	Position	HireDate
جدول الحضور (Attendance)

| AttendanceID | EmployeeID | Date | CheckIn | CheckOut | Status (Present/Late/Absent) |

جدول الإجازات (LeaveRequests)

| LeaveID | EmployeeID | StartDate | EndDate | Type (Annual/Sick/Unpaid) | Status (Pending/Approved/Rejected) |

جدول العطلات (Holidays)

| HolidayID | Date | Description |

⚖️ فوائد النظام

تقليل التلاعب في سجلات الحضور.

استخراج تقارير دقيقة وسريعة.



1️⃣ Employee Model

//public class Employee
{
    public int EmployeeId { get; set; }   // المفتاح الأساسي
    public string FullName { get; set; } = string.Empty;
    public string JobTitle { get; set; } = string.Empty;
    public int DepartmentId { get; set; } // علاقة مع جدول Department
    public DateTime HireDate { get; set; }
    public Department? Department { get; set; }
    public ICollection<Attendance>? Attendances { get; set; }
    public ICollection<LeaveRequest>? LeaveRequests { get; set; }
}

2️⃣ Department Model
public class Department
{
    public int DepartmentId { get; set; }   // المفتاح الأساسي
    public string Name { get; set; } = string.Empty;

    // Navigation property
    public ICollection<Employee>? Employees { get; set; }
}

3️⃣ Attendance Model
public class Attendance
{
    public int AttendanceId { get; set; }   // المفتاح الأساسي
    public int EmployeeId { get; set; }     // علاقة مع جدول Employee
    public DateTime Date { get; set; }
    public DateTime? CheckIn { get; set; }
    public DateTime? CheckOut { get; set; }
    public string Status { get; set; } = "Present"; 
    // (Present, Late, Absent)

    // Navigation property
    public Employee? Employee { get; set; }
}

4️⃣ LeaveRequest Model
public class LeaveRequest
{
    public int LeaveRequestId { get; set; } // المفتاح الأساسي
    public int EmployeeId { get; set; }     // علاقة مع جدول Employee
    public DateTime StartDate { get; set; }
    public DateTime EndDate { get; set; }
    public string LeaveType { get; set; } = "Annual"; 
    // (Annual, Sick, Unpaid)

    public string Status { get; set; } = "Pending"; 
    // (Pending, Approved, Rejected)

    // Navigation property
    public Employee? Employee { get; set; }
}

5️⃣ Holiday Model
public class Holiday
{
    public int HolidayId { get; set; }     // المفتاح الأساسي
    public DateTime Date { get; set; }
    public string Description { get; set; } = string.Empty;
}

6️⃣ Shift (WorkSchedule) Model
public class Shift
{
    public int ShiftId { get; set; }       // المفتاح الأساسي
    public string Name { get; set; } = string.Empty;  
    // (Morning, Evening, Night)

    public TimeSpan StartTime { get; set; }
    public TimeSpan EndTime { get; set; }

    // علاقة اختيارية مع الموظفين
    public ICollection<Employee>? Employees { get; set; }
}

