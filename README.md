# 🎓 Csag_AndStd (Calculate Semester Average Grade)

> **Dự án:** Ứng dụng tính điểm trung bình cộng học kỳ.
> 
> **Họ - tên:** Lường Văn Hạnh.
> 
> **Mã số Sinh viên:** K225480106013.

---

## 📌 1. Giới Thiệu Chung
**Csag_AndStd** là ứng dụng Android được xây dựng trên ngôn ngữ **Java (JDK 17)** nhằm giải quyết bài toán tính điểm trung bình học kỳ một cách linh hoạt. Không giống như các ứng dụng fix cứng số lượng môn học, ứng dụng này cho phép người dùng tự động thêm/bớt số lượng hàng (môn học) tùy ý, tự động lưu trữ trạng thái và gửi báo cáo kết quả lên máy chủ của giảng viên.
<img width="959" height="298" alt="image" src="https://github.com/user-attachments/assets/5ea254bc-cd1a-4969-a60f-2c695bb3ff33" />

---

## 🛠️ 2. Công Nghệ Sử Dụng
* **Ngôn ngữ phát triển:** Java (JDK 17)
* **Công cụ phát triển:** Android Studio (Cấu trúc build: `build.gradle.kts` - Kotlin DSL)
* **Thư viện kết nối mạng:** Retrofit 2 (v2.9.0) & Gson Converter (v2.9.0)
* **Kiến trúc giao diện:** `RecyclerView` kết hợp `Custom TextWatcher` để lắng nghe sự kiện nhập liệu thời gian thực (Real-time input tracking).
* **Lưu trữ dữ liệu cục bộ:** `SharedPreferences` phối hợp cơ chế Serialization/Deserialization của `Gson` để đồng bộ trạng thái ứng dụng.
<img width="959" height="383" alt="image" src="https://github.com/user-attachments/assets/6cc59259-79b1-4461-83e1-ab065dcbc02e" />

---

## 📑 3. Kiến Trúc Hệ Thống (Các Màn Hình Chức Năng)

Ứng dụng được chia làm 3 phân hệ giao diện chính (`Activity`):

* **Giao diện 1 (About Activity):** Hiển thị thông tin cá nhân sinh viên, mã số sinh viên, lớp học. Đóng vai trò là cổng điều hướng sang các tính năng cốt lõi qua hệ thống `Intent`.
<img width="959" height="404" alt="image" src="https://github.com/user-attachments/assets/4f794825-e1bc-4c99-82d8-f01940ee08f3" />

* **Giao diện 2 (Calculation Activity):** Bảng tính điểm động sử dụng `RecyclerView`. Mặc định hiển thị 3 môn học, hỗ trợ nút thêm/bớt dòng, tự động tính điểm TBC theo công thức:
    $$Điểm TBC = \frac{\sum (Điểm TBMH_i \times Số Tín Chỉ_i)}{\sum Số Tín Chỉ_i}$$
    Đồng thời tự động phân loại học lực (Xuất sắc, Giỏi, Khá, Trung bình) và xét học bổng.
<img width="959" height="401" alt="image" src="https://github.com/user-attachments/assets/959006df-fe9e-4a9f-a786-903732e746ed" />

  <img width="959" height="370" alt="image" src="https://github.com/user-attachments/assets/c6a8fd2e-5875-41b4-a94e-81e35b4001f4" />

  <img width="959" height="405" alt="image" src="https://github.com/user-attachments/assets/1c2f90be-8d91-463c-81ff-fc637d04c735" />

* **Giao diện 3 (WebView Activity):** Tích hợp trình duyệt nhúng Webview, tự động gán mã sinh viên vào URL `https://k58kmt.tdh.io.vn?masv=K225480106013` để truy cập hệ thống chấm điểm trực tuyến.
<img width="959" height="388" alt="image" src="https://github.com/user-attachments/assets/d9d22341-8c8b-4a60-9b83-286bbed82bf9" />

File AndroidManifest như sau:
```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@drawable/iconapp2"
        android:label="@string/app_name"
        android:roundIcon="@drawable/iconapp2"
        android:supportsRtl="true"
        android:theme="@style/Theme.Csag"
        android:usesCleartextTraffic="true"
        tools:targetApi="34">

        <activity
            android:name=".Activity1"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity
            android:name=".Activity2"
            android:exported="false" />

        <activity
            android:name=".Activity3"
            android:exported="false" />

    </application>

</manifest>
```
---

## 🚀 4. Quá Trình Thực Hiện Bài Tập

Quá trình nghiên cứu và hoàn thiện ứng dụng trải qua 5 giai đoạn cốt lõi:

### Giai đoạn 1: Thiết kế cơ sở dữ liệu và Model Object
* Xây dựng cấu trúc đối tượng `MonHoc.java` để quản lý các thuộc tính động: Tên môn (`String`), Số tín chỉ (`int`), Điểm số (`double`).
<img width="959" height="283" alt="image" src="https://github.com/user-attachments/assets/201dd6da-185b-41fe-ac8c-addf75b0603b" />

```java
package com.example.csag;

public class MonHoc {
    public String tenMon = "";
    public int tinChi = 0;
    public double diem = 0.0;
}
```
### Giai đoạn 2: Tối ưu hóa giao diện người dùng (UI/UX) và Đóng gói Tài nguyên
* Quy hoạch lại toàn bộ hệ thống màu sắc (`colors.xml`) theo ngôn ngữ Material Design (Deep Teal & Mint làm chủ đạo).
* Thiết kế các tệp tài nguyên đồ họa `shape` để bo tròn góc ô nhập liệu, tạo khối Card thông tin (`bg_student_card.xml`, `bg_table_header.xml`) giúp giao diện có chiều sâu.
* Chuyển dịch toàn bộ chuỗi ký tự hiển thị sang tệp quản lý tập trung `strings.xml` để đảm bảo tính bao đóng và tối ưu hóa mã nguồn.
<img width="959" height="396" alt="image" src="https://github.com/user-attachments/assets/11a71b87-9c51-4183-9654-32a2352cf2e0" />

### Giai đoạn 3: Phát triển giải pháp bảng điểm động (RecyclerView & Adapter)
* Xây dựng `MonHocAdapter.java` kết hợp `CustomTextWatcher`. Giải quyết triệt để lỗi mất dữ liệu hoặc sai lệch dữ liệu khi người dùng cuộn danh sách (`Recycle ViewHolder view recycling block`).
<img width="959" height="396" alt="image" src="https://github.com/user-attachments/assets/830b8aa5-27cc-4c2a-b350-eb0bdac3aca7" />

### Giai đoạn 4: Cấu trúc JSON động và Kết nối API RESTful
* Sử dụng cấu trúc `Map<String, Object>` lồng nhau để thiết lập Payload JSON động, giải quyết bài toán số lượng trường (`Mon1, Mon2, ... MonN`) biến thiên theo số hàng người dùng thêm vào.
* Khởi tạo `ApiService` kết nối Endpoint qua Retrofit, xử lý bất đồng bộ kết quả trả về (`enqueue`) để lấy số thứ tự ghi nhận (`STT`) từ Server.

### Giai đoạn 5: Xử lý cơ chế bất tử dữ liệu (State Persistence)
* Ghi đè vòng đời Activity tại hàm `onPause()`. Khi người dùng thoát màn hình hoặc đóng ứng dụng, danh sách môn học và kết quả tính toán sẽ lập tức được Gson chuyển đổi thành chuỗi JSON String và nạp vào bộ nhớ Flash thông qua `SharedPreferences`. Dữ liệu sẽ tự động phục hồi ở hàm `onCreate()` trong lần truy cập tiếp theo.
<img width="959" height="391" alt="image" src="https://github.com/user-attachments/assets/772e2a00-d21e-4ade-957d-348a4e9f9967" />

---

## 📸 5. Minh Chứng Kết Quả (Screenshots)

<img width="360" height="720" alt="Screenshot_2026-06-08-11-41-13-227_com miui home" src="https://github.com/user-attachments/assets/719adfcd-93eb-4d81-b44e-6bf1c63eb4d0" />
<img width="360" height="720" alt="Screenshot_2026-06-08-11-41-23-606_com example csag" src="https://github.com/user-attachments/assets/0e5100b2-1a52-472f-b7c1-533802744bf3" />
<img width="360" height="720" alt="Screenshot_2026-06-08-11-46-16-959_com example csag" src="https://github.com/user-attachments/assets/ce337287-def3-44fc-9756-10d7f8e2c232" />

Chỗ này đang gọi API lỗi, do api không truy cập được, vì web hiện tại không hoạt động

<img width="360" height="720" alt="Screenshot_2026-06-08-11-43-19-962_com example csag" src="https://github.com/user-attachments/assets/92de8e52-f222-45ba-af7c-360dd0068478" />

API đã ok rồi;

<img  width="360" height="720" alt="Screenshot_2026-06-09-19-06-17-788_com example csag" src="https://github.com/user-attachments/assets/773aa383-63de-4d39-9ae6-d5d7d2fc4c36" />

### Cơ chế lưu trữ: Trạng thái dữ liệu được giữ nguyên khi thoát ra vào lại
<img width="360" height="720" alt="Screenshot_2026-06-08-11-43-41-476_com example csag" src="https://github.com/user-attachments/assets/9e66fc9a-98ce-439d-8951-2f41d6818c68" />

Đang lỗi do trang web hiện tại không hoạt động

<img width="360" height="720" alt="Screenshot_2026-06-08-11-43-53-851_com example csag" src="https://github.com/user-attachments/assets/6d200619-3318-46d0-b748-675f331e9e70" />

Trang đích đã ok rồi;

<img  width="360" height="720" alt="Screenshot_2026-06-09-19-04-59-471_com example csag" src="https://github.com/user-attachments/assets/8b230c78-15e4-4de3-99ed-279ae99bb0a9" />

---

## 📦 6. Cấu trúc Payload JSON động gửi lên Server
Dưới đây là cấu trúc dữ liệu thực tế được ứng dụng tự động đóng gói và truyền tải qua phương thức POST:

```json
{
  "app_by": "K225480106013",
  "input": {
    "KhoaHoc": "Khóa 58",
    "Mon1": "Cấu trúc dữ liệu và giải thuật",
    "TC_Mon1": 3,
    "Mon2": "Mạng máy tính",
    "TC_Mon2": 3,
    "Mon3": "Công nghệ phần mềm",
    "TC_Mon3": 3,
    "Mon3": "Cơ sở dữ liệu",
    "TC_Mon3": 3,
    "Mon3": "Phân tích thiết kế hệ thống",
    "TC_Mon3": 3
  },
  "output": {
    "TBC": 8.7,
    "KetLuan": "Xếp loại Giỏi",
    "XetHocBong": "Đủ điều kiện học bổng loại Giỏi"
  }
}
