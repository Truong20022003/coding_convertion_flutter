
# 🎨 Hướng dẫn Quy ước Đặt tên & Nhập khẩu trong Flutter/Dart

> _Bản tiếng Việt có định dạng đẹp, kèm ví dụ tô màu (syntax highlighting) để dễ đọc trên GitHub/GitLab/VS Code._

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

---

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

---

## 🚫 KHÔNG sử dụng **dấu gạch dưới đầu** cho định danh private
> _Ghi chú: Quy tắc tuỳ nhóm. Mặc định Dart dùng `_name` để private-theo-library. Nếu nhóm bạn chọn **không dùng** tiền tố `_` cho private, hãy đảm bảo kiểm soát phạm vi truy cập qua cấu trúc thư mục, export và review._

---

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

## 🧾 **Định dạng (Formatting)**

Giống như nhiều ngôn ngữ khác, **Dart bỏ qua khoảng trắng (whitespace)** — nhưng **con người thì không** 😄.  
Việc tuân thủ một phong cách khoảng trắng nhất quán giúp người đọc nhìn code theo cùng một cách mà trình biên dịch hiểu.

---

### ✅ **DO: Định dạng code bằng `dart format`**

Việc căn chỉnh, xuống dòng, thụt đầu dòng… thủ công rất tốn thời gian — đặc biệt khi refactor.  
May mắn thay, Dart cung cấp công cụ định dạng tự động cực mạnh là:

```bash
dart format .
### 📏 **PREFER: Giữ độ dài dòng ≤ 80 ký tự**

Các nghiên cứu cho thấy **dòng chữ dài khiến mắt khó đọc và dễ mệt** — vì phải lia qua lại nhiều.  
Giống như **báo chí chia cột nhỏ để dễ đọc**, code ngắn gọn giúp bạn **dễ theo dõi và so sánh hơn**.

---

#### 💡 Nếu code của bạn thường vượt 80 ký tự, hãy xem lại:
- Tên class hoặc biến có quá dài không?  
- Mỗi từ trong tên có thực sự cần thiết không?

> Ví dụ:  
> `VeryLongCamelCaseClassName` ➜ có thể rút gọn lại cho ngắn gọn hơn.

---

#### ⚙️ Mặc định của Dart Formatter
- `dart format` **giới hạn 80 ký tự/dòng** (bạn có thể tùy chỉnh).  
- Tuy nhiên, formatter **không tự chia dòng cho chuỗi string**, bạn cần thực hiện thủ công.  
- **URI hoặc file path** có thể vượt quá 80 ký tự để dễ tìm kiếm.  
- **Multi-line string** có thể dài hơn 80 ký tự, vì ngắt dòng sẽ làm thay đổi nội dung thực tế.

---

### 🧩 **DO: Dùng dấu ngoặc nhọn `{}` cho tất cả các câu lệnh điều khiển**

> **Lint rule:** `curly_braces_in_flow_control_structures`

Việc này giúp **tránh lỗi "dangling else"** (trình biên dịch hiểu sai khối lệnh `else`).

---

#### ✅ **Tốt**
```dart
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}

## 💬 Bình luận & Ghi chú trong Code (Comments)

Đừng nghĩ code của bạn “rõ ràng rồi” — người khác (và cả bạn trong tương lai) **không có ngữ cảnh** như hiện tại.  
Một comment ngắn, rõ ràng có thể **tiết kiệm hàng giờ** cho người đọc.  
Code nên dễ hiểu, nhưng **comment đúng chỗ** giúp người khác hiểu nhanh hơn.

---

### ✅ Viết comment như một câu hoàn chỉnh
- Bắt đầu bằng chữ hoa, kết thúc bằng dấu `.`  
- Áp dụng cho mọi comment (kể cả `TODO`).

**Tốt**
```dart
// Không chạy nếu đã có dữ liệu trước đó.
if (_chunks.isNotEmpty) return false;

## 🚫 Không dùng block comment để mô tả

Chỉ dùng `/* ... */` để **tạm tắt code**, không dùng để mô tả logic.

**✅ Tốt**
```dart
// Giả sử tên hợp lệ.
print('Hi, $name!');
❌ Xấu
/* Giả sử tên hợp lệ. */
print('Hi, $name!');


## 🌫️ Null

---

### 🚫 Không khởi tạo biến bằng `null` một cách tường minh  
> **Linter:** `avoid_init_to_null`

Nếu biến có kiểu **không nullable**, Dart sẽ báo lỗi khi dùng nó trước khi được gán giá trị.  
Nếu biến là **nullable**, Dart sẽ **tự động gán giá trị `null`** mặc định — **không cần** gán thủ công.  
Dart không có khái niệm “vùng nhớ chưa khởi tạo”, nên việc gán `= null` là **thừa**.

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

## 💡 Cân nhắc: Type Promotion & Null-check Pattern khi dùng kiểu Nullable

Khi bạn kiểm tra một biến nullable khác `null`, Dart sẽ **tự động "promote"** nó thành kiểu **non-nullable** — giúp bạn truy cập thuộc tính hoặc truyền vào hàm yêu cầu non-null dễ dàng.

Tuy nhiên, **type promotion chỉ hoạt động với**:
- Biến cục bộ (`local variable`)
- Tham số (`parameter`)
- Thuộc tính `private final`

Nếu giá trị có thể bị thay đổi ở nơi khác, Dart **không thể promote** vì không đảm bảo an toàn.

> ✅ Để khắc phục, bạn có thể:
> - Dùng **pattern matching với null-check**
> - Hoặc gán vào biến cục bộ trước khi kiểm tra

---

### ✅ Cách 1: Dùng **null-check pattern**

**Tốt**
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
### ✅ Cách 2:Gán vào biến cục bộ trước khi kiểm tra**
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

### Xấu**
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


## 🧵 Chuỗi ký tự (Strings)

Khi xử lý chuỗi trong **Dart**, hãy giữ code **ngắn gọn, dễ đọc** và **tuân theo quy tắc chuẩn**.

---

### ✅ Dùng chuỗi liền kề để nối *string literal*
> **Linter:** `prefer_adjacent_string_concatenation`

Khi nối **chuỗi literal** (chuỗi viết trực tiếp, không phải biến), **không cần dùng dấu `+`**.  
Chỉ cần **đặt chúng cạnh nhau** — Dart sẽ tự động nối lại.

---

**✅ Tốt**
```dart
raiseAlarm(
  'ERROR: Phần thân tàu đang cháy. Các phần khác '
  'đang bị người sao Hỏa chiếm đóng. Không rõ phần nào là phần nào.',
);


**❌ Xấu**
raiseAlarm(
  'ERROR: Phần thân tàu đang cháy. Các phần khác ' +
      'đang bị người sao Hỏa chiếm đóng. Không rõ phần nào là phần nào.',
);

## 💬 Chuỗi & Tập hợp (Strings & Collections)

---

### 💡 PREFER: Dùng nội suy chuỗi (interpolation) để ghép chuỗi và giá trị  
> **Linter:** `prefer_interpolation_to_compose_strings`

Trong Dart, bạn **nên dùng nội suy chuỗi** thay vì nối chuỗi bằng `+`.  
Cách này **ngắn gọn, dễ đọc** và **hiệu quả hơn**.

**✅ Tốt**
```dart
'Hello, $name! You are ${year - birth} years old.';
❌ Xấu

dart
Copy code
'Hello, ' + name + '! You are ' + (year - birth).toString() + ' years old.';
✅ Dùng .toString() chỉ khi cần chuyển một giá trị duy nhất thành chuỗi.

⚠️ AVOID: Dùng ngoặc nhọn {} trong nội suy khi không cần thiết
Linter: unnecessary_brace_in_string_interps

Nếu bạn chỉ chèn tên biến đơn giản, không cần dùng {} trừ khi ngay sau đó là ký tự chữ hoặc số khác.

✅ Tốt

dart
Copy code
var greeting = 'Hi, $name! I love your ${decade}s costume.';
❌ Xấu

dart
Copy code
var greeting = 'Hi, ${name}! I love your ${decade}s costume.';
🧩 Collections (Tập hợp)
Dart hỗ trợ 4 loại tập hợp chính: List, Map, Queue, và Set.
Dưới đây là các quy tắc hay khi làm việc với chúng.

✅ DO: Dùng cú pháp literal để tạo collection
Linter: prefer_collection_literals

Thay vì khởi tạo bằng Map() hoặc Set(), hãy dùng cú pháp ngắn gọn với {}, [], và < >.

✅ Tốt

dart
Copy code
var points = <Point>[];
var addresses = <String, Address>{};
var counts = <int>{};
❌ Xấu

dart
Copy code
var addresses = Map<String, Address>();
var counts = Set<int>();
💡 Cú pháp literal giúp bạn dùng được spread (...), null-spread (...?), và if/for trong collection.
