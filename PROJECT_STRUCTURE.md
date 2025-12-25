# Project Structure - Network Fault Prediction Dataset

## 📁 Cấu trúc Dataset

```
dataset/
│
├── 📄 ids_relationship.csv                    # File liên kết trung tâm: IP → Institution → Subnet
├── 📄 weekends_and_holidays.csv                # Ngày cuối tuần và ngày lễ
│
├── 📁 institutions/                            # Dữ liệu theo Institution
│   ├── 📄 identifiers.csv                     # Danh sách Institution IDs (285 institutions)
│   ├── 📁 agg_1_day/                          # Metrics aggregate theo ngày
│   │   ├── 0.csv                              # Metrics của Institution 0
│   │   ├── 1.csv                              # Metrics của Institution 1
│   │   ├── 2.csv
│   │   ├── 3.csv
│   │   ├── 4.csv
│   │   ├── 5.csv
│   │   ├── 6.csv
│   │   ├── 7.csv
│   │   ├── 8.csv
│   │   ├── 9.csv
│   │   └── ... (283 files total)
│
├── 📁 institution_subnets/                     # Dữ liệu theo Institution Subnet
│   ├── 📄 identifiers.csv                      # Danh sách Subnet IDs (550 subnets)
│   ├── 📁 agg_1_day/                          # Metrics aggregate theo ngày
│   │   ├── 0.csv                              # Metrics của Subnet 0
│   │   ├── 1.csv                              # Metrics của Subnet 1
│   │   ├── 2.csv
│   │   ├── 3.csv
│   │   ├── 4.csv
│   │   ├── 5.csv
│   │   ├── 6.csv
│   │   ├── 7.csv
│   │   ├── 8.csv
│   │   ├── 9.csv
│   │   └── ... (546 files total)
│
├── 📁 ip_addresses_sample/                     # Dữ liệu theo IP Address (Sample - 1000 IPs)
│   ├── 📄 identifiers.csv                      # Danh sách IP IDs (1000 IPs)
│   ├── 📁 agg_1_day/                          # Metrics aggregate theo ngày
│   │   ├── 520007.csv                         # Metrics của IP 520007
│   │   ├── 1781410.csv                         # Metrics của IP 1781410
│   │   ├── 1637350.csv
│   │   ├── 810210.csv
│   │   ├── 8989.csv
│   │   ├── 482558.csv
│   │   ├── 1796080.csv
│   │   ├── 682759.csv
│   │   ├── 262580.csv
│   │   ├── 1758858.csv
│   │   └── ... (1000 files total)
├── 📁 ip_addresses_full/                      # Dữ liệu theo IP Address (Full dataset - rất nhiều IPs)
│   ├── 📄 identifiers.csv                      # Danh sách IP IDs (toàn bộ IPs)
│   │
│   ├── 📁 agg_1_day/                          # Metrics aggregate theo ngày
│   │   │                                       # Cấu trúc: 11 subfolders (1-11), mỗi subfolder ~25000 files
│   │   ├── 📁 1/                               # Subfolder 1: ~25000 CSV files
│   │   │   ├── 1001.csv
│   │   │   ├── 1002.csv
│   │   │   ├── 1003.csv
│   │   │   ├── 1004.csv
│   │   │   ├── 1005.csv
│   │   │   ├── 1006.csv
│   │   │   ├── 1007.csv
│   │   │   ├── 1008.csv
│   │   │   ├── 1009.csv
│   │   │   ├── 1010.csv
│   │   │   └── ... (~25000 files total)
│   │   ├── 📁 2/                               # Subfolder 2: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 3/                               # Subfolder 3: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 4/                               # Subfolder 4: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 5/                               # Subfolder 5: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 6/                               # Subfolder 6: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 7/                               # Subfolder 7: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 8/                               # Subfolder 8: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 9/                               # Subfolder 9: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   ├── 📁 10/                              # Subfolder 10: ~25000 CSV files
│   │   │   └── ... (~25000 files)
│   │   └── 📁 11/                              # Subfolder 11: ~25000 CSV files
│   │       └── ... (~25000 files)
│   │
├── 📁 times/                                   # Time Dimensions
│   ├── 📄 times_1_day.csv                      # Time dimension theo ngày (282 time points)
│
└── 📁 Documentation Files (Generated)
    ├── 📄 DATASET_RELATIONSHIPS.md             # Tài liệu mối quan hệ giữa các file CSV
    ├── 📄 ERD_DOCUMENTATION.md                 # Tài liệu ERD diagram
    ├── 📄 erd_diagram.dbml                     # ERD diagram code (dbdiagram.io format)
    ├── 📄 erd_postgresql.sql                   # PostgreSQL schema
    └── 📄 PROJECT_STRUCTURE.md                 # File này
```

---

## 📊 Thống kê Dataset

### Số lượng Files:

| Loại | Số lượng |
|------|----------|
| **Institutions** | 285 |
| **Institution Subnets** | 550 |
| **IP Addresses (Sample)** | 1,000 |
| **IP Addresses (Full)** | Rất nhiều (phân bố trong subfolders) |
| **Time Points (1 Day)** | 282 |
| **Time Points (1 Hour)** | 6,710 |
| **Time Points (10 Minutes)** | 40,290 |

### Tổng số Files CSV:

- **Institution metrics**: 285 × 3 (time granularities) = **855 files**
- **Subnet metrics**: 550 × 3 = **1,650 files**
- **IP Sample metrics**: 1,000 × 3 = **3,000 files**
- **IP Full metrics**: 
  - 3 time granularities × 11 subfolders × ~25,000 files/subfolder = **~825,000 files**
  - (Mỗi subfolder có khoảng 25,000 CSV files)
- **Time dimensions**: 3 files
- **Identifiers**: 4 files
- **Relationship**: 1 file
- **WeekendHoliday**: 1 file

**Tổng cộng**: Hơn **800,000 files CSV** trong dataset

---

## 🔑 Quy tắc đặt tên File

### 1. Identifiers Files
- `institutions/identifiers.csv` → Danh sách Institution IDs
- `institution_subnets/identifiers.csv` → Danh sách Subnet IDs
- `ip_addresses_sample/identifiers.csv` → Danh sách IP IDs (sample)
- `ip_addresses_full/identifiers.csv` → Danh sách IP IDs (full)

### 2. Metrics Files

#### Institution & Subnet:
- **Tên file = ID**: `0.csv` = Institution/Subnet có ID = 0
- **Vị trí**: `institutions/agg_1_day/0.csv` = Metrics của Institution 0, aggregate theo ngày

#### IP Address:
- **Tên file = ID**: `520007.csv` = IP có ID = 520007
- **Vị trí**: 
  - **Sample**: `ip_addresses_sample/agg_1_day/520007.csv` (file trực tiếp trong folder)
  - **Full**: `ip_addresses_full/agg_1_day/{subfolder_number}/520007.csv`
    - Subfolder được phân chia theo số đầu tiên của IP ID (1-11)
    - Ví dụ: IP `520007` → subfolder `5/` → `ip_addresses_full/agg_1_day/5/520007.csv`

### 3. Time Dimension Files
- `times/times_1_day.csv` → Dùng cho tất cả file trong `*_1_day/` folders
- `times/times_1_hour.csv` → Dùng cho tất cả file trong `*_1_hour/` folders
- `times/times_10_minutes.csv` → Dùng cho tất cả file trong `*_10_minutes/` folders

---

## 🎯 Cách sử dụng Structure

1. **Tìm metrics của một Institution:**
   - Xem `institutions/identifiers.csv` để biết ID có tồn tại không
   - Đọc file `institutions/agg_1_day/{id}.csv`

2. **Tìm metrics của một IP:**
   - Xem `ids_relationship.csv` để biết IP thuộc Institution/Subnet nào
   - Đọc file `ip_addresses_sample/agg_1_day/{id_ip}.csv` hoặc trong `ip_addresses_full/`

3. **Xác định thời gian:**
   - Dùng `times/times_*.csv` tương ứng với mức độ aggregation
   - Match `id_time` từ metrics file với `id_time` trong time dimension file

4. **Kiểm tra ngày cuối tuần/ngày lễ:**
   - Extract date từ `times/times_*.csv`
   - Match với `weekends_and_holidays.csv`

---

## 📝 Lưu ý

- **ip_addresses_sample**: Dataset mẫu với 1000 IPs, dùng cho testing/development
- **ip_addresses_full**: Dataset đầy đủ với rất nhiều IPs, được tổ chức trong subfolders theo chữ số đầu tiên của ID
- Tất cả metrics files có cùng cấu trúc: `id_time` + các metrics columns
- Mỗi entity (Institution/Subnet/IP) có 3 files metrics tương ứng với 3 mức độ thời gian

