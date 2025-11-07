# KIẾN TRÚC DỰ ÁN - Auralis AI Photo Studio
---
## 📋 MỤC LỤC
1. [Kiến trúc hiện tại](#1-kiến-trúc-hiện-tại)
2. [Kiến trúc MVVM mục tiêu](#2-kiến-trúc-mvvm-mục-tiêu)

---
## 1. KIẾN TRÚC HIỆN TẠI
### 1.1. Cấu trúc thư mục
```
lib/
├── main.dart                    # Entry point + Init (Ads, Firebase)
├── data/                        # App config
├── model/                       # Shared models
├── routes/                      # App routing
└── ui03_from_aerobrush_v066/    # ⚠️ Tên không chuẩn
    ├── common_screen/           # Screens
    │   ├── home/
    │   │   ├── home_screen.dart         # View
    │   │   ├── home_view_model.dart     # ViewModel (609 dòng!)
    │   │   └── widgets/
    │   ├── history/
    │   ├── generate/
    │   ├── category/
    │   └── ...
    ├── config/                  # Theme, colors, strings
    ├── data/                    # JSON loaders
    ├── model/                   # Models
    ├── state_managers/          # Global state (TaskStatusManager)
    └── utils/                   # Utilities

modules/image_ai/lib/
├── src/
│   ├── cf_image_processor.dart  # ⚠️ Singleton API handler
│   └── ...
├── history/
│   └── history_shared_pref_utils.dart  # ⚠️ Trực tiếp SharedPrefs
└── noco/
    └── noco_utils.dart          # ⚠️ API client không abstraction
```

### 1.2. Luồng dữ liệu hiện tại

```
View → ViewModel → [THIẾU SERVICE] → API/Storage trực tiếp
       (609 dòng)                     ↓
                               NocoUtils/CfImageProcessor
                               HistorySharedPrefUtils
```

### 1.3. Điểm mạnh ✅

1. **Có cấu trúc MVVM cơ bản**
   - Tách View và ViewModel rõ ràng
   - ViewModels dùng `ChangeNotifier` đúng chuẩn
   - Mỗi màn hình có ViewModel riêng

2. **State Management hợp lý**
   - Provider cho global state (`TaskStatusManager`, `ConnectivityProvider`)
   - Local state trong ViewModel (không lạm dụng Provider)

3. **Performance tốt**
   - Singleton pattern cho ViewModels
   - Image precaching
   - Category caching

### 1.4. Vấn đề cần khắc phục ⚠️

#### **Vấn đề 1: THIẾU TẦNG SERVICE/REPOSITORY**

```dart
// ❌ HomeViewModel gọi trực tiếp API (609 dòng code)
class HomeViewModel extends ChangeNotifier {
  Future<void> fetchHomeData() async {
    await NocoUtils().getDataRowByKey(..., _nocoCallback);  // Gọi API trực tiếp
  }
  
  Future<File?> openCamera() { ... }  // Logic camera trong ViewModel
  
  void onSectionItemTapped(...) {
    // 100+ dòng code xử lý logic tìm kiếm item
  }
}
```

**Hậu quả:**
- ViewModel quá phình, khó maintain
- Logic nghiệp vụ không tái sử dụng được
- Khó test vì phụ thuộc trực tiếp vào API
- Vi phạm Single Responsibility Principle

#### **Vấn đề 2: DATA PERSISTENCE KHÔNG CHUẨN**

```dart
// ❌ Trực tiếp dùng SharedPreferences
class HistorySharedPrefUtils {
  static Future<void> saveHistoryList(List<HistoryDataModel> list) async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setString('history', jsonEncode(list));
  }
}
```

**Hậu quả:**
- Không có abstraction cho storage
- Khó chuyển đổi sang Hive/SQLite sau này
- Không thể mock cho testing

#### **Vấn đề 3: CALLBACK HELL**

```dart
// ❌ Dùng callback thay vì async/await
late final NocoCallback _nocoCallback = NocoCallback(
  onSuccess: (data) { ... },
  onError: (error) { ... },
);
```

**Hậu quả:**
- Code khó đọc
- Error handling phức tạp
- Không tận dụng async/await hiện đại

#### **Vấn đề 4: VIEWMODEL PHỤ THUỘC BUILDCONTEXT**

```dart
// ❌ Navigation trong ViewModel
void onSectionItemTapped(..., BuildContext context) {
  Navigator.pushNamed(context, '/category');
}
```

**Hậu quả:**
- Vi phạm separation of concerns
- Khó test ViewModel

#### **Vấn đề 5: NAMING KHÔNG NHẤT QUÁN**

- Thư mục: `ui03_from_aerobrush_v066` ❌
- Files: `history_viewmodel.dart` vs `home_view_model.dart` ❌
- Variables: `cf`, `noco` (viết tắt không rõ) ❌

---

## 2. KIẾN TRÚC MVVM MỤC TIÊU

### 2.1. Nguyên tắc thiết kế

```
┌────────────────────────────────────────────────────────┐
│  VIEW (UI Only)                                        │
│  - Hiển thị UI, không có logic                         │
│  - Listen ViewModel qua Consumer/Provider              │
└───────────────────┬────────────────────────────────────┘
                    │ Events
                    ▼
┌────────────────────────────────────────────────────────┐
│  VIEWMODEL (UI Logic & State)                          │
│  - Quản lý UI state                                    │
│  - Validate input                                      │
│  - Gọi Service                                         │
│  - notifyListeners() khi state thay đổi               │
└───────────────────┬────────────────────────────────────┘
                    │ Calls
                    ▼
┌────────────────────────────────────────────────────────┐
│  SERVICE (Business Logic)                              │
│  - Xử lý use cases                                     │
│  - Điều phối nhiều Repository                          │
│  - Transform data                                      │
└───────────────────┬────────────────────────────────────┘
                    │ Calls
                    ▼
┌────────────────────────────────────────────────────────┐
│  REPOSITORY (Data Management)                          │
│  - Trừu tượng hóa data sources                         │
│  - Quyết định local hay remote                         │
│  - Caching logic                                       │
└───────────────────┬────────────────────────────────────┘
                    │ Calls
                    ▼
┌────────────────────────────────────────────────────────┐
│  DATA SOURCE (Local/Remote)                            │
│  - API calls (NocoAPI, AI API)                         │
│  - Local storage (SharedPrefs, Hive)                   │
└────────────────────────────────────────────────────────┘
```

### 2.2. Cấu trúc thư mục mục tiêu

```
lib/
├── main.dart                              // Entry point, khởi tạo app
│
├── app/
│   ├── routes/
│   │   └── app_routes.dart               // Named routes mapping
│   └── config/                           // ⭐ App config, theme, colors, strings
│       ├── app_theme.dart                // Theme configuration
│       ├── app_colors.dart               // Color constants
│       ├── app_strings.dart              // Text strings
│       └── app_config.dart               // API keys, endpoints
│
├── core/                                 // ⭐ Core utilities (dùng chung toàn app)
│   ├── constants/
│   │   └── app_constants.dart            // Constants (max file size, timeouts...)
│   ├── errors/
│   │   └── exceptions.dart               // Custom exceptions (AppException, ServiceException...)
│   ├── extensions/
│   │   └── string_extensions.dart        // Extension methods
│   └── utils/
│       └── date_formatter.dart           // Utility functions
│
├── data/                                 // ⭐ Data Layer
│   ├── models/                           // Data models (POJO - Plain Old Java Object)
│   │   ├── category_model.dart           // CategoryModel: id, name, items[]
│   │   ├── history_model.dart            // HistoryModel: taskId, imageURL, status...
│   │   └── ai_task_model.dart            // AITaskModel: taskId, workflow, result...
│   │
│   ├── repositories/                     // ⭐ Repositories (quản lý data: cache + API + parse)
│   │   ├── category_repository.dart      // Quản lý category data (cache → API → parse)
│   │   ├── history_repository.dart       // Quản lý history data (save/load từ storage)
│   │   └── ai_processing_repository.dart // Quản lý AI tasks (submit → check result)
│   │
│   └── datasources/                      // ⭐ Data sources (lấy raw data)
│       ├── local/                        // Local storage
│       │   ├── local_storage_service.dart // Interface: saveString(), getString()...
│       │   ├── shared_prefs_storage.dart  // Implementation với SharedPreferences
│       │   └── cache_manager.dart         // Quản lý cache (check expiry, clear old cache)
│       │
│       └── remote/                       // Remote API
│           ├── api_client.dart            // Base HTTP client (Dio config)
│           ├── noco_api.dart              // NocoDB API calls (fetchCategory, fetchHome...)
│           └── ai_api.dart                // AI API calls (processImage, checkResult...)
│
├── services/                             // ⭐ Business Logic (use cases)
│   ├── category_service.dart             // Logic: load categories, search, sort...
│   ├── ai_processing_service.dart        // Logic: submit task, check status, retry...
│   ├── image_service.dart                // Logic: pick image, compress, upload...
│   └── permission_service.dart           // Logic: check permissions, request...
│
├── view_models/                          // ⭐ UI Logic & State (MVVM ViewModels)
│   ├── home_viewmodel.dart               // Home UI state: categories[], isLoading...
│   ├── history_viewmodel.dart            // History UI state: histories[], isEmpty...
│   └── category_viewmodel.dart           // Category UI state: items[], selectedIndex...
│
├── views/                                // ⭐ UI Only (Flutter Widgets)
│   ├── home/
│   │   ├── home_screen.dart              // Home screen UI
│   │   └── widgets/                      // Home-specific widgets
│   │       ├── category_card.dart
│   │       └── section_header.dart
│   ├── history/
│   │   ├── history_screen.dart           // History screen UI
│   │   └── widgets/
│   │       └── history_item_card.dart
│   ├── category/
│   │   ├── category_screen.dart          // Category screen UI
│   │   └── widgets/
│   ├── common widgets/                           // widgets (dùng chung nhiều màn hình)
│   │   └──
│   │       ├── loading_indicator.dart
│   │       ├── error_widget.dart
│   │       └── custom_button.dart
│   └── ...
│
└── shared/
    └── state_managers/                   // Global state managers (Singleton)
        ├── task_status_manager.dart      // Quản lý AI tasks toàn app
        └── connectivity_provider.dart    // Quản lý internet connection
```

---

## 3. LUỒNG DỮ LIỆU CHI TIẾT

### 3.1. Ví dụ 1: User xem danh sách Categories

#### Luồng hoạt động:

```
👤 USER
  │ Tap vào nút "Xem Categories"
  ▼
📱 VIEW (home_screen.dart)
  │ onPressed: () => viewModel.loadCategories()
  ▼
🎨 VIEWMODEL (home_viewmodel.dart)
  │ loadCategories() {
  │   _isLoading = true;
  │   notifyListeners();  // → UI hiển thị loading
  │   
  │   _categories = await _categoryService.getAllCategories();
  │   
  │   _isLoading = false;
  │   notifyListeners();  // → UI hiển thị data
  │ }
  ▼
⚙️ SERVICE (category_service.dart)
  │ getAllCategories() {
  │   // Validate, business logic
  │   final categories = await _repository.getAllCategories();
  │   
  │   // Sort by name
  │   categories.sort((a, b) => a.name.compareTo(b.name));
  │   
  │   return categories;
  │ }
  ▼
💾 REPOSITORY (category_repository.dart)
  │ getAllCategories() {
  │   // 1️⃣ Check cache trước
  │   final cached = await _cacheManager.get('all_categories');
  │   if (cached != null) {
  │     return cached;  // ✅ Có cache → return luôn
  │   }
  │   
  │   // 2️⃣ Không có cache → gọi API
  │   final rawData = await _nocoApi.fetchCategories();
  │   
  │   // 3️⃣ Parse JSON → Model
  │   final categories = rawData.map((json) => 
  │     CategoryModel.fromJson(json)
  │   ).toList();
  │   
  │   // 4️⃣ Save vào cache
  │   await _cacheManager.saveWithExpiry(
  │     'all_categories', 
  │     categories,
  │     Duration(hours: 1)
  │   );
  │   
  │   return categories;
  │ }
  ▼
📡 DATA SOURCE (noco_api.dart)
  │ fetchCategories() {
  │   // GỌI API THẬT với Dio
  │   final response = await _apiClient.get(
  │     'https://api.nocodb.com/api/v2/tables/xxx/records',
  │     params: {'where': '(status,eq,active)'}
  │   );
  │   
  │   return response.data['list'];  // Trả RAW JSON
  │ }
  ▼
🌐 INTERNET
  │ HTTP GET Request
  │ ↓
  │ NocoDB Server
  │ ↓
  │ Response: JSON data
  ▼
📡 DATA SOURCE
  │ Nhận JSON response
  │ Return về Repository
  ▼
💾 REPOSITORY
  │ Parse JSON → CategoryModel
  │ Save cache
  │ Return về Service
  ▼
⚙️ SERVICE
  │ Sort data
  │ Return về ViewModel
  ▼
🎨 VIEWMODEL
  │ _categories = result
  │ notifyListeners() → Update UI
  ▼
📱 VIEW
  │ Consumer<HomeViewModel> nhận notify
  │ Rebuild widget
  │ Hiển thị danh sách categories lên màn hình
  ▼
👤 USER thấy kết quả! ✅
```

---

### 3.2. Ví dụ 2: User lưu History

#### Luồng hoạt động:

```
👤 USER
  │ Hoàn thành AI processing
  ▼
📱 VIEW (result_screen.dart)
  │ viewModel.saveToHistory(result)
  ▼
🎨 VIEWMODEL (result_viewmodel.dart)
  │ saveToHistory(result) {
  │   await _historyService.addHistory(
  │     HistoryModel(
  │       taskId: result.taskId,
  │       imageURL: result.imageUrl,
  │       createdAt: DateTime.now()
  │     )
  │   );
  │   
  │   _showSuccessMessage();
  │ }
  ▼
⚙️ SERVICE (history_service.dart)
  │ addHistory(history) {
  │   // Validate
  │   if (history.taskId.isEmpty) throw Exception();
  │   
  │   // Check limit (max 100 items)
  │   final count = await _repository.getHistoryCount();
  │   if (count >= 100) {
  │     await _repository.deleteOldest();
  │   }
  │   
  │   // Add new
  │   await _repository.addHistory(history);
  │ }
  ▼
💾 REPOSITORY (history_repository.dart)
  │ addHistory(history) {
  │   // 1️⃣ Load existing list
  │   final list = await _localStorage.getString('history_list');
  │   final histories = jsonDecode(list ?? '[]');
  │   
  │   // 2️⃣ Add new item to top
  │   histories.insert(0, history.toJson());
  │   
  │   // 3️⃣ Save back to storage
  │   await _localStorage.saveString(
  │     'history_list',
  │     jsonEncode(histories)
  │   );
  │ }
  ▼
💿 DATA SOURCE (shared_prefs_storage.dart)
  │ saveString(key, value) {
  │   // LƯU VÀO BỘ NHỚ ĐIỆN THOẠI
  │   await _prefs.setString(key, value);
  │ }
  ▼
📲 DEVICE STORAGE
  │ Lưu file vào bộ nhớ
  │ /data/data/com.app/shared_prefs/history_list
  ▼
✅ SAVED! User có thể xem lại sau
```

---

### 3.3. Ví dụ 3: User xử lý ảnh với AI

#### Luồng hoạt động:

```
👤 USER
  │ Chọn ảnh và style → Tap "Generate"
  ▼
📱 VIEW (generate_screen.dart)
  │ viewModel.generateImage(imagePath, style)
  ▼
🎨 VIEWMODEL (generate_viewmodel.dart)
  │ generateImage(imagePath, style) {
  │   _isProcessing = true;
  │   notifyListeners();
  │   
  │   try {
  │     final result = await _aiService.processImage(
  │       imagePath: imagePath,
  │       workflow: style.workflow
  │     );
  │     
  │     _result = result;
  │     _navigateToResult();
  │   } catch (e) {
  │     _errorMessage = e.toString();
  │   } finally {
  │     _isProcessing = false;
  │     notifyListeners();
  │   }
  │ }
  ▼
⚙️ SERVICE (ai_processing_service.dart)
  │ processImage(imagePath, workflow) {
  │   // 1️⃣ Compress image
  │   final compressed = await _imageService.compressImage(imagePath);
  │   
  │   // 2️⃣ Convert to base64
  │   final base64 = base64Encode(compressed);
  │   
  │   // 3️⃣ Submit to AI API
  │   final taskId = await _repository.submitTask(base64, workflow);
  │   
  │   // 4️⃣ Poll result every 5 seconds
  │   while (true) {
  │     await Future.delayed(Duration(seconds: 5));
  │     
  │     final status = await _repository.checkTaskStatus(taskId);
  │     
  │     if (status.isCompleted) {
  │       return status.result;
  │     }
  │     
  │     if (status.isFailed) {
  │       throw Exception(status.error);
  │     }
  │   }
  │ }
  ▼
💾 REPOSITORY (ai_processing_repository.dart)
  │ submitTask(base64, workflow) {
  │   // Call API to submit
  │   final response = await _aiApi.processImage(
  │     imageBase64: base64,
  │     workflow: workflow
  │   );
  │   
  │   return response['task_id'];
  │ }
  │
  │ checkTaskStatus(taskId) {
  │   final response = await _aiApi.checkResult(taskId);
  │   return AITaskStatus.fromJson(response);
  │ }
  ▼
📡 DATA SOURCE (ai_api.dart)
  │ processImage(imageBase64, workflow) {
  │   // POST request to AI API
  │   final response = await _apiClient.post(
  │     'https://ai-api.com/process',
  │     data: {
  │       'image': imageBase64,
  │       'workflow': workflow
  │     }
  │   );
  │   
  │   return response.data;
  │ }
  │
  │ checkResult(taskId) {
  │   // GET request to check status
  │   final response = await _apiClient.get(
  │     'https://ai-api.com/result/$taskId'
  │   );
  │   
  │   return response.data;
  │ }
  ▼
🤖 AI SERVER
  │ Processing image...
  │ (takes 30-60 seconds)
  │ ↓
  │ Task completed!
  │ Return result image URL
  ▼
📡 DATA SOURCE → 💾 REPOSITORY → ⚙️ SERVICE → 🎨 VIEWMODEL → 📱 VIEW
  │
  ▼
👤 USER thấy ảnh đã xử lý! 🎉
```

**Tài liệu tham khảo:**
- [Flutter MVVM Best Practices](https://docs.flutter.dev)
- [Clean Architecture](https://blog.cleancoder.com)
- [Repository Pattern](https://martinfowler.com)
