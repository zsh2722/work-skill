# 多语言编码与命名规范

> 适用语言：C、C++、Java、Kotlin、Dart、Go、Python<br>
> 文档版本：1.0<br>
> 更新日期：2026-07-09<br>
> 建议文件名：`CODING_STYLE.md`

---

## 目录

- [1. 文档目标](#1-文档目标)
- [2. 规范优先级](#2-规范优先级)
- [3. 规则等级](#3-规则等级)
- [4. 多语言通用原则](#4-多语言通用原则)
- [5. 各语言命名风格速查](#5-各语言命名风格速查)
- [6. C 编码与命名规范](#6-c-编码与命名规范)
- [7. C++ 编码与命名规范](#7-c-编码与命名规范)
- [8. Java 编码与命名规范](#8-java-编码与命名规范)
- [9. Kotlin 编码与命名规范](#9-kotlin-编码与命名规范)
- [10. Dart 编码与命名规范](#10-dart-编码与命名规范)
- [11. Go 编码与命名规范](#11-go-编码与命名规范)
- [12. Python 编码与命名规范](#12-python-编码与命名规范)
- [13. 注释与文档统一要求](#13-注释与文档统一要求)
- [14. Git、提交与代码评审建议](#14-git提交与代码评审建议)
- [15. 自动格式化与静态检查](#15-自动格式化与静态检查)
- [16. Code Review 通用检查清单](#16-code-review-通用检查清单)
- [17. 官方与主要参考资料](#17-官方与主要参考资料)

---

# 1. 文档目标

本规范用于统一多语言项目中的：

- 标识符命名；
- 文件和目录命名；
- 源代码排版；
- 函数、类、模块和包的组织；
- 注释和 API 文档；
- 错误处理和资源管理；
- 自动格式化、静态检查和代码评审要求。

本规范不是把所有语言强制改成同一种风格，而是保留每种语言的官方惯例和生态习惯。

例如：

- Java、Kotlin、Dart 通常使用 `UpperCamelCase` 和 `lowerCamelCase`；
- C、C++、Python 大量使用 `lower_snake_case`；
- Go 使用 `MixedCaps`，并通过首字母大小写控制标识符是否导出；
- Dart 常量通常仍使用 `lowerCamelCase`，不默认使用 `SCREAMING_SNAKE_CASE`；
- Python 常量通常使用 `UPPER_SNAKE_CASE`；
- C++ 不存在全行业唯一命名法，新增项目必须选择一套并保持一致。

---

# 2. 规范优先级

发生冲突时，按以下顺序执行：

1. 上游仓库、框架或组织明确规定的规范；
2. 当前仓库中的 `CONTRIBUTING.md`、`STYLE.md`、`CODING_STYLE.md`；
3. 当前仓库中的格式化和检查配置；
4. 当前模块、目录或相邻代码已经形成的稳定风格；
5. 本文档；
6. 开发人员个人偏好。

必须遵守以下原则：

- 修改第三方或上游仓库时，以该仓库规则为准；
- 修复一个小问题时，不应顺便格式化整个文件或整个项目；
- 不应为了个人偏好制造大量无业务意义的格式变更；
- 新项目应尽早提交格式化配置，并由 CI 自动检查；
- 同一个项目、模块或文件内的一致性，比个人认为“更漂亮”的写法更重要。

---

# 3. 规则等级

| 等级 | 含义 |
|---|---|
| **必须** | 新增代码必须遵守；违反时应在代码评审中阻止合并 |
| **推荐** | 无明确理由时应遵守；偏离时应能够说明原因 |
| **可选** | 由项目或模块自行决定，但一旦选定应保持一致 |
| **禁止** | 新增代码中不允许使用 |

---

# 4. 多语言通用原则

## 4.1 命名应表达业务含义

推荐：

```text
retry_count
connectionTimeout
calculateTotalPrice
UserRepository
is_authenticated
```

不推荐：

```text
n
num1
str2
flag
data
obj
thing
foo
bar
```

说明：

- 名称应优先描述“是什么”或“做什么”；
- 不应只描述数据类型；
- 作用域越大，名称应越完整；
- 极小作用域内可以使用约定俗成的短名称。

允许的常见短名称：

```text
i, j           循环索引
x, y, z        坐标或数学变量
id             标识符
fd             文件描述符
it             迭代器
ctx            上下文
err            错误值
src, dst       来源和目标
lhs, rhs       左、右操作数
req, resp      请求和响应
```

短名称必须建立在上下文足够清晰的前提下。

---

## 4.2 使用英文标识符

必须：

```text
user_name
calculatePrice
NetworkClient
```

禁止：

```text
yong_hu_ming
jisuanjiage
yhxx
```

要求：

- 标识符使用英文；
- 不使用拼音；
- 不使用只有作者能理解的内部缩写；
- 面向国际协作的项目优先使用 ASCII 标识符；
- 中文可用于注释、文档和用户界面文本，但不建议用于代码标识符。

---

## 4.3 避免无意义缩写

推荐：

```text
connection_timeout
calculateAverageValue
customerIdentifier
```

不推荐：

```text
conn_to
calc_avg_val
cstmr_id
```

允许使用行业内普遍认可的缩写：

```text
API, CPU, GPU, HTTP, HTTPS, TCP, UDP
URL, URI, UUID, JSON, XML, SQL
ID, UI, IO, IP, DNS, TLS
```

缩写的大小写形式必须遵循对应语言习惯。例如：

| 语言 | 推荐示例 |
|---|---|
| Java | `HttpClient`, `userId` |
| Kotlin | `HttpClient`, `userId` |
| Dart | `HttpClient`, `userId` |
| Go | `HTTPClient`, `userID` |
| Python | `http_client`, `user_id` |
| C | `http_client`, `user_id` |
| C++（本文默认） | `HttpClient`, `user_id` |

---

## 4.4 禁止匈牙利命名法

禁止在名称中重复编码变量类型。

不推荐：

```text
strUserName
pszUserName
iUserCount
bConnected
m_userName
```

推荐：

```text
userName
user_name
userCount
isConnected
```

说明：

- 类型信息应由语言类型系统、IDE 或静态分析工具提供；
- 类型变化不应导致变量名称立即失去意义；
- 项目已有稳定的成员前缀规范时，可以继续遵守，但新增项目不建议采用。

---

## 4.5 布尔名称必须能自然回答“是或否”

推荐前缀：

```text
is
has
can
should
needs
supports
contains
allows
```

示例：

```text
isReady
hasPermission
canRetry
shouldRefresh
needsUpdate
supportsTls
containsUser
```

不推荐：

```text
flag
status
state
value
check
result
```

避免双重否定：

不推荐：

```text
isNotDisabled
notInvalid
```

推荐：

```text
isEnabled
isValid
```

---

## 4.6 集合名称通常使用复数

推荐：

```text
users
activeUsers
user_list
supported_formats
```

不推荐：

```text
user
userData
list1
arrayData
```

当对象是“集合类型本身”而不是集合实例时，可以使用单数类型名：

```text
UserList
UserCollection
```

---

## 4.7 名称中明确单位

推荐：

```text
timeout_ms
connectionTimeoutMillis
buffer_size_bytes
maxRetries
```

更推荐使用强类型单位：

```cpp
std::chrono::milliseconds timeout;
```

```kotlin
val timeout: Duration
```

```dart
final Duration timeout;
```

禁止在上下文不明确时使用：

```text
timeout
size
length
interval
```

---

## 4.8 避免视觉混淆

禁止使用容易混淆的单字符名称：

```text
l
I
O
```

避免：

```text
userCount
usersCount
user_count
users_count
```

应明确区分：

```text
activeUserCount
totalUserCount
```

---

## 4.9 一个函数只承担一个主要职责

推荐：

```text
parseRequest()
validateRequest()
executeRequest()
```

不推荐：

```text
parseValidateExecuteAndSaveRequest()
```

要求：

- 函数名称应能概括函数的主要行为；
- 函数内部出现多个明显阶段时，应考虑拆分；
- 嵌套过深时，优先使用提前返回、辅助函数或多态；
- 不以机械行数作为唯一标准，但普通业务函数应保持可快速理解。

---

## 4.10 注释解释“为什么”，代码表达“怎么做”

推荐：

```text
// Retry once because the server may still be switching leaders.
```

不推荐：

```text
// Retry the request.
retryRequest();
```

禁止长期保留大段注释掉的旧代码。历史代码应由 Git 保存。

---

# 5. 各语言命名风格速查

| 对象 | C | C++（本文默认） | Java | Kotlin | Dart | Go | Python |
|---|---|---|---|---|---|---|---|
| 文件 | `lower_snake.c` | `lower_snake.cpp` | `UpperCamel.java` | `UpperCamel.kt` | `lower_snake.dart` | `lowercase.go` | `lower_snake.py` |
| 包/模块 | 前缀式 | `lower_snake` 命名空间 | 全小写点分 | 全小写点分 | `lower_snake` | 小写单词 | 全小写，必要时下划线 |
| 类/类型 | `struct user_info` | `UserInfo` | `UserInfo` | `UserInfo` | `UserInfo` | `UserInfo`（导出） | `UserInfo` |
| 接口 | 不适用 | `UserStore` | `UserRepository` | `UserRepository` | `UserRepository` | `Reader` | `UserRepository` |
| 函数/方法 | `load_user` | `LoadUser` | `loadUser` | `loadUser` | `loadUser` | `LoadUser` / `loadUser` | `load_user` |
| 普通变量 | `user_count` | `user_count` | `userCount` | `userCount` | `userCount` | `userCount` | `user_count` |
| 常量 | `MODULE_MAX_SIZE` | `kMaxSize` | `MAX_SIZE` | `MAX_SIZE` | `maxSize` | `MaxSize` / `maxSize` | `MAX_SIZE` |
| 私有成员 | 模块 `static` | `user_name_` | `userName` | `userName` | `_userName` | `userName` | `_user_name` |
| 枚举类型 | `enum state` | `ConnectionState` | `ConnectionState` | `ConnectionState` | `ConnectionState` | `ConnectionState` | `ConnectionState` |
| 枚举值 | `STATE_READY` | `kReady` | `READY` | `READY` | `ready` | `StateReady` | `READY` |
| 测试文件 | `*_test.c` | `*_test.cpp` | `*Test.java` | `*Test.kt` | `*_test.dart` | `*_test.go` | `test_*.py` |

> 注意：表格用于快速对比，详细规则以对应语言章节为准。

---

# 6. C 编码与命名规范

## 6.1 适用说明

C 语言没有命名空间，公共符号容易发生冲突，因此公共 API 必须使用模块前缀。

本文对普通跨平台 C 项目采用：

- `lower_snake_case` 标识符；
- 模块前缀；
- 自动格式化；
- 明确所有权；
- 尽量缩小符号作用域。

Linux 内核、CPython、FreeBSD 等大型 C 仓库都有自己的细节规则。向这些仓库贡献代码时，必须优先遵守其上游规范，不应直接套用本章节的缩进和排版细节。

## 6.2 C 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 源文件 | `lower_snake_case.c` | `http_client.c` |
| 头文件 | `lower_snake_case.h` | `http_client.h` |
| 函数 | `lower_snake_case` | `http_client_connect()` |
| 普通变量 | `lower_snake_case` | `retry_count` |
| 函数参数 | `lower_snake_case` | `buffer_size` |
| 结构体标签 | `lower_snake_case` | `struct user_info` |
| 联合体标签 | `lower_snake_case` | `union packet_data` |
| 枚举类型 | `lower_snake_case` | `enum connection_state` |
| 枚举值 | `MODULE_UPPER_SNAKE_CASE` | `HTTP_STATE_CONNECTED` |
| 常量宏 | `MODULE_UPPER_SNAKE_CASE` | `HTTP_MAX_RETRY_COUNT` |
| 函数式宏 | `MODULE_UPPER_SNAKE_CASE` | `ARRAY_SIZE(x)` |
| 内部静态函数 | `lower_snake_case` | `parse_header()` |
| `typedef` | 项目统一 | `user_info_t` 或 `user_info` |

## 6.3 公共 API 必须包含模块前缀

推荐：

```c
int http_client_connect(struct http_client *client);
void http_client_disconnect(struct http_client *client);
size_t byte_buffer_size(const struct byte_buffer *buffer);
```

禁止：

```c
int connect(void);
void destroy(void);
size_t size(void);
```

推荐结构：

```text
模块_动作_对象
模块_对象_动作
```

示例：

```c
user_create();
user_destroy();
user_find_by_id();

buffer_init();
buffer_append();
buffer_clear();

http_client_connect();
http_client_send();
http_client_close();
```

## 6.4 限制符号作用域

文件内部函数和变量必须优先声明为 `static`：

```c
static bool parse_header(
    const char *input,
    struct http_header *header)
{
    /* ... */
}
```

不得把仅在一个 `.c` 文件中使用的符号暴露到公共头文件。

## 6.5 结构体和 `typedef`

推荐保留结构体标签：

```c
struct user_info {
    int user_id;
    const char *user_name;
};
```

项目决定使用 `_t` 后缀时，应全项目统一：

```c
typedef struct user_info {
    int user_id;
    const char *user_name;
} user_info_t;
```

注意：

- POSIX 保留了大量以 `_t` 结尾的名称；
- 面向 POSIX 的公共库应谨慎创建新的全局 `_t` 名称；
- 不应在同一个模块中混用 `struct user_info`、`UserInfo`、`USER_INFO` 等多种风格。

## 6.6 枚举必须避免全局名称冲突

推荐：

```c
enum http_client_state {
    HTTP_CLIENT_STATE_DISCONNECTED,
    HTTP_CLIENT_STATE_CONNECTING,
    HTTP_CLIENT_STATE_CONNECTED,
    HTTP_CLIENT_STATE_FAILED,
};
```

禁止：

```c
enum state {
    READY,
    ERROR,
    CLOSED,
};
```

## 6.7 宏

必须：

- 宏名称使用大写下划线；
- 公共宏添加项目或模块前缀；
- 函数式宏中的参数和完整表达式加括号；
- 多语句宏使用安全封装；
- 能用函数、`enum`、`const` 或语言特性解决时，不优先使用宏。

示例：

```c
#define APP_ARRAY_SIZE(array) \
    (sizeof(array) / sizeof((array)[0]))
```

多语句宏：

```c
#define APP_LOG_AND_RETURN(error_code) \
    do {                               \
        app_log_error(error_code);     \
        return (error_code);           \
    } while (0)
```

## 6.8 缩进和大括号

普通跨平台项目建议：

- 4 个空格缩进；
- 不使用 Tab 进行手工对齐；
- 左大括号风格由 `.clang-format` 固定；
- 控制语句始终使用大括号。

示例：

```c
if (client == NULL) {
    return HTTP_ERROR_INVALID_ARGUMENT;
}
```

禁止：

```c
if (client == NULL)
    return HTTP_ERROR_INVALID_ARGUMENT;
```

> Linux 内核代码应遵守内核自己的 Tab、8 字符缩进及大括号规则。

## 6.9 声明和初始化

必须：

- 变量尽量在首次使用附近声明；
- 声明时初始化；
- 一次声明一个变量；
- 不在一行中混合不同指针层级。

推荐：

```c
int width = 0;
int height = 0;
char *buffer = NULL;
```

禁止：

```c
int width = 0, height = 0;
char *buffer, value;
```

## 6.10 所有权和资源释放

公共 API 必须明确：

- 谁创建资源；
- 谁释放资源；
- 参数是否允许为 `NULL`；
- 返回指针的生命周期；
- 返回内存是否由调用方释放；
- 函数是否保留传入指针。

推荐命名：

```c
widget_create();
widget_destroy();

buffer_init();
buffer_deinit();
```

创建和销毁函数应成对出现。

## 6.11 C 示例

```c
#ifndef APP_HTTP_CLIENT_H
#define APP_HTTP_CLIENT_H

#include <stdbool.h>
#include <stddef.h>

#ifdef __cplusplus
extern "C" {
#endif

enum http_client_state {
    HTTP_CLIENT_STATE_DISCONNECTED,
    HTTP_CLIENT_STATE_CONNECTING,
    HTTP_CLIENT_STATE_CONNECTED,
    HTTP_CLIENT_STATE_FAILED,
};

struct http_client;

struct http_client_options {
    const char *server_address;
    unsigned int server_port;
    unsigned int timeout_ms;
};

struct http_client *http_client_create(
    const struct http_client_options *options);

void http_client_destroy(struct http_client *client);

bool http_client_connect(struct http_client *client);

void http_client_disconnect(struct http_client *client);

bool http_client_is_connected(
    const struct http_client *client);

#ifdef __cplusplus
}
#endif

#endif /* APP_HTTP_CLIENT_H */
```

---

# 7. C++ 编码与命名规范

## 7.1 适用说明

C++ 标准本身不规定唯一命名风格。Google、LLVM、Chromium、Qt、Unreal、Boost 等仓库存在明显差异。

本文为新建通用 C++ 项目选择以下默认风格：

- 类型：`PascalCase`；
- 函数：`PascalCase`；
- 普通变量：`lower_snake_case`；
- 私有成员：`lower_snake_case_`；
- 常量：`kPascalCase`；
- 宏：`PROJECT_UPPER_SNAKE_CASE`。

向既有项目贡献代码时，必须服从上游项目风格。

## 7.2 C++ 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 文件 | `lower_snake_case` | `http_client.cpp` |
| 目录 | `lower_snake_case` | `network_utils/` |
| 命名空间 | `lower_snake_case` | `image_processing` |
| 类 | `PascalCase` | `HttpClient` |
| 结构体 | `PascalCase` | `UserInfo` |
| 枚举类型 | `PascalCase` | `ConnectionState` |
| 类型别名 | `PascalCase` | `UserList` |
| Concept | `PascalCase` | `SortableRange` |
| 模板类型参数 | `PascalCase` 或惯用单字母 | `ValueType`, `T` |
| 普通变量 | `lower_snake_case` | `user_count` |
| 参数 | `lower_snake_case` | `request_timeout` |
| 类私有成员 | `lower_snake_case_` | `user_name_` |
| `struct` 公共成员 | `lower_snake_case` | `user_name` |
| 函数 | `PascalCase` | `CalculateTotal()` |
| 成员函数 | `PascalCase` | `ConnectServer()` |
| 简单 Getter | `lower_snake_case` | `user_name()` |
| Setter | `set_lower_snake_case` | `set_user_name()` |
| 常量 | `kPascalCase` | `kMaxRetryCount` |
| 枚举值 | `kPascalCase` | `kConnected` |
| 宏 | `PROJECT_UPPER_SNAKE_CASE` | `APP_MAX_BUFFER_SIZE` |

## 7.3 文件

推荐：

```text
http_client.h
http_client.cpp
http_client_test.cpp
```

要求：

- 头文件和实现文件使用相同基础名称；
- 项目统一选择 `.cpp` 或 `.cc`；
- 不在同一项目中无规则混用；
- 测试文件使用统一后缀，例如 `_test.cpp`。

## 7.4 命名空间

推荐：

```cpp
namespace app::network {

class HttpClient {
};

}  // namespace app::network
```

禁止在头文件中：

```cpp
using namespace std;
```

源文件中也应避免大范围 `using namespace`。

## 7.5 类和结构体

类名使用名词或名词短语：

```cpp
class NetworkManager;
class ImageProcessor;
class UserRepository;

struct UserInfo;
struct ConnectionOptions;
```

不推荐：

```cpp
class ManageNetwork;
class ProcessImage;
class DoRequest;
```

## 7.6 函数

函数名称使用动词或动词短语：

```cpp
LoadConfiguration();
CreateConnection();
CalculateTotalPrice();
RemoveExpiredItems();
```

布尔判断：

```cpp
IsReady();
HasPermission();
CanRetry();
ContainsUser();
```

避免：

```cpp
DoWork();
Handle();
Process();
Func1();
```

当类上下文已经非常清晰时，短名称可以接受：

```cpp
class ImageProcessor {
 public:
    void Process();
};
```

## 7.7 Getter 和 Setter

简单属性访问器：

```cpp
class User {
 public:
    const std::string& user_name() const {
        return user_name_;
    }

    void set_user_name(std::string user_name) {
        user_name_ = std::move(user_name);
    }

 private:
    std::string user_name_;
};
```

当函数包含计算、查询、加载或副作用时，使用动词：

```cpp
CalculateTotalPrice();
LoadUserName();
QueryUserStatus();
```

## 7.8 常量和枚举

推荐：

```cpp
constexpr int kMaxRetryCount = 3;
constexpr std::size_t kDefaultBufferSize = 4096;

enum class ConnectionState {
    kDisconnected,
    kConnecting,
    kConnected,
    kFailed,
};
```

禁止把普通常量写成宏：

```cpp
#define MAX_RETRY_COUNT 3
```

优先使用：

```text
constexpr
constinit
inline constexpr
enum class
```

## 7.9 初始化和类型推导

必须：

- 变量声明时初始化；
- 使用 `nullptr`；
- 不使用 C 风格强制转换；
- `auto` 应提高可读性，而不是隐藏关键类型。

推荐：

```cpp
int retry_count = 0;
User* user = nullptr;
auto iterator = users.find(user_id);
auto value = static_cast<int>(size);
```

禁止：

```cpp
User* user = NULL;
int value = (int)size;
```

## 7.10 资源管理

必须优先使用 RAII：

```cpp
auto user = std::make_unique<User>();
std::lock_guard<std::mutex> lock(mutex_);
```

所有权规则：

| 类型 | 含义 |
|---|---|
| `T&` | 必须存在、非拥有引用 |
| `const T&` | 只读、非拥有引用 |
| `T*` | 可空、默认非拥有指针 |
| `std::unique_ptr<T>` | 唯一所有权 |
| `std::shared_ptr<T>` | 确实存在共享所有权 |
| `std::weak_ptr<T>` | 非拥有共享观察者 |

禁止为了方便把所有对象都改成 `shared_ptr`。

## 7.11 头文件

头文件必须能够独立包含：

```cpp
#ifndef APP_NETWORK_HTTP_CLIENT_H_
#define APP_NETWORK_HTTP_CLIENT_H_

#include <string>

namespace app::network {

class HttpClient {
 public:
    explicit HttpClient(std::string address);

 private:
    std::string address_;
};

}  // namespace app::network

#endif  // APP_NETWORK_HTTP_CLIENT_H_
```

要求：

- 不依赖间接包含；
- 不在头文件中使用 `using namespace`；
- 尽量减少头文件依赖；
- 可使用前置声明时，应评估完整类型要求；
- 模板实现通常需要放在头文件中；
- 复杂非模板实现放入源文件。

## 7.12 Include 顺序

推荐：

```cpp
#include "current_file.h"

#include <cstdint>
#include <string>
#include <vector>

#include <third_party/library.h>

#include "app/base/logging.h"
#include "app/network/socket.h"
```

同组按字母排序，由工具自动处理。

## 7.13 C++ 示例

```cpp
#ifndef APP_NETWORK_HTTP_CLIENT_H_
#define APP_NETWORK_HTTP_CLIENT_H_

#include <chrono>
#include <string>

namespace app::network {

enum class ConnectionState {
    kDisconnected,
    kConnecting,
    kConnected,
    kFailed,
};

struct ConnectionOptions {
    std::string server_address;
    int server_port = 443;
    std::chrono::milliseconds timeout{5000};
};

class HttpClient {
 public:
    explicit HttpClient(ConnectionOptions options);

    bool Connect();
    void Disconnect();

    bool is_connected() const {
        return state_ == ConnectionState::kConnected;
    }

    const std::string& server_address() const {
        return options_.server_address;
    }

 private:
    bool ValidateOptions() const;

    ConnectionOptions options_;
    ConnectionState state_ = ConnectionState::kDisconnected;
};

}  // namespace app::network

#endif  // APP_NETWORK_HTTP_CLIENT_H_
```

---

# 8. Java 编码与命名规范

## 8.1 Java 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 源文件 | 与顶级类型同名 | `HttpClient.java` |
| 包 | 全小写点分 | `com.example.network` |
| 模块 | 全小写点分 | `com.example.app` |
| 类 | `UpperCamelCase` | `HttpClient` |
| 接口 | `UpperCamelCase` | `UserRepository` |
| 枚举 | `UpperCamelCase` | `ConnectionState` |
| 注解 | `UpperCamelCase` | `VisibleForTesting` |
| 方法 | `lowerCamelCase` | `calculateTotal()` |
| 字段 | `lowerCamelCase` | `userCount` |
| 局部变量 | `lowerCamelCase` | `retryCount` |
| 参数 | `lowerCamelCase` | `requestTimeout` |
| 常量 | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| 类型参数 | 单个大写字母或简短类型名 | `T`, `E`, `K`, `V` |
| 测试类 | `UpperCamelCase + Test` | `HttpClientTest` |

## 8.2 包名

必须：

- 全小写；
- 使用反向域名形成唯一前缀；
- 不使用大写字母；
- 不使用连字符；
- 避免无意义层级。

推荐：

```java
package com.example.payment.api;
```

不推荐：

```java
package Com.Example.PaymentAPI;
package com.example.payment_api;
```

## 8.3 类和接口

类名通常使用名词或名词短语：

```java
class UserService {}
class HttpClient {}
class PaymentRequest {}
```

接口名称应表达能力、角色或契约：

```java
interface Runnable {}
interface Closeable {}
interface UserRepository {}
interface PaymentGateway {}
```

禁止无必要添加 `I` 前缀：

```java
interface IUserRepository {}
```

除非项目或框架已经明确采用该规则。

## 8.4 方法

方法使用动词或动词短语：

```java
loadUser();
calculateTotalPrice();
removeExpiredItems();
```

常见约定：

```java
getName();
setName();
isReady();
hasPermission();
toJson();
fromJson();
```

布尔方法优先：

```java
isEmpty();
isEnabled();
hasNext();
canRetry();
```

避免：

```java
check();
handle();
doWork();
processData();
```

除非上下文已经足够明确。

## 8.5 常量

真正的常量使用 `UPPER_SNAKE_CASE`：

```java
private static final int MAX_RETRY_COUNT = 3;
private static final Duration DEFAULT_TIMEOUT = Duration.ofSeconds(5);
```

不是所有 `static final` 字段都必须视为常量。可变对象引用不应因为 `final` 就机械使用大写名称。

## 8.6 字段

推荐：

```java
private final UserRepository userRepository;
private int retryCount;
```

不推荐：

```java
private final UserRepository mUserRepository;
private int _retryCount;
```

除非上游项目已有明确规则。

## 8.7 类型参数

推荐使用约定单字母：

```java
class Box<T> {}
interface List<E> {}
interface Map<K, V> {}
```

多个任意类型需要区分时：

```java
class Converter<SourceT, TargetT> {}
```

项目使用长类型参数名时，应统一后缀风格。

## 8.8 格式

本文建议采用 Google Java Style：

- 2 或 4 空格必须由项目统一；
- 使用自动格式化器时，不手工对齐；
- 左大括号不单独换行；
- 控制语句始终使用大括号；
- 单行最大宽度建议 100 个字符；
- 每个顶级公开类独立文件；
- 一个源文件只声明一个公共顶级类型。

示例：

```java
if (request.isValid()) {
  sendRequest(request);
} else {
  reportInvalidRequest(request);
}
```

## 8.9 Import

必须：

- 不使用通配符导入；
- 删除未使用导入；
- 静态导入与普通导入分组；
- 导入顺序由格式化工具固定。

不推荐：

```java
import java.util.*;
```

推荐：

```java
import java.util.List;
import java.util.Map;
```

## 8.10 异常

要求：

- 不捕获后完全忽略异常；
- 不使用异常控制普通业务流程；
- 捕获尽量具体的异常类型；
- 重新抛出时保留原始异常作为 cause；
- 自定义异常以 `Exception` 或 `Error` 结尾。

推荐：

```java
throw new ConfigurationException(
    "Failed to load configuration", exception);
```

禁止：

```java
try {
  loadConfiguration();
} catch (Exception ignored) {
}
```

## 8.11 Java 示例

```java
package com.example.network;

import java.time.Duration;
import java.util.Objects;

public final class HttpClient {
  private static final int MAX_RETRY_COUNT = 3;

  private final String serverAddress;
  private final Duration timeout;

  public HttpClient(String serverAddress, Duration timeout) {
    this.serverAddress = Objects.requireNonNull(serverAddress);
    this.timeout = Objects.requireNonNull(timeout);
  }

  public boolean connect() {
    if (serverAddress.isBlank()) {
      return false;
    }

    return connectWithRetry(MAX_RETRY_COUNT);
  }

  public String getServerAddress() {
    return serverAddress;
  }

  private boolean connectWithRetry(int maxRetryCount) {
    // Implementation omitted.
    return true;
  }
}
```

---

# 9. Kotlin 编码与命名规范

## 9.1 Kotlin 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 单一类型文件 | 与类型同名 | `HttpClient.kt` |
| 多声明文件 | `UpperCamelCase.kt` | `NetworkUtils.kt` |
| 包 | 全小写点分，通常无下划线 | `com.example.network` |
| 类 | `UpperCamelCase` | `HttpClient` |
| 接口 | `UpperCamelCase` | `UserRepository` |
| 对象 | `UpperCamelCase` | `DefaultLogger` |
| 函数 | `lowerCamelCase` | `calculateTotal()` |
| 属性 | `lowerCamelCase` | `userCount` |
| 参数 | `lowerCamelCase` | `requestTimeout` |
| 常量 | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| 枚举值 | `UPPER_SNAKE_CASE` | `CONNECTED` |
| 类型参数 | 单个大写字母或 `UpperCamelCase` | `T`, `Key`, `Value` |
| 后备字段 | 前导下划线 | `_items` |
| 测试文件 | `UpperCamelCase + Test.kt` | `HttpClientTest.kt` |

## 9.2 文件

单个类或接口：

```text
HttpClient.kt
UserRepository.kt
```

包含多个相关顶层声明：

```text
NetworkUtils.kt
ProcessDeclarations.kt
```

禁止使用无意义文件名：

```text
Utils.kt
Common.kt
Helper.kt
Misc.kt
```

除非文件内容确实有稳定、明确的公共职责。

## 9.3 包名

推荐：

```kotlin
package com.example.network.client
```

要求：

- 全小写；
- 通常不使用下划线；
- 包层级与目录结构保持一致；
- Kotlin 与 Java 混合项目遵循同一源目录结构。

## 9.4 类、函数和属性

推荐：

```kotlin
class UserRepository
interface PaymentGateway

fun loadUser(userId: Long): User
val retryCount: Int
```

布尔属性：

```kotlin
val isReady: Boolean
val hasPermission: Boolean
val canRetry: Boolean
```

## 9.5 常量

仅对真正的常量或静态深度不可变值使用大写下划线：

```kotlin
const val MAX_RETRY_COUNT = 3

val DEFAULT_TIMEOUT: Duration = 5.seconds
```

普通只读属性仍使用 `lowerCamelCase`：

```kotlin
val userName: String
val connectionTimeout: Duration
```

不要把所有 `val` 都命名成大写。

## 9.6 后备属性

需要公开只读视图时，私有可变后备属性使用前导下划线：

```kotlin
private val _items = mutableListOf<Item>()
val items: List<Item>
    get() = _items
```

不要给普通私有字段机械添加下划线。

## 9.7 函数设计

推荐利用 Kotlin 语言特性降低样板代码：

```kotlin
fun findUser(userId: Long): User? =
    users[userId]
```

避免过度使用扩展函数：

- 与类型本身普遍相关的扩展，可放在类型附近；
- 仅对特定调用方有意义的扩展，应放在调用方附近；
- 不建立一个存放所有扩展的超大 `Extensions.kt`。

## 9.8 空安全

必须：

- 优先使用可空类型表达可能为空；
- 避免使用 `!!`；
- 使用 Elvis、提前返回或显式校验；
- Java 互操作的平台类型应尽快转换为明确可空或非空类型。

推荐：

```kotlin
val user = repository.findUser(userId)
    ?: return Result.failure(UserNotFoundException(userId))
```

不推荐：

```kotlin
val user = repository.findUser(userId)!!
```

## 9.9 格式

建议使用 Kotlin 官方风格：

- 4 个空格缩进；
- 不使用 Tab；
- 最大行宽建议 120；
- `else`、`catch`、`finally` 与前一个右大括号同一行；
- 长参数列表每项独立一行；
- 多行声明推荐使用尾随逗号；
- 单个 lambda 参数优先放在圆括号外。

示例：

```kotlin
val client = HttpClient(
    serverAddress = serverAddress,
    timeout = timeout,
    enableTls = true,
)
```

Lambda：

```kotlin
users
    .filter { it.isActive }
    .map { it.userName }
```

## 9.10 Kotlin 示例

```kotlin
package com.example.network

import kotlin.time.Duration
import kotlin.time.Duration.Companion.seconds

private const val MAX_RETRY_COUNT = 3

class HttpClient(
    private val serverAddress: String,
    private val timeout: Duration = 5.seconds,
) {
    var isConnected: Boolean = false
        private set

    fun connect(): Boolean {
        if (serverAddress.isBlank()) {
            return false
        }

        isConnected = connectWithRetry(MAX_RETRY_COUNT)
        return isConnected
    }

    private fun connectWithRetry(maxRetryCount: Int): Boolean {
        // Implementation omitted.
        return true
    }
}
```

---

# 10. Dart 编码与命名规范

## 10.1 Dart 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 包 | `lowercase_with_underscores` | `network_client` |
| 目录 | `lowercase_with_underscores` | `network_utils/` |
| 文件 | `lowercase_with_underscores` | `http_client.dart` |
| 类 | `UpperCamelCase` | `HttpClient` |
| 枚举类型 | `UpperCamelCase` | `ConnectionState` |
| 扩展 | `UpperCamelCase` | `IterableExtensions` |
| 类型别名 | `UpperCamelCase` | `UserPredicate` |
| 类型参数 | `UpperCamelCase` | `T`, `Key`, `Value` |
| 函数 | `lowerCamelCase` | `calculateTotal()` |
| 方法 | `lowerCamelCase` | `connectServer()` |
| 变量 | `lowerCamelCase` | `retryCount` |
| 参数 | `lowerCamelCase` | `requestTimeout` |
| 常量 | `lowerCamelCase` | `maxRetryCount` |
| 私有标识符 | 前导 `_` | `_connectWithRetry()` |
| 枚举值 | `lowerCamelCase` | `connected` |
| 测试文件 | `lowercase_with_underscores_test.dart` | `http_client_test.dart` |

## 10.2 类型和文件

推荐：

```dart
class HttpClient {}
enum ConnectionState { disconnected, connected }
typedef UserPredicate = bool Function(User user);
```

文件：

```text
http_client.dart
user_repository.dart
connection_state.dart
```

禁止：

```text
HttpClient.dart
http-client.dart
HTTP_CLIENT.dart
```

## 10.3 普通标识符

函数、方法、变量、参数和常量使用 `lowerCamelCase`：

```dart
const maxRetryCount = 3;
final requestTimeout = Duration(seconds: 5);

bool canRetry() => true;
```

Dart 新代码不默认使用：

```dart
const MAX_RETRY_COUNT = 3;
```

只有与既有代码保持一致或生成跨语言代码时，才考虑继续使用全大写常量。

## 10.4 私有成员

Dart 使用库级私有，前导下划线表示私有：

```dart
class HttpClient {
  bool _isConnected = false;

  bool _connectWithRetry() {
    return true;
  }
}
```

禁止给非私有标识符添加无意义前导下划线。

## 10.5 缩写

长度超过两个字符的缩写按普通单词处理：

推荐：

```dart
class HttpClient {}
class JsonParser {}
final userId = 1;
```

不推荐：

```dart
class HTTPClient {}
class JSONParser {}
final userID = 1;
```

## 10.6 Import 顺序

顺序：

1. `dart:`；
2. `package:`；
3. 相对导入；
4. `export` 单独分组。

示例：

```dart
import 'dart:async';

import 'package:http/http.dart' as http;

import 'connection_state.dart';
import 'request_options.dart';
```

同组按字母排序，交给工具处理。

## 10.7 格式

必须：

- 使用 `dart format`；
- 不手工与格式化器对抗；
- 推荐每行不超过 80 个字符；
- 所有流程控制语句使用大括号；
- 使用尾随逗号帮助格式化器形成稳定多行布局。

推荐：

```dart
final client = HttpClient(
  serverAddress: serverAddress,
  timeout: const Duration(seconds: 5),
  enableTls: true,
);
```

## 10.8 空安全和异步

必须：

- 使用非空类型作为默认；
- 只在确实允许缺失时使用 `?`；
- 避免无必要的 `!`；
- 异步函数返回 `Future<T>`；
- 不忽略未等待的 Future，除非有明确意图并使用相应工具标记。

推荐：

```dart
Future<User> loadUser(String userId) async {
  final user = await repository.findUser(userId);
  if (user == null) {
    throw UserNotFoundException(userId);
  }
  return user;
}
```

## 10.9 Dart 示例

```dart
const maxRetryCount = 3;

class HttpClient {
  HttpClient({
    required this.serverAddress,
    this.timeout = const Duration(seconds: 5),
  });

  final String serverAddress;
  final Duration timeout;

  bool _isConnected = false;

  bool get isConnected => _isConnected;

  Future<bool> connect() async {
    if (serverAddress.isEmpty) {
      return false;
    }

    _isConnected = await _connectWithRetry(maxRetryCount);
    return _isConnected;
  }

  Future<bool> _connectWithRetry(int retryCount) async {
    // Implementation omitted.
    return true;
  }
}
```

---

# 11. Go 编码与命名规范

## 11.1 Go 核心原则

Go 的格式应由工具决定，而不是由团队手写大量排版规则。

必须：

```bash
gofmt -w .
```

推荐：

```bash
goimports -w .
```

不得通过手工空格和对齐绕过 `gofmt`。

## 11.2 Go 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 包 | 小写、简短、通常单个单词 | `http`, `bytes`, `user` |
| 文件 | 小写、简洁 | `client.go`, `http_client.go` |
| 测试文件 | `_test.go` | `client_test.go` |
| 导出类型 | `MixedCaps`，首字母大写 | `HTTPClient` |
| 未导出类型 | `mixedCaps`，首字母小写 | `httpClient` |
| 导出函数 | 首字母大写 | `LoadUser` |
| 未导出函数 | 首字母小写 | `loadUser` |
| 变量 | `mixedCaps` | `retryCount` |
| 常量 | `MixedCaps` | `MaxRetryCount` |
| 接口 | 能力名，常见 `-er` | `Reader`, `Writer` |
| 缩写 | 保持统一大写 | `HTTPServer`, `userID` |

## 11.3 导出性

Go 标识符是否导出由首字母大小写决定：

```go
type Client struct{}     // 导出
func LoadUser() {}       // 导出

type client struct{}     // 未导出
func loadUser() {}       // 未导出
```

不得为了“看起来统一”随意改变首字母大小写，因为这会改变 API 可见性。

## 11.4 包名

推荐：

```go
package http
package user
package image
```

要求：

- 小写；
- 简短；
- 通常为单个单词；
- 不使用 `snake_case`；
- 不使用 `mixedCaps`；
- 不重复父级路径信息；
- 不使用 `util`、`common`、`misc` 作为无边界杂物包。

不推荐：

```go
package http_client
package httpClient
package common
package utils
```

## 11.5 Getter 和 Setter

Go Getter 不使用 `Get` 前缀：

推荐：

```go
func (c *Client) Owner() string {
    return c.owner
}

func (c *Client) SetOwner(owner string) {
    c.owner = owner
}
```

不推荐：

```go
func (c *Client) GetOwner() string {
    return c.owner
}
```

## 11.6 接口

单方法接口通常使用方法名加 `-er`：

```go
type Reader interface {
    Read(p []byte) (int, error)
}

type Stringer interface {
    String() string
}
```

接口应由使用方定义，而不是为了每个实现类型提前创建接口。

避免：

```go
type UserServiceInterface interface {
    // ...
}
```

推荐：

```go
type UserLoader interface {
    LoadUser(ctx context.Context, id string) (User, error)
}
```

## 11.7 缩写

Go 中常见首字母缩写保持一致大小写：

推荐：

```go
HTTPClient
URLParser
userID
apiURL
serveHTTP
```

不推荐：

```go
HttpClient
UrlParser
userId
apiUrl
serveHttp
```

同一名称中的缩写应保持一致。

## 11.8 变量名称

局部变量可简短，但必须清晰：

```go
for i, user := range users {
    // ...
}

ctx := context.Background()
req, err := http.NewRequestWithContext(ctx, method, url, body)
```

不要把 Java 风格长名称机械移植到 Go：

不推荐：

```go
numberOfUsersInCurrentRequest := len(users)
```

可改为：

```go
userCount := len(users)
```

## 11.9 错误

必须：

- 错误值放在最后一个返回值；
- 错误变量通常命名为 `err`；
- 立即检查错误；
- 添加上下文时使用 `%w` 保留错误链；
- 错误字符串小写开头，通常不以句号结尾；
- 不用 panic 处理普通可恢复错误。

推荐：

```go
user, err := repository.LoadUser(ctx, userID)
if err != nil {
    return User{}, fmt.Errorf("load user %q: %w", userID, err)
}
```

## 11.10 Context

推荐：

- `context.Context` 通常是第一个参数；
- 参数名使用 `ctx`；
- 不把 Context 存入结构体，除非 API 设计有明确理由；
- 不传 `nil` Context。

```go
func LoadUser(
    ctx context.Context,
    userID string,
) (User, error) {
    // ...
}
```

## 11.11 注释

每个导出名称都应有文档注释，注释以被描述名称开头：

```go
// Client sends requests to a remote service.
type Client struct {
    // ...
}

// LoadUser loads a user by ID.
func LoadUser(ctx context.Context, userID string) (User, error) {
    // ...
}
```

每个包应有包注释：

```go
// Package network provides HTTP networking helpers.
package network
```

## 11.12 行宽

Go 没有官方硬性行宽限制。

要求：

- 不为了满足人为行宽制造奇怪换行；
- 行过长影响阅读时，应重构表达式、拆分变量或函数；
- 最终格式以 `gofmt` 为准。

## 11.13 Go 示例

```go
// Package network provides HTTP client functionality.
package network

import (
    "context"
    "errors"
    "fmt"
    "time"
)

const maxRetryCount = 3

// Client connects to a remote HTTP service.
type Client struct {
    serverAddress string
    timeout       time.Duration
    connected     bool
}

// NewClient creates a Client.
func NewClient(serverAddress string, timeout time.Duration) *Client {
    return &Client{
        serverAddress: serverAddress,
        timeout:       timeout,
    }
}

// Connect connects the client to the remote service.
func (c *Client) Connect(ctx context.Context) error {
    if c.serverAddress == "" {
        return errors.New("server address is empty")
    }

    if err := c.connectWithRetry(ctx, maxRetryCount); err != nil {
        return fmt.Errorf("connect to %q: %w", c.serverAddress, err)
    }

    c.connected = true
    return nil
}

// IsConnected reports whether the client is connected.
func (c *Client) IsConnected() bool {
    return c.connected
}

func (c *Client) connectWithRetry(
    ctx context.Context,
    retryCount int,
) error {
    // Implementation omitted.
    return nil
}
```

---

# 12. Python 编码与命名规范

## 12.1 Python 命名表

| 对象 | 规则 | 示例 |
|---|---|---|
| 模块 | `lower_snake_case.py` | `http_client.py` |
| 包 | 短小全小写，尽量无下划线 | `network` |
| 类 | `CapWords` / `PascalCase` | `HttpClient` |
| 异常 | `PascalCase + Error` | `ConnectionError` |
| 函数 | `lower_snake_case` | `calculate_total()` |
| 方法 | `lower_snake_case` | `connect_server()` |
| 变量 | `lower_snake_case` | `retry_count` |
| 参数 | `lower_snake_case` | `request_timeout` |
| 常量 | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| 非公开标识符 | 前导 `_` | `_connect_with_retry()` |
| 类型变量 | `PascalCase` 或约定短名 | `T`, `KeyT` |
| 测试文件 | `test_*.py` | `test_http_client.py` |
| 测试函数 | `test_*` | `test_connect_retries()` |

## 12.2 模块和包

推荐：

```text
network/
    __init__.py
    http_client.py
    connection_state.py
```

要求：

- 模块名短小、全小写；
- 下划线可以提高模块可读性；
- 包名也应短小、全小写，尽量避免下划线；
- 不使用连字符；
- 不使用大写文件名。

## 12.3 类和异常

推荐：

```python
class HttpClient:
    pass


class UserRepository:
    pass


class UserNotFoundError(Exception):
    pass
```

异常类使用 `Error` 后缀，除非已有惯例要求其他命名。

## 12.4 函数、方法和变量

推荐：

```python
def load_user(user_id: str) -> User:
    ...


retry_count = 0
request_timeout = 5.0
```

布尔值：

```python
is_ready = True
has_permission = False
can_retry = retry_count < MAX_RETRY_COUNT
```

## 12.5 常量

模块级常量：

```python
MAX_RETRY_COUNT = 3
DEFAULT_TIMEOUT_SECONDS = 5.0
```

Python 不会强制常量不可修改，大写命名表达“调用方不应重新赋值”的约定。

## 12.6 非公开成员

单前导下划线表示非公开实现细节：

```python
class HttpClient:
    def _connect_with_retry(self) -> bool:
        ...
```

双前导下划线会触发名称改写，不应把它当作普通 private 修饰符：

```python
class Base:
    def __internal_hook(self) -> None:
        ...
```

仅在防止子类意外覆盖等确有需要时使用。

不要自行创建双前后下划线名称：

```python
__custom__  # 禁止自造
```

双前后下划线名称保留给 Python 数据模型和语言协议。

## 12.7 缩进和行宽

必须：

- 4 个空格缩进；
- 不使用 Tab；
- 顶级函数和类之间两个空行；
- 类内方法之间一个空行；
- 使用自动格式化工具。

行宽选择：

- 严格 PEP 8：代码 79 字符，注释和文档字符串 72 字符；
- 使用 Black 或 Ruff formatter 的新项目：通常采用 88 字符；
- 团队可统一到不超过 99 字符，但必须配置到工具中；
- 不允许不同开发者使用不同宽度。

## 12.8 Import

顺序：

1. 标准库；
2. 第三方库；
3. 本地应用或项目模块。

分组之间空一行：

```python
from __future__ import annotations

import logging
from pathlib import Path

import httpx

from app.models import User
from app.repositories import UserRepository
```

要求：

- 通常一行一个导入；
- 避免通配符导入；
- 优先使用清晰的绝对导入；
- 显式相对导入可用于包内部；
- 删除未使用导入；
- 使用 Ruff、isort 或格式化器自动排序。

禁止：

```python
from module import *
```

## 12.9 类型注解

新项目推荐为公共接口添加类型注解：

```python
def load_user(user_id: str) -> User:
    ...
```

要求：

- 类型注解应提高可读性；
- 不重复写显而易见且无价值的复杂类型；
- 公共库 API、数据模型和跨模块边界优先完整标注；
- 使用 `mypy` 或 `pyright` 进行检查时，应固定配置；
- 避免无边界滥用 `Any`。

## 12.10 默认参数

禁止使用可变对象作为默认参数：

不推荐：

```python
def add_user(users: list[User] = []) -> None:
    users.append(User())
```

推荐：

```python
def add_user(users: list[User] | None = None) -> None:
    if users is None:
        users = []
```

## 12.11 异常

要求：

- 捕获具体异常；
- 不使用裸 `except:`；
- 不静默吞掉异常；
- 重新抛出时使用 `raise ... from ...` 保留因果链；
- 不用异常替代普通条件判断。

推荐：

```python
try:
    config = load_config(path)
except OSError as exc:
    raise ConfigurationError(
        f"failed to load configuration from {path}"
    ) from exc
```

## 12.12 文档字符串

公开模块、类、函数和方法应按项目要求添加 docstring：

```python
def load_user(user_id: str) -> User:
    """Load and return a user by ID.

    Raises:
        UserNotFoundError: If the user does not exist.
    """
```

项目应统一 Google、NumPy 或 Sphinx 风格，不应混用。

## 12.13 Python 示例

```python
from __future__ import annotations

from dataclasses import dataclass
from datetime import timedelta

MAX_RETRY_COUNT = 3


@dataclass(frozen=True)
class ConnectionOptions:
    server_address: str
    timeout: timedelta = timedelta(seconds=5)


class HttpClient:
    """Connects to a remote HTTP service."""

    def __init__(self, options: ConnectionOptions) -> None:
        self._options = options
        self._is_connected = False

    @property
    def is_connected(self) -> bool:
        return self._is_connected

    def connect(self) -> bool:
        if not self._options.server_address:
            return False

        self._is_connected = self._connect_with_retry(
            MAX_RETRY_COUNT
        )
        return self._is_connected

    def _connect_with_retry(self, retry_count: int) -> bool:
        """Try to connect up to the requested number of times."""
        return True
```

---

# 13. 注释与文档统一要求

## 13.1 注释原则

注释应说明：

- 为什么这样实现；
- 有什么业务约束；
- 存在哪些不明显的边界条件；
- 为什么没有采用看似更简单的实现；
- 外部系统或协议有什么特殊要求；
- 性能、安全、并发或兼容性原因。

不应说明：

- 每一行代码做了什么；
- 从代码本身就能直接读出的信息；
- 已经过期的实现细节；
- 没有上下文的个人猜测。

## 13.2 TODO

推荐：

```text
TODO(PROJ-125): Remove the compatibility path after V3 migration.
```

可接受：

```text
TODO(pamela): Replace this implementation after the new API is available.
```

不推荐：

```text
TODO: Fix later.
TODO: Maybe optimize.
```

TODO 应至少包含以下一项：

- 任务编号；
- 负责人；
- 删除条件；
- 明确的后续目标。

## 13.3 公共 API 文档

非显而易见的公共 API 应说明：

- 功能；
- 参数约束；
- 返回值；
- 可空性；
- 所有权和生命周期；
- 线程安全性；
- 错误和异常；
- 性能特征；
- 副作用。

各语言使用其原生文档系统：

| 语言 | 推荐文档形式 |
|---|---|
| C | Doxygen、项目自定义 API 注释、上游格式 |
| C++ | Doxygen、项目自定义 API 注释 |
| Java | Javadoc |
| Kotlin | KDoc |
| Dart | `///` 文档注释 |
| Go | Go doc comment |
| Python | Docstring |

---

# 14. Git、提交与代码评审建议

## 14.1 一次提交只处理一个逻辑主题

推荐：

```text
feat(network): add request timeout support
fix(parser): reject malformed headers
refactor(user): extract repository interface
```

不推荐把以下内容混在一个提交：

- 功能开发；
- 全仓库格式化；
- 文件重命名；
- 大规模无关重构；
- 依赖升级。

## 14.2 不提交无意义格式噪声

禁止：

- 修改行尾但没有功能变化；
- 手工重排格式化器已经固定的代码；
- 在功能 PR 中重排整个文件；
- 修改第三方生成代码。

## 14.3 生成代码

生成代码应：

- 标记为生成文件；
- 不手工修改；
- 由生成器或模板修改；
- 根据仓库策略决定是否提交；
- 从普通 lint 规则中按需排除。

常见标记：

```text
DO NOT EDIT
Generated code
Code generated ... DO NOT EDIT.
```

---

# 15. 自动格式化与静态检查

## 15.1 工具总表

| 语言 | 格式化 | Lint / 静态检查 | 类型 / 深度检查 |
|---|---|---|---|
| C | `clang-format` | `clang-tidy`, compiler warnings | ASan, UBSan, static analyzer |
| C++ | `clang-format` | `clang-tidy`, compiler warnings | ASan, UBSan, TSan |
| Java | `google-java-format`, Spotless | Checkstyle, Error Prone, PMD | SpotBugs |
| Kotlin | IntelliJ formatter, `ktfmt` | `ktlint`, Detekt | Kotlin compiler |
| Dart | `dart format` | `dart analyze`, lints | Dart analyzer |
| Go | `gofmt`, `goimports` | `go vet`, Staticcheck | race detector |
| Python | Ruff formatter 或 Black | Ruff, Pylint | mypy 或 pyright |

> 工具选择可按项目调整，但每种语言应有唯一的默认格式化入口。

## 15.2 C/C++ 建议

项目根目录提交：

```text
.clang-format
.clang-tidy
.editorconfig
```

GCC/Clang 基础警告建议：

```bash
-Wall
-Wextra
-Wpedantic
-Wshadow
```

按项目风险评估启用：

```bash
-Wconversion
-Wsign-conversion
-Werror
```

说明：

- `-Werror` 更适合 CI 或受控编译器版本；
- 跨多编译器、多平台项目需谨慎处理警告差异；
- 第三方代码应单独隔离，不应降低整个项目警告等级。

## 15.3 Java 建议

推荐统一入口：

```bash
./gradlew spotlessCheck
./gradlew test
```

或：

```bash
mvn verify
```

要求：

- 格式化器版本固定；
- Checkstyle 规则文件提交到仓库；
- CI 中执行格式检查、静态检查和测试。

## 15.4 Kotlin 建议

推荐：

```bash
./gradlew ktlintCheck
./gradlew detekt
./gradlew test
```

使用 IntelliJ IDEA 或 Android Studio 时，将 Kotlin 官方风格设置为项目级配置。

## 15.5 Dart 建议

推荐：

```bash
dart format --output=none --set-exit-if-changed .
dart analyze
dart test
```

Flutter 项目：

```bash
flutter analyze
flutter test
```

## 15.6 Go 建议

推荐：

```bash
gofmt -w .
go vet ./...
go test ./...
```

可增加：

```bash
staticcheck ./...
go test -race ./...
```

CI 应检查工作区经过 `gofmt` 后没有差异。

## 15.7 Python 建议

使用 Ruff：

```bash
ruff format --check .
ruff check .
pytest
```

使用 Black：

```bash
black --check .
ruff check .
pytest
```

类型检查：

```bash
mypy src
```

或：

```bash
pyright
```

项目应在 `pyproject.toml` 中固定：

- Python 版本；
- 行宽；
- lint 规则；
- 格式化规则；
- 类型检查严格度；
- 测试路径。

---

# 16. Code Review 通用检查清单

## 16.1 命名

- [ ] 名称能够表达业务用途
- [ ] 未使用拼音标识符
- [ ] 未使用难以理解的缩写
- [ ] 未使用无意义的 `data`、`obj`、`flag`、`temp`
- [ ] 布尔名称能自然表达真假
- [ ] 集合名称使用合适的复数形式
- [ ] 数值单位明确
- [ ] 没有混用当前语言不常见的命名风格
- [ ] 公共 API 名称稳定、清晰且不泄漏实现细节

## 16.2 结构

- [ ] 一个函数只有一个主要职责
- [ ] 没有过深嵌套
- [ ] 没有过长参数列表
- [ ] 重复逻辑已适当抽取
- [ ] 没有建立无边界的 `Utils`、`Common`、`Helper`
- [ ] 模块和包职责清晰
- [ ] 公共 API 最小化

## 16.3 安全与正确性

- [ ] 输入参数已校验
- [ ] 错误没有被静默忽略
- [ ] 资源能够在所有路径正确释放
- [ ] 空值、空指针和可空类型处理明确
- [ ] 并发代码说明线程安全假设
- [ ] 没有新增不必要的全局可变状态
- [ ] 没有硬编码密钥、令牌或敏感信息

## 16.4 可维护性

- [ ] 注释说明原因而不是重复代码
- [ ] TODO 有任务号、负责人或删除条件
- [ ] 没有长期保留注释掉的旧代码
- [ ] 公共 API 有必要文档
- [ ] 测试覆盖主要行为和边界条件
- [ ] 变更没有夹带无关格式化

## 16.5 工具

- [ ] 已运行对应语言格式化器
- [ ] 格式化后工作区无额外变更
- [ ] Lint 和静态检查通过
- [ ] 编译无新增警告
- [ ] 单元测试通过
- [ ] 必要时通过集成测试
- [ ] CI 检查通过

---

# 17. 官方与主要参考资料

以下资料用于形成本文档。各项目在实际执行时，应继续以对应上游仓库的最新版本为准。

## 17.1 C

1. Linux kernel coding style<br>
   <https://docs.kernel.org/process/coding-style.html>

2. PEP 7 – Style Guide for C Code<br>
   <https://peps.python.org/pep-0007/>

> Linux 内核规则是内核仓库专用规则，不能无条件视为所有 C 项目的通用格式。

## 17.2 C++

1. C++ Core Guidelines<br>
   <https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines>

2. Google C++ Style Guide<br>
   <https://google.github.io/styleguide/cppguide.html>

3. LLVM Coding Standards<br>
   <https://llvm.org/docs/CodingStandards.html>

## 17.3 Java

1. Google Java Style Guide<br>
   <https://google.github.io/styleguide/javaguide.html>

2. Java Language Specification – Names<br>
   <https://docs.oracle.com/javase/specs/jls/se17/html/jls-6.html>

## 17.4 Kotlin

1. Kotlin Coding Conventions<br>
   <https://kotlinlang.org/docs/coding-conventions.html>

## 17.5 Dart

1. Effective Dart: Style<br>
   <https://dart.dev/effective-dart/style>

2. Effective Dart<br>
   <https://dart.dev/effective-dart>

## 17.6 Go

1. Effective Go<br>
   <https://go.dev/doc/effective_go>

2. Go Code Review Comments<br>
   <https://go.dev/wiki/CodeReviewComments>

3. Go Doc Comments<br>
   <https://go.dev/doc/comment>

4. Google Go Style Guide<br>
   <https://google.github.io/styleguide/go/>

## 17.7 Python

1. PEP 8 – Style Guide for Python Code<br>
   <https://peps.python.org/pep-0008/>

2. PEP 257 – Docstring Conventions<br>
   <https://peps.python.org/pep-0257/>

3. Google Python Style Guide<br>
   <https://google.github.io/styleguide/pyguide.html>

---

# 附录 A：推荐的新项目默认策略

当团队没有任何既有规范时，可采用以下默认值。

| 语言 | 默认格式化 | 默认行宽 | 默认命名核心 |
|---|---|---:|---|
| C | `clang-format` | 100 | `lower_snake_case` + 模块前缀 |
| C++ | `clang-format` | 100 | 类型/函数 PascalCase，变量 snake_case |
| Java | `google-java-format` | 100 | 类型 UpperCamel，成员 lowerCamel |
| Kotlin | Kotlin 官方格式 | 120 | 类型 UpperCamel，成员 lowerCamel |
| Dart | `dart format` | 80 | 类型 UpperCamel，其他 lowerCamel |
| Go | `gofmt` | 无硬限制 | MixedCaps，大小写决定导出 |
| Python | Ruff format 或 Black | 88 | 类型 PascalCase，函数/变量 snake_case |

---

# 附录 B：最终执行原则

```text
项目规范优先于个人偏好。
上游仓库规范优先于本文档。
自动格式化优先于人工排版争论。
语言原生习惯优先于跨语言强行统一。
可读性优先于少写几个字符。
公共 API 稳定性优先于风格清理。
一致性优先，但不应以破坏兼容性为代价。
```
