# 📚 توثيق APIs - الهيكل الأكاديمي
### Academic Structure API Documentation

> **موجّه لـ:** مطوري Frontend (React/Vue/Angular) · مطوري Windows Forms · مطوري Mobile (Android/iOS/Flutter)
>
> **Base URL:** `https://<your-server>/api`
>
> **المصادقة:** كل الطلبات (ما عدا المُعلّمة بـ 🔓) تحتاج `Authorization` Header:
> ```
> Authorization: Bearer <JWT_TOKEN>
> ```

---

## 🏫 جدول الأدوار والصلاحيات (Roles Reference)

### 👑 الإداريون (Admins)
| الدور | الوصف |
|---|---|
| `مشرف عام` | Super Admin — صلاحيات كاملة على كل شيء |
| `إداري الجامعة` | University Admin — يدير جامعته فقط |
| `إداري الكلية` | Faculty Admin — يدير كليته فقط |

### 🎓 الأكاديميون (Academic Staff)
| الدور | الوصف |
|---|---|
| `دكتور` | Doctor |
| `أستاذ` | Professor |
| `أستاذ مساعد` | Associate Professor |
| `مدرس مساعد` | Assistant Lecturer |
| `معيد` | Teaching Assistant |

### 🗂️ الموظفون (Staff)
| الدور | الوصف |
|---|---|
| `موظف المالية` | Finance Staff |
| `موظف شئون الطلاب` | Student Affairs Staff |
| `موظف رعاية الشباب` | Youth Care Staff |
| `موظف الحسابات` | Accounts Staff |
| `موظف المشتريات` | Procurement Staff |
| `موظف المكتبة` | Library Staff |

### 👨‍🎓 الطلاب والخريجون
| الدور | الوصف |
|---|---|
| `طالب بكالوريوس` | Bachelor Student |
| `طالب ماجستير` | Master Student |
| `طالب دكتوراه` | PhD Student |
| `خريج بكالوريوس` | Bachelor Graduate |
| `خريج ماجستير` | Master Graduate |
| `خريج دكتوراه` | PhD Graduate |
| `زائر` | Visitor |

---

### 📜 الـ Policies المُعرَّفة في النظام

| Policy Name | الأدوار المشمولة |
|---|---|
| `SuperAdminOnly` | `مشرف عام` |
| `UniversityAdminOnly` | `إداري الجامعة` |
| `FacultyAdminOnly` | `إداري الكلية` |
| `AdminOnly` | `مشرف عام` + `إداري الجامعة` + `إداري الكلية` |
| `AcademicStaff` | `دكتور` + `أستاذ` + `أستاذ مساعد` + `مدرس مساعد` + `معيد` |
| `Staff` | موظفو المالية، شئون الطلاب، رعاية الشباب، الحسابات، المشتريات، المكتبة |
| `AdminAndAcademic` | `AdminOnly` + `AcademicStaff` |
| `AdminAcademicAndStaff` | `AdminOnly` + `AcademicStaff` + `Staff` |
| `AllExceptVisitor` | الكل (بما فيهم الطلاب والخريجين والزائر) |

---

## 🗂️ فهرس المحتوى

1. [Universities - الجامعات](#1-universities---الجامعات)
2. [Faculties - الكليات](#2-faculties---الكليات)
3. [Program Types - أنواع البرامج](#3-program-types---أنواع-البرامج)
4. [Programs - البرامج الأكاديمية](#4-programs---البرامج-الأكاديمية)
5. [Departments - الأقسام](#5-departments---الأقسام)
6. [Standard Types - أنواع المعايير](#6-standard-types---أنواع-المعايير)
7. [Standards - المعايير](#7-standards---المعايير)
8. [Indicators - المؤشرات](#8-indicators---المؤشرات)

---

## 1. Universities - الجامعات

**Base Route:** `GET/POST/PUT/DELETE /api/universities`

### 📌 1.1 عرض قائمة الجامعات

```
GET /api/universities
```

**🔓 لا يحتاج مصادقة — متاح للجميع**

**Query Parameters:**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `page` | int | 1 | رقم الصفحة |
| `limit` | int | 10 | عدد العناصر في الصفحة |
| `search` | string | null | بحث بالاسم أو الإيميل |

**Example Request:**
```http
GET /api/universities?page=1&limit=10&search=القاهرة
```

**Response `200 OK`:**
```json
{
  "data": [
    {
      "universityId": 1,
      "name": "جامعة القاهرة",
      "logoFiles": "logo.png",
      "establishmentYear": "1908",
      "facultiesCount": 25
    }
  ],
  "total": 50,
  "page": 1,
  "limit": 10
}
```

---

### 📌 1.2 عرض جامعة بالتفصيل

```
GET /api/universities/{id}
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Path Parameter:** `id` = معرّف الجامعة (int)

**Response `200 OK`:**
```json
{
  "universityId": 1,
  "name": "جامعة القاهرة",
  "email": "info@cairo.edu",
  "phone": "+20xxxxxxxxxx",
  "geographicLocation": "القاهرة، مصر",
  "websiteUrl": "https://cu.edu.eg",
  "establishmentYear": "1908",
  "description": "...",
  "logoFiles": "logo.png",
  "facultiesCount": 25,
  "createdAt": "2024-01-01T00:00:00Z",
  "updatedAt": "2024-06-01T00:00:00Z",
  "faculties": [
    { "facultyId": 1, "facultyName": "كلية الهندسة" }
  ],
  "visions": [
    { "visionId": 1, "visionText": "...", "createdAt": "..." }
  ],
  "missions": [
    { "missionId": 1, "missionText": "...", "createdAt": "..." }
  ],
  "goals": [
    { "goalId": 1, "goalText": "...", "createdAt": "..." }
  ]
}
```

**Response `404`:** `{ "error": "University not found" }`

---

### 📌 1.3 إنشاء جامعة جديدة

```
POST /api/universities
```

**🔐 يحتاج: `مشرف عام` فقط (SuperAdminOnly)**

**Request Body:**
```json
{
  "name": "جامعة الإسكندرية",
  "email": "info@alex.edu.eg",
  "phone": "+20xxxxxxxxxx",
  "geographicLocation": "الإسكندرية، مصر",
  "websiteUrl": "https://alexu.edu.eg",
  "establishmentYear": "1942",
  "description": "وصف الجامعة",
  "logoFiles": ["logo.png"],
  "vision": "رؤية الجامعة",
  "mission": "رسالة الجامعة",
  "goals": "هدف 1|هدف 2|هدف 3"
}
```

> **⚠️ ملاحظة حقل `goals`:** يتم الفصل بين الأهداف المتعددة بعلامة `|`

**Request Fields:**

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | اسم الجامعة |
| `email` | string | ✅ | البريد الإلكتروني (فريد) |
| `phone` | string | ❌ | رقم الهاتف |
| `geographicLocation` | string | ❌ | الموقع الجغرافي |
| `websiteUrl` | string | ❌ | رابط الموقع |
| `establishmentYear` | string | ❌ | سنة التأسيس |
| `description` | string | ❌ | وصف |
| `logoFiles` | string[] | ❌ | مسارات الشعارات |
| `vision` | string | ❌ | رؤية الجامعة (أول رؤية) |
| `mission` | string | ❌ | رسالة الجامعة (أول رسالة) |
| `goals` | string | ❌ | الأهداف مفصولة بـ `\|` |

**Response `201 Created`:** بيانات الجامعة المُنشأة

**Response `400`:** `{ "error": "University with email ... already exists" }`

---

### 📌 1.4 تحديث جامعة

```
PUT /api/universities/{id}
```

**🔐 يحتاج: `مشرف عام` أو `إداري الجامعة` (لجامعته فقط)**

**Request Body:** نفس حقول الإنشاء (جميعها اختيارية، يتم تحديث ما يُرسَل فقط)

**Response `200 OK`:** بيانات الجامعة المُحدّثة

**Response `403 Forbidden`:** المستخدم لا يملك صلاحية على هذه الجامعة

---

### 📌 1.5 حذف جامعة

```
DELETE /api/universities/{id}
```

**🔐 يحتاج: `مشرف عام` فقط (SuperAdminOnly)**

> **⚠️ مهم:** لا يمكن حذف الجامعة إذا كان لها كليات. يجب حذف الكليات أولاً.

**Response `200 OK`:**
```json
{ "success": true, "message": "University deleted successfully", "deletedId": 1 }
```

**Response `400`:** `{ "error": "Cannot delete university '...' because it has X faculty/faculties..." }`

---

### 📌 1.6 APIs الرؤية (Vision)

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/universities/{id}/vision/current` | 🔐 أي مستخدم | الرؤية الحالية (الأحدث) |
| `GET` | `/api/universities/{id}/visions` | 🔐 أي مستخدم | كل الرؤى بالتاريخ |
| `POST` | `/api/universities/{id}/visions` | 🔐 `مشرف عام` أو `إداري الجامعة` | إضافة رؤية جديدة |
| `PUT` | `/api/visions/{visionId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | تعديل رؤية |
| `DELETE` | `/api/visions/{visionId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | حذف رؤية |

**POST Request Body:**
```json
{ "visionText": "نص الرؤية الجديد" }
```

**GET Response Example:**
```json
{
  "visionId": 5,
  "universityId": 1,
  "visionText": "نص الرؤية",
  "createdAt": "2024-06-01T00:00:00Z"
}
```

---

### 📌 1.7 APIs الرسالة (Mission)

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/universities/{id}/mission/current` | 🔐 أي مستخدم | الرسالة الحالية |
| `GET` | `/api/universities/{id}/missions` | 🔐 أي مستخدم | كل الرسائل |
| `POST` | `/api/universities/{id}/missions` | 🔐 `مشرف عام` أو `إداري الجامعة` | إضافة رسالة |
| `PUT` | `/api/missions/{missionId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | تعديل رسالة |
| `DELETE` | `/api/missions/{missionId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | حذف رسالة |

**POST Request Body:**
```json
{ "missionText": "نص الرسالة" }
```

---

### 📌 1.8 APIs الأهداف (Goals)

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/universities/{id}/goal/current` | 🔐 أي مستخدم | الهدف الأحدث |
| `GET` | `/api/universities/{id}/goals` | 🔐 أي مستخدم | كل الأهداف |
| `POST` | `/api/universities/{id}/goals` | 🔐 `مشرف عام` أو `إداري الجامعة` | إضافة هدف |
| `PUT` | `/api/goals/{goalId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | تعديل هدف |
| `DELETE` | `/api/goals/{goalId}` | 🔐 `مشرف عام` أو `إداري الجامعة` | حذف هدف |

**POST Request Body:**
```json
{ "goalText": "نص الهدف" }
```

---

## 2. Faculties - الكليات

**Routes تحت University:**
- `GET /api/universities/{id}/faculties`
- `POST /api/universities/{id}/faculties`
- `GET /api/universities/faculties/{facultyId}`
- `PUT /api/universities/faculties/{facultyId}`
- `DELETE /api/universities/faculties/{facultyId}`

---

### 📌 2.1 عرض كليات جامعة معينة

```
GET /api/universities/{id}/faculties
```

**🔓 لا يحتاج مصادقة**

**Response `200 OK`:**
```json
[
  {
    "facultyId": 1,
    "facultyName": "كلية الهندسة",
    "universityId": 1,
    "deanName": "د. أحمد محمد",
    "email": "eng@uni.edu",
    "phone": "+20xxxxxxxxxx",
    "description": "وصف الكلية",
    "logoUrl": "https://...",
    "createdAt": "...",
    "updatedAt": "...",
    "programsCount": 5,
    "departmentsCount": 12,
    "studentsCount": 3000,
    "facultyMembersCount": 200,
    "publicationsCount": 500,
    "hasPreparatoryYear": false
  }
]
```

---

### 📌 2.2 عرض كلية بالتفصيل

```
GET /api/universities/faculties/{facultyId}
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:** (يشمل كل بيانات الكلية + الرؤى + الرسائل + الأهداف)
```json
{
  "facultyId": 1,
  "facultyName": "كلية الهندسة",
  "universityId": 1,
  "deanName": "د. أحمد",
  "email": "eng@uni.edu",
  "phone": "...",
  "description": "...",
  "logoUrl": "...",
  "createdAt": "...",
  "updatedAt": "...",
  "programsCount": 5,
  "departmentsCount": 12,
  "studentsCount": 3000,
  "facultyMembersCount": 200,
  "publicationsCount": 500,
  "hasPreparatoryYear": false,
  "currentVision": { "visionId": 1, "facultyId": 1, "visionText": "...", "createdAt": "..." },
  "currentMission": { "missionId": 1, "facultyId": 1, "missionText": "...", "createdAt": "..." },
  "currentGoal": { "goalId": 1, "facultyId": 1, "goalText": "...", "createdAt": "..." }
}
```

---

### 📌 2.3 إنشاء كلية جديدة

```
POST /api/universities/{id}/faculties
```

**🔐 يحتاج: `مشرف عام` أو `إداري الجامعة` (لجامعته فقط)**

**Request Body:**
```json
{
  "facultyName": "كلية الطب",
  "deanName": "د. محمد علي",
  "email": "medicine@uni.edu",
  "phone": "+20xxxxxxxxxx",
  "description": "وصف الكلية",
  "logoUrl": "https://...",
  "hasPreparatoryYear": true
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `facultyName` | string | ✅ | اسم الكلية |
| `deanName` | string | ❌ | اسم العميد |
| `email` | string | ❌ | البريد الإلكتروني |
| `phone` | string | ❌ | الهاتف |
| `description` | string | ❌ | وصف الكلية |
| `logoUrl` | string | ❌ | رابط الشعار |
| `hasPreparatoryYear` | bool | ❌ | هل تحتوي سنة تحضيرية |

**Response `201 Created`:** بيانات الكلية المُنشأة

---

### 📌 2.4 تحديث كلية

```
PUT /api/universities/faculties/{facultyId}
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من ملكية الكلية)**

> **ملاحظة:** `إداري الجامعة` يمكنه تعديل كليات جامعته، `إداري الكلية` يمكنه تعديل كليته فقط.

**Request Body:** نفس حقول الإنشاء + حقول إضافية:
```json
{
  "facultyName": "اسم جديد",
  "deanName": "عميد جديد",
  "email": "new@uni.edu",
  "phone": "...",
  "description": "...",
  "logoUrl": "...",
  "hasPreparatoryYear": false,
  "universityId": 2,
  "vision": "رؤية جديدة (تُضاف كسجل جديد)",
  "mission": "رسالة جديدة",
  "goals": "هدف جديد 1|هدف جديد 2"
}
```

> **📢 إشعار تلقائي:** عند تحديث الكلية، يُرسَل إشعار تلقائياً لـ `إداري الكلية` المرتبطين.

**Response `200 OK`:** بيانات الكلية المُحدّثة

---

### 📌 2.5 حذف كلية

```
DELETE /api/universities/faculties/{facultyId}
```

**🔐 يحتاج: `مشرف عام` أو `إداري الجامعة` (لجامعته فقط)**

**Response `200 OK`:** `{ "success": true, "message": "Faculty deleted successfully" }`

---

### 📌 2.6 APIs رؤية الكلية (Faculty Vision/Mission/Goal)

**الرؤية:**
| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/api/faculties/{id}/vision/current` | 🔐 أي مستخدم | الرؤية الحالية |
| `GET` | `/api/faculties/{id}/visions` | 🔐 أي مستخدم | كل الرؤى |
| `POST` | `/api/faculties/{id}/visions` | 🔐 `AdminOnly` + ملكية | إضافة رؤية |
| `PUT` | `/api/faculties/{id}/visions/{visionId}` | 🔐 `AdminOnly` + ملكية | تعديل رؤية |
| `DELETE` | `/api/faculties/{id}/visions/{visionId}` | 🔐 `AdminOnly` + ملكية | حذف رؤية |

**الرسالة:** نفس الـ pattern مع `/missions/` و `missionText`

**الأهداف:** نفس الـ pattern مع `/goals/` و `goalText`

**POST Request Body (Vision):**
```json
{ "visionText": "رؤية الكلية" }
```

---

## 3. Program Types - أنواع البرامج

**Base Route:** `/api/v1/programtypes` أو `/api/v1/faculties/{facultyId}/program-types`

---

### 📌 3.1 عرض كل أنواع البرامج

```
GET /api/v1/programtypes
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:**
```json
[
  {
    "programTypeId": 1,
    "facultyId": 2,
    "name": "بكالوريوس",
    "isActive": true,
    "createdAt": "...",
    "updatedAt": "..."
  }
]
```

---

### 📌 3.2 عرض أنواع برامج كلية معينة

```
GET /api/v1/faculties/{facultyId}/program-types
```

**🔓 لا يحتاج مصادقة**

**Response:** نفس الشكل السابق، مُصفَّى حسب `facultyId`

---

### 📌 3.3 عرض نوع برنامج بالتفصيل

```
GET /api/v1/programtypes/{id}
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:** كائن `ProgramTypeDto` واحد

---

### 📌 3.4 إنشاء نوع برنامج

```
POST /api/v1/faculties/{facultyId}/program-types
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{ "name": "دراسات عليا" }
```

**Response `201 Created`:** بيانات نوع البرنامج المُنشأ

---

### 📌 3.5 تحديث نوع برنامج

```
PUT /api/v1/programtypes/{id}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{
  "name": "اسم جديد",
  "isActive": false
}
```

**Response `200 OK`:** بيانات نوع البرنامج المُحدّث

---

### 📌 3.6 حذف نوع برنامج

```
DELETE /api/v1/programtypes/{id}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

> **⚠️ مهم:** لا يمكن الحذف إذا كان هناك برامج مرتبطة بهذا النوع.

**Response `200 OK`:** `{ "success": true, "message": "Program type deleted successfully" }`

**Response `400`:** `{ "error": "Cannot delete program type that has associated programs..." }`

---

## 4. Programs - البرامج الأكاديمية

**Base Route:** `/api/v1/programs` أو `/api/v1/faculties/{facultyId}/programs`

---

### 📌 4.1 عرض كل البرامج

```
GET /api/v1/programs
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:**
```json
[
  {
    "programId": 1,
    "facultyId": 2,
    "facultyName": "كلية الهندسة",
    "universityName": "جامعة القاهرة",
    "programTypeId": 1,
    "programTypeName": "بكالوريوس",
    "name": "هندسة الحاسبات",
    "code": "CS2024",
    "description": "...",
    "yearsOfStudy": 4,
    "isActive": true,
    "createdAt": "...",
    "updatedAt": "..."
  }
]
```

---

### 📌 4.2 عرض برامج كلية معينة

```
GET /api/v1/faculties/{facultyId}/programs
```

**🔓 لا يحتاج مصادقة**

**Response:** نفس الشكل السابق مُصفَّى حسب `facultyId`

---

### 📌 4.3 عرض برامج حسب الكلية والنوع

```
GET /api/v1/faculties/{facultyId}/program-types/{programTypeId}/programs
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response:** نفس الشكل مُصفَّى حسب `facultyId` و `programTypeId`

---

### 📌 4.4 عرض برنامج بالتفصيل

```
GET /api/v1/programs/{id}
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response:** كائن `ProgramDto` واحد

---

### 📌 4.5 إنشاء برنامج

```
POST /api/v1/faculties/{facultyId}/programs
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{
  "name": "هندسة الاتصالات",
  "programTypeId": 1,
  "code": "EE2024",
  "description": "وصف البرنامج",
  "yearsOfStudy": 5
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | اسم البرنامج |
| `programTypeId` | int | ✅ | معرّف نوع البرنامج (يجب أن يتبع نفس الكلية) |
| `code` | string | ❌ | كود البرنامج (فريد داخل الكلية) |
| `description` | string | ❌ | وصف البرنامج |
| `yearsOfStudy` | int | ❌ | عدد سنوات الدراسة |

> **📢 إشعار تلقائي:** عند إنشاء البرنامج، يُرسَل إشعار للـ `أكاديمي` في الكلية.

**Response `201 Created`:** بيانات البرنامج المُنشأ

**Response `400`:** إذا كان الكود مكرراً أو نوع البرنامج لا ينتمي لهذه الكلية

---

### 📌 4.6 تحديث برنامج

```
PUT /api/v1/programs/{id}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{
  "name": "اسم جديد",
  "programTypeId": 2,
  "code": "CS2025",
  "description": "وصف جديد",
  "yearsOfStudy": 4,
  "isActive": true
}
```

**Response `200 OK`:** بيانات البرنامج المُحدّث

---

### 📌 4.7 حذف برنامج

```
DELETE /api/v1/programs/{id}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Response `200 OK`:** `{ "success": true, "message": "Program deleted successfully" }`

---

## 5. Departments - الأقسام

**Routes:**
- `POST /api/programs/{programId}/departments`
- `GET /api/programs/{programId}/departments`
- `GET /api/departments/{departmentId}`
- `PUT /api/departments/{departmentId}`
- `PATCH /api/departments/{departmentId}/status`
- `DELETE /api/departments/{departmentId}`

---

### 📌 5.1 عرض أقسام برنامج معين

```
GET /api/programs/{programId}/departments
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:**
```json
[
  {
    "departmentId": 1,
    "name": "قسم البرمجيات",
    "faculty": "كلية الهندسة",
    "university": "جامعة القاهرة",
    "program": "هندسة الحاسبات",
    "studentsCount": 0,
    "professorsCount": 0,
    "coursesCount": 0,
    "isActive": true
  }
]
```

---

### 📌 5.2 عرض قسم بالتفصيل

```
GET /api/departments/{departmentId}
```

**🔐 يحتاج مصادقة (أي مستخدم)**

**Response `200 OK`:**
```json
{
  "departmentId": 1,
  "name": "قسم البرمجيات",
  "faculty": "كلية الهندسة",
  "university": "جامعة القاهرة",
  "program": "هندسة الحاسبات",
  "headName": "د. محمد أحمد",
  "description": "وصف القسم",
  "studentsCount": 0,
  "professorsCount": 0,
  "coursesCount": 0,
  "isActive": true,
  "createdAt": "...",
  "updatedAt": "..."
}
```

---

### 📌 5.3 إنشاء قسم

```
POST /api/programs/{programId}/departments
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{
  "name": "قسم الشبكات",
  "headName": "د. خالد محمود",
  "description": "وصف القسم"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | اسم القسم |
| `headName` | string | ❌ | رئيس القسم |
| `description` | string | ❌ | وصف القسم |

**Response `201 Created`:** بيانات القسم المُنشأ

---

### 📌 5.4 تحديث قسم

```
PUT /api/departments/{departmentId}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{
  "name": "اسم جديد",
  "headName": "رئيس جديد",
  "description": "وصف جديد",
  "isActive": true
}
```

**Response `200 OK`:** بيانات القسم المُحدّث

---

### 📌 5.5 تحديث حالة قسم فقط (Active/Inactive)

```
PATCH /api/departments/{departmentId}/status
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Request Body:**
```json
{ "isActive": false }
```

**Response `200 OK`:**
```json
{
  "departmentId": 1,
  "name": "قسم الشبكات",
  "faculty": "...",
  "university": "...",
  "program": "...",
  "isActive": false,
  "message": "Department status updated successfully"
}
```

---

### 📌 5.6 حذف قسم

```
DELETE /api/departments/{departmentId}
```

**🔐 يحتاج: `AdminOnly` (مع ملكية الكلية)**

**Response `200 OK`:**
```json
{
  "message": "Department deleted successfully",
  "departmentId": 1,
  "name": "قسم الشبكات",
  "faculty": "...",
  "university": "...",
  "program": "..."
}
```

---

## 6. Standard Types - أنواع المعايير

**Base Route:** `/api/standardtypes`

---

### 📌 6.1 عرض كل أنواع المعايير

```
GET /api/standardtypes
```

**🔓 لا يحتاج مصادقة**

**Response `200 OK`:**
```json
[
  {
    "standardTypeId": 1,
    "name": "معايير الجودة",
    "createdAt": "..."
  }
]
```

---

### 📌 6.2 عرض نوع معيار بالتفصيل

```
GET /api/standardtypes/{id}
```

**🔓 لا يحتاج مصادقة**

**Response:** كائن `StandardTypeDto` واحد

---

### 📌 6.3 إنشاء نوع معيار

```
POST /api/standardtypes
```

**🔐 يحتاج: `مشرف عام` فقط (SuperAdminOnly)**

**Request Body:**
```json
{ "name": "معايير الاعتماد الأكاديمي" }
```

**Response `201 Created`:** بيانات نوع المعيار

---

### 📌 6.4 تحديث نوع معيار

```
PUT /api/standardtypes/{id}
```

**🔐 يحتاج: `مشرف عام` فقط (SuperAdminOnly)**

**Request Body:**
```json
{ "name": "اسم محدّث" }
```

**Response `204 No Content`**

---

### 📌 6.5 حذف نوع معيار

```
DELETE /api/standardtypes/{id}
```

**🔐 يحتاج: `مشرف عام` فقط (SuperAdminOnly)**

> **⚠️ مهم:** لا يمكن الحذف إذا كان هناك معايير مرتبطة بهذا النوع.

**Response `204 No Content`**

**Response `400`:** `{ "error": "Cannot delete type while it has associated standards." }`

---

## 7. Standards - المعايير

**Base Route:** `/api/standards`

> **💡 مفهوم النطاق (Scoping):** المعايير يمكن أن تكون على مستوى:
> - **عالمي (Global):** لا `universityId` ولا `facultyId` — للمشرف العام فقط
> - **جامعة (University):** مرتبطة بـ `universityId`
> - **كلية (Faculty):** مرتبطة بـ `facultyId`
> - **برنامج (Program):** مرتبطة بـ `programId`
> - **مقرر (Course):** مرتبطة بـ `courseId`

---

### 📌 7.1 عرض قائمة المعايير (مع فلترة)

```
GET /api/standards
```

**🔓 لا يحتاج مصادقة**

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `typeId` | int | فلترة حسب نوع المعيار |
| `universityId` | int | فلترة حسب الجامعة |
| `facultyId` | int | فلترة حسب الكلية |
| `programId` | int | فلترة حسب البرنامج |
| `courseId` | int | فلترة حسب المقرر |
| `search` | string | بحث بالاسم أو الكود |

**Example:**
```http
GET /api/standards?facultyId=1&typeId=2&search=جودة
```

**Response `200 OK`:**
```json
[
  {
    "standardId": 1,
    "name": "جودة التدريس",
    "standardCode": "QT-001",
    "standardTypeId": 1,
    "standardTypeName": "معايير الجودة",
    "universityId": null,
    "facultyId": 1,
    "programId": null,
    "courseId": null,
    "isActive": true,
    "createdAt": "...",
    "indicators": [
      {
        "indicatorId": 1,
        "title": "نسبة التفاعل",
        "indicatorCode": "IND-001",
        "sortOrder": 1,
        "standardId": 1,
        "isActive": true,
        "createdAt": "..."
      }
    ]
  }
]
```

---

### 📌 7.2 عرض معيار بالتفصيل

```
GET /api/standards/{id}
```

**🔓 لا يحتاج مصادقة**

**Response:** نفس شكل قائمة المعايير لكائن واحد (يشمل المؤشرات)

---

### 📌 7.3 إنشاء معيار

```
POST /api/standards
```

**🔐 يحتاج: `AdminOnly`**

> **منطق الصلاحية حسب النطاق:**
> - إذا كان `facultyId` موجوداً → يجب أن يكون لديك صلاحية على الكلية
> - إذا كان فقط `universityId` → يجب أن تكون `إداري الجامعة` لتلك الجامعة أو `مشرف عام`
> - إذا لم يوجد لا `facultyId` ولا `universityId` → `مشرف عام` فقط

**Request Body:**
```json
{
  "name": "جودة المناهج",
  "standardCode": "CQ-001",
  "standardTypeId": 1,
  "universityId": null,
  "facultyId": 2,
  "programId": null,
  "courseId": null,
  "indicators": [
    { "title": "مراجعة المنهج سنوياً", "indicatorCode": "IND-01", "sortOrder": 1 },
    { "title": "توافق مع المعايير الدولية", "indicatorCode": "IND-02", "sortOrder": 2 }
  ]
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string | ✅ | اسم المعيار |
| `standardCode` | string | ❌ | كود المعيار |
| `standardTypeId` | int | ✅ | معرّف نوع المعيار |
| `universityId` | int? | ❌ | نطاق الجامعة |
| `facultyId` | int? | ❌ | نطاق الكلية |
| `programId` | int? | ❌ | نطاق البرنامج |
| `courseId` | int? | ❌ | نطاق المقرر |
| `indicators` | array | ❌ | مؤشرات أولية (اختيارية) |

**Response `201 Created`:** معرّف المعيار المُنشأ

---

### 📌 7.4 تحديث معيار

```
PUT /api/standards/{id}
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من نطاق الملكية)**

**Request Body:**
```json
{
  "name": "اسم محدّث",
  "standardCode": "CQ-002",
  "standardTypeId": 2,
  "universityId": null,
  "facultyId": 2,
  "programId": null,
  "courseId": null,
  "isActive": true
}
```

> **📢 إشعار تلقائي:**
> - إذا مرتبط بكلية → يُرسَل لـ `أكاديمي` و`إداري الكلية` في تلك الكلية
> - إذا مرتبط بجامعة → يُرسَل لـ `مشرف عام` و`إداري الجامعة`

**Response `204 No Content`**

---

### 📌 7.5 حذف معيار

```
DELETE /api/standards/{id}
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من نطاق الملكية)**

**Response `204 No Content`**

---

## 8. Indicators - المؤشرات

**Base Route:** `/api/indicators`

---

### 📌 8.1 عرض قائمة المؤشرات

```
GET /api/indicators
```

**🔓 لا يحتاج مصادقة**

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `standardId` | int | فلترة حسب المعيار |

**Example:**
```http
GET /api/indicators?standardId=5
```

**Response `200 OK`:**
```json
[
  {
    "indicatorId": 1,
    "title": "نسبة التفاعل الطلابي",
    "indicatorCode": "IND-001",
    "sortOrder": 1,
    "standardId": 5,
    "isActive": true,
    "createdAt": "..."
  }
]
```

---

### 📌 8.2 عرض مؤشر بالتفصيل

```
GET /api/indicators/{id}
```

**🔓 لا يحتاج مصادقة**

**Response:** كائن `IndicatorDto` واحد

---

### 📌 8.3 إنشاء مؤشر

```
POST /api/indicators
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من ملكية المعيار)**

**Request Body:**
```json
{
  "title": "مستوى رضا الطلاب",
  "indicatorCode": "IND-010",
  "sortOrder": 3,
  "standardId": 5
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `title` | string | ✅ | عنوان المؤشر |
| `standardId` | int | ✅ | معرّف المعيار الأب |
| `indicatorCode` | string | ❌ | كود المؤشر |
| `sortOrder` | int | ❌ | ترتيب العرض (افتراضي: 0) |

> **📢 إشعار تلقائي:**
> - إذا المعيار مرتبط بكلية → يُرسَل إشعار لـ `أكاديمي` و`إداري الكلية`
> - إذا المعيار مرتبط بجامعة → يُرسَل لـ `مشرف عام` و`إداري الجامعة`

**Response `201 Created`:** بيانات المؤشر المُنشأ

---

### 📌 8.4 تحديث مؤشر

```
PUT /api/indicators/{id}
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من ملكية المعيار)**

**Request Body:**
```json
{
  "title": "عنوان جديد",
  "indicatorCode": "IND-011",
  "sortOrder": 5,
  "isActive": true
}
```

**Response `204 No Content`**

---

### 📌 8.5 حذف مؤشر

```
DELETE /api/indicators/{id}
```

**🔐 يحتاج: `AdminOnly` (مع التحقق من ملكية المعيار)**

**Response `204 No Content`**

---

## 🗺️ خريطة العلاقات (Entity Hierarchy)

```
University (جامعة)
├── Vision / Mission / Goals
└── Faculty (كلية)
    ├── Vision / Mission / Goals
    └── ProgramType (نوع البرنامج)
        └── Program (برنامج أكاديمي)
            └── Department (قسم)

Standard (معيار) ─── StandardType (نوع المعيار)
├── يمكن ربطه بـ: University / Faculty / Program / Course
└── Indicators (مؤشرات)
```

---

## ⚡ جدول موجز للـ Endpoints

### Universities
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/universities` | 🔓 | قائمة الجامعات |
| GET | `/api/universities/{id}` | 🔐 أي مستخدم | تفاصيل جامعة |
| POST | `/api/universities` | 🔐 SuperAdmin | إنشاء جامعة |
| PUT | `/api/universities/{id}` | 🔐 SuperAdmin / UniAdmin | تحديث جامعة |
| DELETE | `/api/universities/{id}` | 🔐 SuperAdmin | حذف جامعة |

### Faculties
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/universities/{id}/faculties` | 🔓 | كليات جامعة |
| GET | `/api/universities/faculties/{facultyId}` | 🔐 أي مستخدم | تفاصيل كلية |
| POST | `/api/universities/{id}/faculties` | 🔐 SuperAdmin / UniAdmin | إنشاء كلية |
| PUT | `/api/universities/faculties/{facultyId}` | 🔐 AdminOnly | تحديث كلية |
| DELETE | `/api/universities/faculties/{facultyId}` | 🔐 SuperAdmin / UniAdmin | حذف كلية |

### Program Types
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/v1/programtypes` | 🔐 أي مستخدم | كل الأنواع |
| GET | `/api/v1/faculties/{id}/program-types` | 🔓 | أنواع كلية |
| GET | `/api/v1/programtypes/{id}` | 🔐 أي مستخدم | نوع بعينه |
| POST | `/api/v1/faculties/{id}/program-types` | 🔐 AdminOnly | إنشاء نوع |
| PUT | `/api/v1/programtypes/{id}` | 🔐 AdminOnly | تحديث نوع |
| DELETE | `/api/v1/programtypes/{id}` | 🔐 AdminOnly | حذف نوع |

### Programs
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/v1/programs` | 🔐 أي مستخدم | كل البرامج |
| GET | `/api/v1/faculties/{id}/programs` | 🔓 | برامج كلية |
| GET | `/api/v1/faculties/{fId}/program-types/{tId}/programs` | 🔐 أي مستخدم | برامج حسب النوع |
| GET | `/api/v1/programs/{id}` | 🔐 أي مستخدم | تفاصيل برنامج |
| POST | `/api/v1/faculties/{id}/programs` | 🔐 AdminOnly | إنشاء برنامج |
| PUT | `/api/v1/programs/{id}` | 🔐 AdminOnly | تحديث برنامج |
| DELETE | `/api/v1/programs/{id}` | 🔐 AdminOnly | حذف برنامج |

### Departments
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/programs/{programId}/departments` | 🔐 أي مستخدم | أقسام برنامج |
| GET | `/api/departments/{departmentId}` | 🔐 أي مستخدم | تفاصيل قسم |
| POST | `/api/programs/{programId}/departments` | 🔐 AdminOnly | إنشاء قسم |
| PUT | `/api/departments/{departmentId}` | 🔐 AdminOnly | تحديث قسم |
| PATCH | `/api/departments/{departmentId}/status` | 🔐 AdminOnly | تفعيل/تعطيل قسم |
| DELETE | `/api/departments/{departmentId}` | 🔐 AdminOnly | حذف قسم |

### Standard Types
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/standardtypes` | 🔓 | كل الأنواع |
| GET | `/api/standardtypes/{id}` | 🔓 | نوع بعينه |
| POST | `/api/standardtypes` | 🔐 SuperAdmin | إنشاء نوع |
| PUT | `/api/standardtypes/{id}` | 🔐 SuperAdmin | تحديث نوع |
| DELETE | `/api/standardtypes/{id}` | 🔐 SuperAdmin | حذف نوع |

### Standards
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/standards` | 🔓 | قائمة المعايير (مع فلاتر) |
| GET | `/api/standards/{id}` | 🔓 | تفاصيل معيار |
| POST | `/api/standards` | 🔐 AdminOnly | إنشاء معيار |
| PUT | `/api/standards/{id}` | 🔐 AdminOnly | تحديث معيار |
| DELETE | `/api/standards/{id}` | 🔐 AdminOnly | حذف معيار |

### Indicators
| Method | Endpoint | Auth | Action |
|---|---|---|---|
| GET | `/api/indicators` | 🔓 | قائمة المؤشرات |
| GET | `/api/indicators/{id}` | 🔓 | تفاصيل مؤشر |
| POST | `/api/indicators` | 🔐 AdminOnly | إنشاء مؤشر |
| PUT | `/api/indicators/{id}` | 🔐 AdminOnly | تحديث مؤشر |
| DELETE | `/api/indicators/{id}` | 🔐 AdminOnly | حذف مؤشر |

---

## 🚨 الأخطاء الشائعة (Error Codes)

| HTTP Status | معنى | السبب الشائع |
|---|---|---|
| `400 Bad Request` | طلب غير صحيح | حقل مطلوب ناقص، أو كود مكرر |
| `401 Unauthorized` | غير مصادق | JWT مفقود أو منتهي الصلاحية |
| `403 Forbidden` | ممنوع | المستخدم لا يملك صلاحية على هذا المورد |
| `404 Not Found` | غير موجود | المعرّف المطلوب غير موجود في قاعدة البيانات |
| `500 Internal Server Error` | خطأ في السيرفر | خطأ داخلي، تحقق من الـ logs |

---

## 💡 نصائح للمطورين

### Frontend (React/Vue/Angular)
- استخدم **Axios** أو **Fetch API** لإرسال الطلبات.
- احفظ الـ JWT في `localStorage` أو `sessionStorage` وأضفه في كل طلب.
- للفلترة والبحث، استخدم `query params` مباشرة في الـ URL.
- عند استقبال `403`، أعِد توجيه المستخدم لصفحة "غير مصرح".
- عند الحصول على `401`، أعِد تسجيل الدخول أو جدّد الـ Token.

### Windows Forms (.NET)
- استخدم `HttpClient` مع `DefaultRequestHeaders.Authorization`.
- استخدم `JsonConvert` من **Newtonsoft.Json** أو `System.Text.Json` لـ Deserialization.
- احفظ الـ Token في `Application.UserAppDataPath` أو `Settings`.
- تعامل مع `HttpResponseMessage.StatusCode` لمعالجة الأخطاء.

```csharp
// مثال: جلب قائمة الجامعات
var client = new HttpClient();
client.DefaultRequestHeaders.Authorization = 
    new AuthenticationHeaderValue("Bearer", "YOUR_TOKEN");

var response = await client.GetAsync("https://your-server/api/universities");
var json = await response.Content.ReadAsStringAsync();
```

### Mobile (Flutter/React Native/Android)
- استخدم **Dio** (Flutter) أو **Axios** (React Native) أو **Retrofit** (Android).
- احفظ الـ Token بأمان في **Secure Storage** / **Keychain**.
- تعامل مع حالات انتهاء الصلاحية وأعِد المصادقة تلقائياً.
- للبيانات الكبيرة، استخدم **Pagination** عبر `page` و `limit`.

```dart
// مثال: Flutter + Dio
final dio = Dio();
dio.options.headers['Authorization'] = 'Bearer $token';

final response = await dio.get('/api/universities', 
  queryParameters: {'page': 1, 'limit': 10});
```
