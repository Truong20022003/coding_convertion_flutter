# 🎨 Hướng dẫn Quy ước Đặt tên trong Flutter/Dart

# 📖 MỤC LỤC

1. [🎨 Hướng dẫn Quy ước Đặt tên trong Flutter/Dart](#-hướng-dẫn-quy-ước-đặt-tên-trong-flutterdart)  
   1.1. [✅ Class / Enum / Typedef / Type Parameter (UpperCamelCase)]   (#-do-đặt-tên-uppercamelcase-cho-class--enum--typedef--tham-số-kiểu)  
   1.2. [📁 Quy ước tên Package & Tên File](#-quy-ước-tên-package--tên-file)  
   1.3. [🧩 Alias import (lowercase_with_underscores)](#-do-tiền-tố-alias-import-dùng-lowercase_with_underscores)  
   1.4. [🧱 Members / Variables / Parameters (lowerCamelCase)](#-tên-members--top-level-definitions--variables--parameters--named-args-dùng-lowercamelcase)  
   1.5. [🧷 Hằng số (lowerCamelCase)](#-ưu-tiên-hằng-số-đặt-tên-lowercamelcase)  
   1.6. [🔤 Viết hoa từ viết tắt (như Nasa, Uri...)](#-nên-viết-hoa-từ-viết-tắt-dài-hơn-hai-chữ-cái-như-từ-bình-thường)  
   1.7. [🧰 Callback không dùng tới](#-ưu-tiên-dùng-tham-số-ký-tự-đại-diện-cho-callback-không-dùng-tới)  
   1.8. [🚫 Không dùng tiền tố chữ cái](#-không-dùng-tiền-tố-chữ-cái-hungarian-notation)  
   1.9. [📚 Không đặt tên thư viện tường minh](#-không-đặt-tên-thư-viện-một-cách-tường-minh)  
   1.10. [📦 Thứ tự Imports/Exports](#-thứ-tự-importsexports)  
   1.11. [💡 Tip: Bật rule trong analysis_options.yaml](#-tip-bạn-có-thể-bật-các-rule-tương-ứng-trong-analysis_optionsyaml)  
  
2. [🧾 Effective Dart: Formatting, Comments, Null, Strings & Design]  (#-effective-dart-formatting-comments-null-strings--design)  
   2.1. [🧾 Định dạng (Formatting)](#-định-dạng-formatting)  
   2.2. [💬 Bình luận & Ghi chú trong code](#-bình-luận--ghi-chú-trong-code-comments)  
   2.3. [🌫️ Null](#-null)  
   2.4. [✅ Code Quality Rules](#-code-quality-rules)  
   2.5. [🧵 Chuỗi (Strings)](#-chuỗi-ký-tự-strings)  
   2.6. [💬 Chuỗi & Tập hợp (Strings & Collections)](#-chuỗi--tập-hợp-strings--collections)  
   2.7. [🏷️ Effective Dart: Design](#-effective-dart-design)  
   2.8. [✅ Tóm tắt quy tắc ngắn gọn](#-tóm-tắt-ngắn-gọn)  
  
3. [📚 References](#-references)  

---



---

## ✅ DO: Đặt tên **UpperCamelCase** cho **class / enum / typedef / tham số kiểu**
Các lớp, kiểu enum, typedef và tham số kiểu phải viết hoa chữ cái đầu tiên của mỗi từ (bao gồm cả từ đầu tiên) và **không dùng dấu phân cách**.

**Tốt**
```dart
class SliderMenu {
  // ...
}

class HttpRequest {
  // ...
}

typedef Predicate<T> = bool Function(T value);

class Foo {
  const Foo([Object? arg]);
}

@Foo(anArg)
class A {
  // ...
}

@Foo()
class B {
  // ...
}

const foo = Foo();

@foo
class C {
  // ...
}
```

**Phần mở rộng (extension)** cũng viết **UpperCamelCase** và không dùng dấu phân cách.

**Tốt**
```dart
extension MyFancyList<T> on List<T> {
  // ...
}

extension SmartIterable<T> on Iterable<T> {
  // ...
}
```

---

## 📁 Quy ước **tên package & tên file**

Một số hệ thống tệp **không phân biệt hoa/thường**, vì vậy nhiều dự án yêu cầu **tên thư mục/tệp viết thường hoàn toàn**. Dùng **dấu gạch dưới** làm dấu phân cách để tên vẫn dễ đọc _và_ vẫn là định danh Dart hợp lệ nếu sau này được import bằng ký hiệu.

**Tốt**
```text
my_package
└─ lib
   ├─ file_system.dart
   └─ slider_menu.dart
```

**Xấu**
```text
mypackage
└─ lib
   ├─ file-system.dart
   └─ SliderMenu.dart
```

---

## 🧩 DO: Tiền tố alias import dùng **lowercase_with_underscores**

**Tốt**
```dart
import 'dart:math' as math;
import 'package:angular_components/angular_components.dart' as angular_components;
import 'package:js/js.dart' as js;
```

**Xấu**
```dart
import 'dart:math' as Math;
import 'package:angular_components/angular_components.dart' as angularComponents;
import 'package:js/js.dart' as JS;
```

---

## 🧱 Tên **members / top-level definitions / variables / parameters / named args** dùng **lowerCamelCase**
```dart
var count = 3;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
```

---

## 🧷 ƯU TIÊN: Hằng số đặt tên **lowerCamelCase**
**Tốt**
```dart
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = RegExp(r'^([a-z]+):');

class Dice {
  static final numberGenerator = Random();
}
```

**Xấu**
```dart
const PI = 3.14;
const DefaultTimeout = 1000;
final URL_SCHEME = RegExp(r'^([a-z]+):');

class Dice {
  static final NUMBER_GENERATOR = Random();
}
```
### 1. DRY (Don't Repeat Yourself)
```dart
// ❌ Bad
Text('Item 1', style: TextStyle(fontSize: 16, color: Colors.white))
Text('Item 2', style: TextStyle(fontSize: 16, color: Colors.white))

// ✅ Good
final textStyle = TextStyle(fontSize: 16, color: Colors.white);
Text('Item 1', style: textStyle)
Text('Item 2', style: textStyle)
```

---

### 2. Single Responsibility
```dart
// ❌ Bad - ViewModel xử lý cả API call
class HomeViewModel {
  Future<void> loadData() async {
    final response = await http.get(url);
    // Parse response...
  }
}

// ✅ Good - Tách API logic ra Service
class ApiService {
  Future<List<Item>> fetchItems() async {
    final response = await http.get(url);
    return parseItems(response);
  }
}

class HomeViewModel {
  final ApiService _apiService = ApiService();
  
  Future<void> loadData() async {
    _items = await _apiService.fetchItems();
  }
}
```


### 3. Widget Extraction
```dart
// ❌ Bad - Nested widgets
Widget build(BuildContext context) {
  return Column(
    children: [
      Container(
        child: Row(
          children: [
            Icon(...),
            Text(...),
          ],
        ),
      ),
    ],
  );
}

// ✅ Good - Extract to separate widget
Widget build(BuildContext context) {
  return Column(
    children: [
      _buildHeader(),
    ],
  );
}

Widget _buildHeader() {
  return Container(
    child: Row(
      children: [
        Icon(...),
        Text(...),
      ],
    ),
  );
}
## 🔤 NÊN viết hoa **từ viết tắt** (dài hơn hai chữ cái) như **từ bình thường**
**Tốt**
```text
Http  // "hypertext transfer protocol"
Nasa  // "national aeronautics and space administration"
Uri   // "uniform resource identifier"
Esq   // "esquire"
Ave   // "avenue"
```

**Xấu**
```text
HTTP // "hypertext transfer protocol"
NASA // "national aeronautics and space administration"
URI  // "uniform resource identifier"
esq  // "esquire"
ave  // "avenue"
```

---

## 🧰 ƯU TIÊN: Dùng **tham số ký tự đại diện** cho callback **không dùng tới**
```dart
futureOfVoid.then((_) {
  print('Operation complete.');
}).onError((_, __) {
  print('Operation failed.');
});
```

## 🚫 KHÔNG dùng **tiền tố chữ cái** (Hungarian notation)
**Tốt**
```dart
defaultTimeout;
```

**Xấu**
```dart
kDefaultTimeout;
```

---

## 📚 KHÔNG đặt tên thư viện một cách tường minh
**Xấu**
```dart
library my_library;
```
**Tốt**
```dart
/// A really great test library.
@TestOn('browser')
library;
```

---

## 📦 Thứ tự **imports/exports**
### DO: Đặt `dart:` **trước** các import khác
**Tốt**
```dart
import 'dart:async';
import 'dart:collection';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

### DO: Đặt `package:` **trước** import tương đối
**Tốt**
```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'util.dart';
```

### DO: **Exports** nằm **sau tất cả imports** và ở **một khối riêng**
**Tốt**
```dart
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```
**Xấu**
```dart
import 'src/error.dart';
export 'src/error.dart';
import 'src/foo_bar.dart';
```

### DO: Sắp xếp **theo alphabet** từng khối
**Tốt**
```dart
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
import 'foo.dart';
import 'foo/foo.dart';
```

**Xấu**
```dart
import 'package:foo/foo.dart';
import 'package:bar/bar.dart';
import 'foo/foo.dart';
import 'foo.dart';
```

---

> 💡 **Tip**: Bạn có thể bật các rule tương ứng trong `analysis_options.yaml` (ví dụ `prefer_const_constructors`, `prefer_final_locals`, `avoid_print`, v.v.) để tự động hoá kiểm tra.
# 🧾 Effective Dart: Formatting, Comments, Null, Strings & Design

Tổng hợp hướng dẫn chuẩn hóa code trong Dart — bao gồm **định dạng, comment, null-safety, chuỗi, tập hợp và thiết kế API**.

---

## 🧾 **Định dạng (Formatting)**

Giống như nhiều ngôn ngữ khác, **Dart bỏ qua khoảng trắng (whitespace)** — nhưng **con người thì không 😄**.  
Việc tuân thủ phong cách định dạng nhất quán giúp người đọc **nhìn code giống như trình biên dịch hiểu**.

---

### ✅ **DO: Định dạng code bằng `dart format`**

Việc căn chỉnh, thụt đầu dòng, xuống dòng thủ công rất tốn thời gian — đặc biệt khi refactor.  
May mắn thay, Dart cung cấp công cụ định dạng tự động cực mạnh:

```bash
dart format .
```

---

### 📏 **PREFER: Giữ độ dài dòng ≤ 80 ký tự**

Các nghiên cứu cho thấy **dòng dài khiến mắt khó đọc và dễ mỏi** — vì phải lia qua lại nhiều.  
Giống như **bá chí chia cột nhỏ để dễ đọc**, code ngắn gọn giúp bạn **dễ so sánh và theo dõi hơn**.

#### 💡 Nếu code của bạn thường vượt 80 ký tự, hãy xem lại:
- Tên class hoặc biến có quá dài không?  
- Mỗi từ trong tên có thực sự cần thiết không?

> Ví dụ:  
> `VeryLongCamelCaseClassName` ➜ có thể rút gọn.

#### ⚙️ Mặc định của Dart Formatter
- `dart format` **giới hạn 80 ký tự/dòng** (có thể tùy chỉnh).  
- Formatter **không tự chia dòng cho chuỗi string** — bạn phải làm thủ công.  
- **URI hoặc file path** có thể vượt quá 80 ký tự để dễ tìm kiếm.  
- **Multi-line string** có thể dài hơn 80 ký tự vì ngắt dòng làm thay đổi nội dung thật.

---

### 🧩 **DO: Dùng dấu ngoặc nhọn `{}` cho mọi câu lệnh điều khiển**

> **Lint rule:** `curly_braces_in_flow_control_structures`

Điều này giúp tránh lỗi **“dangling else”** — khi `else` bị hiểu sai khối.

**✅ Tốt**
```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}
```

---

## 💬 **Bình luận & Ghi chú trong Code (Comments)**

Đừng nghĩ code của bạn “rõ ràng rồi” — người khác (và cả bạn trong tương lai) **không có ngữ cảnh** như hiện tại.  
Một comment ngắn, rõ ràng có thể **tiết kiệm hàng giờ đọc hiểu**.  
Code nên tự giải thích, nhưng **comment đúng chỗ** giúp hiểu nhanh hơn.

---

### ✅ Viết comment như một câu hoàn chỉnh
- Bắt đầu bằng chữ hoa, kết thúc bằng dấu `.`  
- Áp dụng cho mọi comment (kể cả `TODO`).

**✅ Tốt**
```dart
// Không chạy nếu đã có dữ liệu trước đó.
if (_chunks.isNotEmpty) return false;
```

---

## 🚫 **Không dùng block comment để mô tả**

Chỉ dùng `/* ... */` để **tạm tắt code**, **không dùng để mô tả logic**.

**✅ Tốt**
```dart
// Giả sử tên hợp lệ.
print('Hi, $name!');
```

**❌ Xấu**
```dart
/* Giả sử tên hợp lệ. */
print('Hi, $name!');
```

---

## 🌫️ **Null**

### 🚫 Không khởi tạo biến bằng `null` một cách tường minh  
> **Linter:** `avoid_init_to_null`

Nếu biến **non-nullable**, Dart sẽ báo lỗi khi dùng trước khi khởi tạo.  
Nếu biến **nullable**, Dart **tự động gán null**, không cần `= null`.

**✅ Tốt**
```dart
Item? bestDeal(List<Item> cart) {
  Item? bestItem;

  for (final item in cart) {
    if (bestItem == null || item.price < bestItem.price) {
      bestItem = item;
    }
  }

  return bestItem;
}
```

---

## 💡 **Cân nhắc: Type Promotion & Null-check Pattern khi dùng kiểu Nullable**

Khi bạn kiểm tra biến nullable khác `null`, Dart **tự động chuyển kiểu (promote)** thành **non-nullable**.  
Điều này cho phép truy cập property hoặc truyền vào hàm non-null dễ dàng hơn.

> Type promotion chỉ hoạt động với:
> - Biến cục bộ (`local variable`)
> - Tham số (`parameter`)
> - Thuộc tính `private final`

Nếu giá trị có thể thay đổi nơi khác, Dart không thể promote vì mất an toàn.  
Để khắc phục, hãy:
- Dùng **pattern matching** với null-check  
- Hoặc **gán vào biến cục bộ** trước khi kiểm tra

### ✅ Cách 1: Null-check pattern
```dart
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    if (this.response case var response?) {
      return 'Không thể upload tới ${response.url} '
          '(mã lỗi ${response.errorCode}): ${response.reason}.';
    }
    return 'Không thể upload (không có phản hồi).';
  }
}
```

### ✅ Cách 2: Gán vào biến cục bộ trước khi kiểm tra
```dart
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    final response = this.response;
    if (response != null) {
      return 'Không thể upload tới ${response.url} '
          '(mã lỗi ${response.errorCode}): ${response.reason}.';
    }
    return 'Không thể upload (không có phản hồi).';
  }
}
```

**❌ Xấu**
```dart
class UploadException {
  final Response? response;

  UploadException([this.response]);

  @override
  String toString() {
    if (response != null) {
      return 'Không thể upload tới ${response!.url} '
          '(mã lỗi ${response!.errorCode}): ${response!.reason}.';
    }

    return 'Không thể upload (không có phản hồi).';
  }
}
```

---


## ✅ CODE QUALITY RULES

### 1. DRY (Don't Repeat Yourself)
```dart
// ❌ Bad
Text('Item 1', style: TextStyle(fontSize: 16, color: Colors.white))
Text('Item 2', style: TextStyle(fontSize: 16, color: Colors.white))

// ✅ Good
final textStyle = TextStyle(fontSize: 16, color: Colors.white);
Text('Item 1', style: textStyle)
Text('Item 2', style: textStyle)
```

## 🧵 **Chuỗi ký tự (Strings)**

Khi xử lý chuỗi trong **Dart**, hãy giữ code **ngắn gọn, dễ đọc** và **tuân theo quy tắc chuẩn**.

### ✅ Dùng chuỗi liền kề để nối *string literal*
> **Linter:** `prefer_adjacent_string_concatenation`

Khi nối **chuỗi literal** (chuỗi viết trực tiếp, không phải biến), **không cần dấu `+`**.  
Chỉ cần đặt cạnh nhau — Dart sẽ tự nối.

**✅ Tốt**
```dart
raiseAlarm(
  'ERROR: Phần thân tàu đang cháy. Các phần khác '
  'đang bị người sao Hỏa chiếm đóng. Không rõ phần nào là phần nào.',
);
```

**❌ Xấu**
```dart
raiseAlarm(
  'ERROR: Phần thân tàu đang cháy. Các phần khác ' +
      'đang bị người sao Hỏa chiếm đóng. Không rõ phần nào là phần nào.',
);
```

---

## 💬 **Chuỗi & Tập hợp (Strings & Collections)**

### 💡 PREFER: Dùng nội suy chuỗi (interpolation) để ghép chuỗi và giá trị  
> **Linter:** `prefer_interpolation_to_compose_strings`

**✅ Tốt**
```dart
'Hello, $name! You are ${year - birth} years old.';
```

**❌ Xấu**
```dart
'Hello, ' + name + '! You are ' + (year - birth).toString() + ' years old.';
```

> 💡 Dùng `.toString()` chỉ khi cần chuyển **một giá trị duy nhất** thành chuỗi.

### ⚠️ AVOID: Dùng ngoặc nhọn `{}` khi không cần thiết  
> **Linter:** `unnecessary_brace_in_string_interps`

**✅ Tốt**
```dart
var greeting = 'Hi, $name! I love your ${decade}s costume.';
```

**❌ Xấu**
```dart
var greeting = 'Hi, ${name}! I love your ${decade}s costume.';
```

---

### 🧩 **Collections (Tập hợp)**

Dart hỗ trợ 4 loại tập hợp chính: **List**, **Map**, **Queue**, và **Set**.  
Dưới đây là các quy tắc hay khi làm việc với chúng.

### ✅ DO: Dùng cú pháp literal để tạo collection  
> **Linter:** `prefer_collection_literals`

**✅ Tốt**
```dart
var points = <Point>[];
var addresses = <String, Address>{};
var counts = <int>{};
```

**❌ Xấu**
```dart
var addresses = Map<String, Address>();
var counts = Set<int>();
```

> 💡 Cú pháp literal giúp bạn dùng được **spread (`...`)**, **null-spread (`...?`)**, và **if/for trong collection**.

---

## 🏷️ **Effective Dart: Design**

Hướng dẫn các quy tắc thiết kế API trong Dart — giúp code nhất quán, dễ hiểu và dễ dùng.

---

## 🏷️ Names (Đặt tên)

### ✅ DO: Sử dụng thuật ngữ nhất quán  
Dùng **cùng một tên cho cùng một khái niệm** trong toàn bộ code.  
Nếu đã có chuẩn trong thư viện hoặc trong ngữ cảnh người dùng quen thuộc, **hãy tuân theo**.

**✅ Tốt**
```dart
pageCount         // Trường dữ liệu.
updatePageCount() // Liên quan tới pageCount.
toSomething()     // Theo chuẩn Iterable.toList().
asSomething()     // Theo chuẩn List.asMap().
Point             // Tên phổ biến, dễ hiểu.
```

**❌ Xấu**
```dart
renumberPages()      // Không nhất quán với pageCount.
convertToSomething() // Không theo quy ước toX().
wrappedAsSomething() // Không theo quy ước asX().
Cartesian            // Khó hiểu, ít quen thuộc.
```

> 🎯 **Mục tiêu:** Tận dụng kiến thức có sẵn của người dùng, tránh khiến họ phải học lại.

---

### ⚠️ AVOID: Viết tắt không cần thiết  
Chỉ viết tắt khi **từ viết tắt phổ biến hơn bản đầy đủ**, và viết hoa đúng quy tắc.

**✅ Tốt**
```dart
pageCount
buildRectangles
IOStream
HttpRequest
```

**❌ Xấu**
```dart
numPages    // “Num” là viết tắt không cần thiết.
buildRects
InputOutputStream
HypertextTransferProtocolRequest
```

---

### 💡 PREFER: Danh từ mô tả nên đặt **cuối cùng**  
Từ cuối cùng nên là danh từ chính mô tả đối tượng, các từ trước có thể là tính từ.

**✅ Tốt**
```dart
pageCount             // Đếm trang.
ConversionSink        // Nơi chuyển đổi.
ChunkedConversionSink // Loại ConversionSink theo khối.
CssFontFaceRule       // Quy tắc font CSS.
```

**❌ Xấu**
```dart
numPages
CanvasRenderingContext2D
RuleFontFaceCss
```

---

### 💬 CONSIDER: Khi không chắc, thử đọc code như câu tự nhiên  
Viết code rồi đọc lại xem có tự nhiên không.

**✅ Tốt**
```dart
if (errors.isEmpty) { ... }          // “Nếu danh sách lỗi trống...”
subscription.cancel();               // “Huỷ đăng ký!”
monsters.where((m) => m.hasClaws);   // “Lấy quái có móng vuốt.”
```

**❌ Xấu**
```dart
if (errors.empty) { ... }
subscription.toggle();
monsters.filter((m) => m.hasClaws);
```

> ❌ Tránh thêm từ “the”, “of”, hoặc viết câu quá dài chỉ để đọc giống ngữ pháp tiếng Anh.

---

### 🧱 PREFER: Dùng **cụm danh từ** cho thuộc tính hoặc biến không-boolean  
Nếu thuộc tính mô tả **“cái gì là gì”**, hãy dùng danh từ.

**✅ Tốt**
```dart
list.length
context.lineWidth
quest.rampagingSwampBeast
```

**❌ Xấu**
```dart
list.deleteItems
```

---

### ⚙️ PREFER: Dùng **cụm động từ không mệnh lệnh** cho biến hoặc thuộc tính boolean  
Tên boolean thường được dùng trong điều kiện, nên cần **đọc tự nhiên**.

**✅ Tốt**
```dart
isEmpty
hasElements
canClose
closesWindow
canShowPopup
hasShownPopup
```

**❌ Xấu**
```dart
empty
withElements
closeable
closingWindow
showPopup
```

> ❗ Tránh đặt tên boolean như mệnh lệnh (`showPopup` nghe như “thực thi hành động”).  
> Boolean chỉ nên **diễn tả trạng thái**, không thực hiện hành động.

---

### ✂️ CONSIDER: Lược bỏ động từ trong **tên tham số boolean**

Với tham số boolean đặt tên trong hàm, bỏ động từ giúp code gọn và dễ đọc hơn.

**✅ Tốt**
```dart
Isolate.spawn(entryPoint, message, paused: false);
var copy = List.from(elements, growable: true);
var regExp = RegExp(pattern, caseSensitive: false);
```

---

### ☯️ PREFER: Dùng tên **tích cực (positive)** cho boolean  
Tránh tên mang nghĩa phủ định (`isNotEmpty`, `isDisabled`, v.v.) nếu có thể.  
Điều này giúp tránh double negation trong điều kiện.

**✅ Tốt**
```dart
if (socket.isConnected && database.hasData) {
  socket.write(database.read());
}
```

**❌ Xấu**
```dart
if (!socket.isDisconnected && !database.isEmpty) {
  socket.write(database.read());
}
```

> ⚠️ Ngoại lệ: Nếu hầu hết trường hợp người dùng cần dùng **phủ định**, có thể chọn dạng negative.

---

### 🧾 PREFER: Dùng **động từ mệnh lệnh** cho hàm có tác dụng phụ  
Khi hàm **thực hiện hành động** hoặc **thay đổi trạng thái**, hãy dùng động từ chỉ hành động.

**✅ Tốt**
```dart
list.add('element');
queue.removeFirst();
window.refresh();
```

---

### 📦 PREFER: Dùng **cụm danh từ hoặc động từ phi mệnh lệnh** cho hàm trả về giá trị  
Nếu hàm **trả về kết quả**, không nên nghe như lệnh hành động.

**✅ Tốt**
```dart
var element = list.elementAt(3);
var first = list.firstWhere(test);
var char = string.codeUnitAt(4);
```

> 💬 Ví dụ như `take()` hay `split()` vẫn được — vì dễ hiểu và có nghĩa logic, dù là động từ.

---

## ✅ Tóm tắt ngắn gọn

| Quy tắc | Nên làm |
|----------|----------|
| Đặt tên nhất quán | Dùng chung tên cho cùng khái niệm |
| Tránh viết tắt | Trừ khi quá quen thuộc (IO, HTTP) |
| Danh từ chính ở cuối | “PageCount”, không phải “NumPages” |
| Dùng cụm danh từ cho non-boolean | `userName`, `pageCount` |
| Dùng cụm động từ cho boolean | `isVisible`, `hasError`, `canSave` |
| Tránh tên phủ định | `isConnected` > `isNotDisconnected` |
| Dùng động từ mệnh lệnh cho hàm thao tác | `addItem()`, `refresh()` |
| Dùng danh từ cho hàm trả giá trị | `firstWhere()`, `elementAt()` |

```
## 📚 REFERENCES

- Flutter Style Guide: https://flutter.dev/docs/development/ui/layout
- Effective Dart: https://dart.dev/guides/language/effective-dart
- Provider Package: https://pub.dev/packages/provider
