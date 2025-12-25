# Tài liệu Mối quan hệ giữa các File CSV trong Dataset

## 🎯 Mục đích

Tài liệu này giải thích **cách các file CSV trong dataset kết nối với nhau** thông qua các trường dữ liệu chung (keys). Đây là cốt lõi để hiểu cách dữ liệu được tổ chức và liên kết.

---

## 📁 Cấu trúc Dataset

Xem chi tiết trong file **[PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md)**

### Tóm tắt:

```
dataset/
├── ids_relationship.csv                    # File liên kết chính
├── institutions/
│   ├── identifiers.csv                    # Danh sách Institution IDs
│   ├── agg_1_day/0.csv, 1.csv, ..., ETC  # Metrics theo Institution (theo ngày)
│   ├── agg_1_hour/0.csv, 1.csv, ..., ETC  # Metrics theo Institution (theo giờ)
│   └── agg_10_minutes/0.csv, 1.csv, ..., ETC  # Metrics theo Institution (theo 10 phút)
├── institution_subnets/
│   ├── identifiers.csv                    # Danh sách Subnet IDs
│   ├── agg_1_day/0.csv, 1.csv, ..., ETC  # Metrics theo Subnet (theo ngày)
│   ├── agg_1_hour/0.csv, 1.csv, ..., ETC  # Metrics theo Subnet (theo giờ)
│   └── agg_10_minutes/0.csv, 1.csv, ..., ETC  # Metrics theo Subnet (theo 10 phút)
├── ip_addresses_sample/
│   ├── identifiers.csv                    # Danh sách IP IDs (1000 IPs)
│   ├── agg_1_day/520007.csv, 1781410.csv, ..., ETC  # Metrics theo IP (theo ngày)
│   ├── agg_1_hour/520007.csv, 1781410.csv, ..., ETC  # Metrics theo IP (theo giờ)
│   └── agg_10_minutes/520007.csv, 1781410.csv, ..., ETC  # Metrics theo IP (theo 10 phút)
├── ip_addresses_full/                     # Dataset đầy đủ (rất nhiều IPs)
│   └── ... (tương tự nhưng có subfolders)
├── times/
│   ├── times_1_day.csv                    # Time dimension (theo ngày)
│   ├── times_1_hour.csv                   # Time dimension (theo giờ)
│   └── times_10_minutes.csv                # Time dimension (theo 10 phút)
└── weekends_and_holidays.csv               # Ngày cuối tuần và ngày lễ
```

---

## 🔗 Mối quan hệ chính giữa các File

### 1. **File liên kết trung tâm: `ids_relationship.csv`**

**Cấu trúc:**
```
id_ip, id_institution, id_institution_subnet
520007, 53, 236
1781410, 0, 0
...
```

**Vai trò:** File này là **trung tâm kết nối** giữa 3 cấp độ entity:
- Mỗi dòng cho biết: **IP nào** (`id_ip`) thuộc về **Institution nào** (`id_institution`) và **Subnet nào** (`id_institution_subnet`)

**Ví dụ:**
- IP `520007` → thuộc Institution `53` và Subnet `236`
- IP `1781410` → thuộc Institution `0` và Subnet `0`

---

### 2. **File Identifiers (Danh sách ID)**

#### `institutions/identifiers.csv`
```
id_institution
0
1
2
...
```

**Mối quan hệ:**
- Mỗi `id_institution` trong file này tương ứng với **một file metrics** trong thư mục `agg_*`
- Ví dụ: `id_institution = 0` → file `institutions/agg_1_day/0.csv`

#### `institution_subnets/identifiers.csv`
```
id_institution_subnet
0
1
2
...
```

**Mối quan hệ:**
- Mỗi `id_institution_subnet` tương ứng với **một file metrics** trong thư mục `agg_*`
- Ví dụ: `id_institution_subnet = 0` → file `institution_subnets/agg_1_day/0.csv`

#### `ip_addresses_sample/identifiers.csv`
```
id_ip
520007
1781410
...
```

**Mối quan hệ:**
- Mỗi `id_ip` tương ứng với **một file metrics** trong thư mục `agg_*`
- Ví dụ: `id_ip = 520007` → file `ip_addresses_sample/agg_1_day/520007.csv`

---

### 3. **File Metrics - Cấu trúc và Mối quan hệ**

#### 3.1. Institution Metrics

**File:** `institutions/agg_1_day/0.csv`
```
id_time, n_flows, n_packets, n_bytes, ...
0, 17967588, 2009822494, 1937481519695, ...
1, 17222782, 2063503609, 2003213170261, ...
...
```

**Mối quan hệ:**
- **Tên file (`0.csv`)** = `id_institution` từ `institutions/identifiers.csv`
- **Cột `id_time`** → liên kết với `times/times_1_day.csv` để biết thời gian cụ thể
- **Ý nghĩa:** File này chứa metrics của Institution 0, được aggregate theo ngày

**Cách đọc:**
1. File `0.csv` = Institution có `id_institution = 0`
2. Dòng có `id_time = 0` → xem `times/times_1_day.csv`, dòng `id_time = 0` → thời gian là `2023-10-09 00:00:00`
3. Dòng có `id_time = 1` → thời gian là `2023-10-10 00:00:00`

#### 3.2. Institution Subnet Metrics

**File:** `institution_subnets/agg_1_day/0.csv`
```
id_time, n_flows, n_packets, n_bytes, ...
0, 6037276, 1296002787, 1283604855378, ...
...
```

**Mối quan hệ:**
- **Tên file (`0.csv`)** = `id_institution_subnet` từ `institution_subnets/identifiers.csv`
- **Cột `id_time`** → liên kết với `times/times_1_day.csv`
- **Ý nghĩa:** File này chứa metrics của Subnet 0, được aggregate theo ngày

#### 3.3. IP Address Metrics

**File:** `ip_addresses_sample/agg_1_day/520007.csv`
```
id_time, n_flows, n_packets, n_bytes, ...
2, 100, 262626, 293912738, ...
3, 58, 103438, 108472580, ...
...
```

**Mối quan hệ:**
- **Tên file (`520007.csv`)** = `id_ip` từ `ip_addresses_sample/identifiers.csv`
- **Cột `id_time`** → liên kết với `times/times_1_day.csv`
- **Để biết IP này thuộc Institution/Subnet nào:** Xem `ids_relationship.csv`
  - Tìm dòng có `id_ip = 520007` → `id_institution = 53`, `id_institution_subnet = 236`
- **Ý nghĩa:** File này chứa metrics của IP 520007, được aggregate theo ngày

---

### 4. **File Time Dimensions**

#### `times/times_1_day.csv`
```
id_time, time
0, 2023-10-09 00:00:00+00:00
1, 2023-10-10 00:00:00+00:00
2, 2023-10-11 00:00:00+00:00
...
```

**Mối quan hệ:**
- **Cột `id_time`** được sử dụng trong **TẤT CẢ** các file metrics
- Khi file metrics có `id_time = 0` → thời gian tương ứng là `2023-10-09 00:00:00`
- **Áp dụng cho:** Tất cả file trong `*_1_day/` folders

#### `times/times_1_hour.csv`
```
id_time, time
0, 2023-10-09 00:00:00+00:00
1, 2023-10-09 01:00:00+00:00
2, 2023-10-09 02:00:00+00:00
...
```

**Mối quan hệ:**
- Tương tự `times_1_day.csv` nhưng mức độ chi tiết theo **giờ**
- **Áp dụng cho:** Tất cả file trong `*_1_hour/` folders

#### `times/times_10_minutes.csv`
```
id_time, time
0, 2023-10-09 02:03:49+02:00
1, 2023-10-09 02:13:49+02:00
2, 2023-10-09 02:23:49+02:00
...
```

**Mối quan hệ:**
- Mức độ chi tiết theo **10 phút**
- **Áp dụng cho:** Tất cả file trong `*_10_minutes/` folders

---

### 5. **File WeekendHoliday**

#### `weekends_and_holidays.csv`
```
Date, Type
2023-10-14, Weekend
2023-10-15, Weekend
2023-11-17, Holiday
...
```

**Mối quan hệ:**
- **Không có foreign key trực tiếp** với các file khác
- **Join bằng cách:** So sánh `Date` với phần **date** của `time` trong các file `times/*.csv`
- **Ví dụ:**
  - `times/times_1_day.csv` có `time = 2023-10-14 00:00:00`
  - `weekends_and_holidays.csv` có `Date = 2023-10-14`
  - → Match! Ngày này là Weekend

---

## 🔄 Luồng kết nối dữ liệu (Data Flow)

### Ví dụ 1: Tìm metrics của một IP cụ thể

**Bước 1:** Xác định IP thuộc Institution/Subnet nào
```
ids_relationship.csv:
id_ip = 520007 → id_institution = 53, id_institution_subnet = 236
```

**Bước 2:** Đọc metrics của IP
```
ip_addresses_sample/agg_1_day/520007.csv:
id_time = 2 → metrics: n_flows=100, n_packets=262626, ...
```

**Bước 3:** Xác định thời gian
```
times/times_1_day.csv:
id_time = 2 → time = 2023-10-11 00:00:00
```

**Bước 4:** Kiểm tra có phải ngày cuối tuần/ngày lễ không
```
weekends_and_holidays.csv:
Date = 2023-10-11 → Không có trong file → Ngày thường
```

**Kết quả:** IP 520007 vào ngày 2023-10-11 có 100 flows, 262626 packets, ...

---

### Ví dụ 2: So sánh metrics giữa các cấp độ

**Bước 1:** Từ `ids_relationship.csv` biết IP 520007 thuộc:
- Institution 53
- Subnet 236

**Bước 2:** Đọc metrics cùng một thời điểm (`id_time = 2`):

```
IP Level:      ip_addresses_sample/agg_1_day/520007.csv
               id_time=2 → n_flows=100

Subnet Level:  institution_subnets/agg_1_day/236.csv
               id_time=2 → n_flows=?

Institution Level: institutions/agg_1_day/53.csv
                   id_time=2 → n_flows=?
```

**Lưu ý:** Metrics ở cấp Institution/Subnet là **tổng hợp** của tất cả IPs trong đó.

---

### Ví dụ 3: Phân tích theo thời gian

**Bước 1:** Chọn một Institution (ví dụ: Institution 0)

**Bước 2:** Đọc tất cả metrics theo thời gian:
```
institutions/agg_1_day/0.csv:
id_time=0 → n_flows=17967588 (ngày 2023-10-09)
id_time=1 → n_flows=17222782 (ngày 2023-10-10)
id_time=2 → n_flows=19697341 (ngày 2023-10-11)
...
```

**Bước 3:** Xác định ngày cuối tuần:
```
times/times_1_day.csv:
id_time=5 → time=2023-10-14
weekends_and_holidays.csv:
Date=2023-10-14 → Type=Weekend
```

**Kết quả:** Có thể so sánh metrics giữa ngày thường và ngày cuối tuần.

---

## 📊 Bảng tóm tắt Mối quan hệ

| File A | Trường liên kết | File B | Trường liên kết | Mối quan hệ |
|--------|----------------|--------|----------------|-------------|
| `ids_relationship.csv` | `id_ip` | `ip_addresses_sample/identifiers.csv` | `id_ip` | 1:1 (mỗi IP có 1 record) |
| `ids_relationship.csv` | `id_institution` | `institutions/identifiers.csv` | `id_institution` | N:1 (nhiều IP → 1 Institution) |
| `ids_relationship.csv` | `id_institution_subnet` | `institution_subnets/identifiers.csv` | `id_institution_subnet` | N:1 (nhiều IP → 1 Subnet) |
| `institutions/agg_1_day/0.csv` | `id_time` | `times/times_1_day.csv` | `id_time` | N:1 (nhiều metrics → 1 time point) |
| `institution_subnets/agg_1_day/0.csv` | `id_time` | `times/times_1_day.csv` | `id_time` | N:1 |
| `ip_addresses_sample/agg_1_day/520007.csv` | `id_time` | `times/times_1_day.csv` | `id_time` | N:1 |
| `times/times_1_day.csv` | `time` (date part) | `weekends_and_holidays.csv` | `Date` | Date matching (không phải FK) |
| `institutions/agg_1_day/0.csv` | Tên file = `0` | `institutions/identifiers.csv` | `id_institution = 0` | 1:1 (file name = ID) |
| `institution_subnets/agg_1_day/0.csv` | Tên file = `0` | `institution_subnets/identifiers.csv` | `id_institution_subnet = 0` | 1:1 |
| `ip_addresses_sample/agg_1_day/520007.csv` | Tên file = `520007` | `ip_addresses_sample/identifiers.csv` | `id_ip = 520007` | 1:1 |

---

## 🎯 Quy tắc vàng để hiểu Dataset

1. **File `ids_relationship.csv` là trung tâm:** Luôn bắt đầu từ đây để biết IP nào thuộc Institution/Subnet nào.

2. **Tên file = ID:** 
   - File `institutions/agg_1_day/0.csv` → Institution ID = 0
   - File `ip_addresses_sample/agg_1_day/520007.csv` → IP ID = 520007

3. **Cột `id_time` kết nối với Time dimensions:**
   - Luôn dùng `times/times_*.csv` tương ứng với mức độ aggregation
   - `agg_1_day/` → `times_1_day.csv`
   - `agg_1_hour/` → `times_1_hour.csv`
   - `agg_10_minutes/` → `times_10_minutes.csv`

4. **WeekendHoliday join bằng date:**
   - Extract date từ `time` trong Time dimensions
   - So sánh với `Date` trong `weekends_and_holidays.csv`

5. **Hierarchy (Thứ bậc):**
   - Institution (cao nhất) → chứa nhiều Subnets
   - Subnet → chứa nhiều IPs
   - IP (thấp nhất) → metrics chi tiết nhất

---

## 💡 Ví dụ thực tế: Query dữ liệu

### Tìm tất cả IPs của Institution 0 vào ngày 2023-10-09

**Bước 1:** Tìm `id_time` tương ứng với ngày 2023-10-09
```
times/times_1_day.csv:
id_time = 0 → time = 2023-10-09 00:00:00
```

**Bước 2:** Tìm tất cả IPs thuộc Institution 0
```
ids_relationship.csv:
Lọc tất cả dòng có id_institution = 0
→ id_ip: 1781410, 1637350, 810210, ...
```

**Bước 3:** Đọc metrics của từng IP
```
ip_addresses_sample/agg_1_day/1781410.csv:
id_time = 0 → metrics của IP này vào ngày 2023-10-09
```

---

Tài liệu này giải thích **cách các file CSV kết nối với nhau** thông qua các trường dữ liệu chung. Đây là nền tảng để hiểu và làm việc với dataset.

