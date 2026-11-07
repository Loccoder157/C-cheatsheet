# CHEAT SHEET C — TỔNG HỢP CHƯƠNG 1–8 + ĐỆ QUY

> Phong cách trình bày theo C90 để dễ biên dịch trên Dev-C++/MinGW đời cũ: **khai báo biến ở đầu khối `{}`**, khai báo `int i;` trước vòng `for`, dùng `int main()` và ưu tiên chú thích `/* ... */`.
>
> Lưu file với đuôi `.c` và biên dịch ở chế độ C, không phải C++. Các ví dụ tập trung vào cú pháp và kiến thức trong 8 chương bài giảng; những ghi chú bổ sung được đánh dấu riêng khi cần.

## 0. Header thường dùng

```c
#include <stdio.h>   /* printf, scanf, gets, puts */
#include <string.h>  /* hàm xử lý chuỗi và vùng nhớ */
#include <ctype.h>   /* kiểm tra và chuyển đổi ký tự */
#include <stdlib.h>  /* malloc, calloc, realloc, free */
#include <stddef.h>  /* size_t, NULL */
```

## 0.1. Các hàm `ctype.h` — kiểm tra và chuyển đổi ký tự

Thêm header:

```c
#include <ctype.h>
```

### Nhóm hàm kiểm tra ký tự

| Hàm | Trả về khác 0 khi | Ví dụ nhận diện |
|---|---|---|
| `isalpha(c)` | `c` là chữ cái | `A`, `z` |
| `isdigit(c)` | `c` là chữ số thập phân | `0` đến `9` |
| `isalnum(c)` | `c` là chữ cái hoặc chữ số | `A`, `7` |
| `islower(c)` | `c` là chữ thường | `a` đến `z` |
| `isupper(c)` | `c` là chữ hoa | `A` đến `Z` |
| `isspace(c)` | `c` là ký tự trắng | dấu cách, `\n`, `\t`, `\r` |
| `ispunct(c)` | `c` là dấu câu | `!`, `,`, `.` |
| `isxdigit(c)` | `c` là chữ số hệ 16 | `0`-`9`, `a`-`f`, `A`-`F` |
| `iscntrl(c)` | `c` là ký tự điều khiển | `\n`, `\t` |
| `isprint(c)` | `c` là ký tự in được, kể cả dấu cách | `A`, ` ` |
| `isgraph(c)` | `c` là ký tự in được, không tính dấu cách | `A`, `!` |

### Nhóm hàm chuyển đổi

| Hàm | Công dụng | Ví dụ |
|---|---|---|
| `toupper(c)` | Chuyển chữ thường thành chữ hoa | `toupper('a')` trả về `'A'` |
| `tolower(c)` | Chuyển chữ hoa thành chữ thường | `tolower('A')` trả về `'a'` |

Các hàm kiểm tra trả về **0 nếu sai**, khác 0 nếu đúng. `toupper` và `tolower` trả về ký tự sau chuyển đổi; nếu không chuyển được thì giữ nguyên ký tự.

### Ví dụ: đếm chữ cái, chữ số, khoảng trắng và đổi chữ thường thành chữ hoa

```c
#include <stdio.h>
#include <ctype.h>

int main()
{
    char s[100];
    int i;
    int chu_cai;
    int chu_so;
    int khoang_trang;

    chu_cai = 0;
    chu_so = 0;
    khoang_trang = 0;

    printf("Nhap chuoi: ");
    if (gets(s) != NULL)
    {
        for (i = 0; s[i] != '\0'; i++)
        {
            if (isalpha((unsigned char)s[i]))
                chu_cai++;
            if (isdigit((unsigned char)s[i]))
                chu_so++;
            if (isspace((unsigned char)s[i]))
                khoang_trang++;

            s[i] = (char)toupper((unsigned char)s[i]);
        }

        printf("Chuoi in hoa: %s", s);
        printf("\nSo chu cai: %d", chu_cai);
        printf("\nSo chu so: %d", chu_so);
        printf("\nSo ky tu trang: %d\n", khoang_trang);
    }

    return 0;
}
```

**Lưu ý quan trọng:** đối số truyền vào các hàm `ctype.h` phải là `EOF` hoặc giá trị biểu diễn được bởi `unsigned char`. Vì `char` có thể mang giá trị âm, nên cách viết an toàn là `isalpha((unsigned char)c)`, `toupper((unsigned char)c)`, v.v. Các hàm này xử lý ký tự theo locale; không dùng trực tiếp để xử lý chữ tiếng Việt UTF-8 nhiều byte như thể mỗi byte là một ký tự.

`isblank(c)` là hàm được bổ sung từ C99, vì vậy bỏ qua hàm này khi cần tương thích nghiêm ngặt với C90 hoặc trình biên dịch rất cũ.

---


---

# 1. TỔNG QUAN NGÔN NGỮ C & CẤU TRÚC CHƯƠNG TRÌNH

## 1.1. Một số khái niệm

- **Chương trình**: tập hợp câu lệnh hướng dẫn máy tính thực hiện công việc.
- **Ngôn ngữ lập trình**: ngôn ngữ dùng để mô tả thuật toán và viết chương trình.
- **Thuật toán**: tập hữu hạn các bước rõ ràng, có thứ tự, giải quyết một bài toán.
- Có thể biểu diễn thuật toán bằng: ngôn ngữ tự nhiên, lưu đồ giải thuật hoặc mã giả.
- **Biên dịch**: dịch chương trình nguồn thành chương trình đích trước khi chạy. C là ngôn ngữ biên dịch.

## 1.2. Cấu trúc chương trình C

```c
#include <stdio.h>

/* Khai bao ham */
void xin_chao();

int main()
{
    /* Khai bao bien o dau khoi */
    int n;

    n = 10;
    printf("Gia tri n = %d\n", n);
    xin_chao();

    return 0;
}

/* Dinh nghia ham */
void xin_chao()
{
    printf("Xin chao C!\n");
}
```

**Thành phần chính:**

| Thành phần | Công dụng |
|---|---|
| `#include <stdio.h>` | Nạp khai báo hàm nhập/xuất chuẩn |
| `#define TEN gia_tri` | Định nghĩa macro/hằng ký hiệu |
| `const int x = 10;` | Khai báo đối tượng không cho sửa qua tên `x` |
| `int main()` | Hàm bắt đầu thực thi chương trình |
| `{ ... }` | Khối lệnh |
| `;` | Kết thúc một câu lệnh |
| `/* ... */` | Chú thích nhiều dòng hoặc một dòng |
| `return 0;` | Kết thúc `main`, báo chạy thành công theo quy ước |

## 1.3. Từ khóa và quy tắc đặt tên

Một số từ khóa thường gặp: `int`, `char`, `float`, `double`, `void`, `if`, `else`, `switch`, `case`, `for`, `while`, `do`, `break`, `continue`, `return`, `struct`, `union`, `enum`, `typedef`, `const`, `static`, `extern`, `sizeof`, `goto`.

Quy tắc tên định danh:

- Có thể gồm chữ cái, chữ số và dấu gạch dưới `_`.
- Không được bắt đầu bằng chữ số.
- Không chứa dấu cách hoặc ký tự đặc biệt như `-`, `@`.
- Không được trùng từ khóa.
- C phân biệt chữ hoa và chữ thường: `tong`, `Tong`, `TONG` là các tên khác nhau.

## 1.4. Các kiểu dữ liệu cơ bản

| Kiểu | Ý nghĩa | Ví dụ khai báo | Định dạng thường dùng |
|---|---|---|---|
| `char` | Ký tự / số nguyên nhỏ | `char c;` | `%c` |
| `int` | Số nguyên | `int n;` | `%d` hoặc `%i` |
| `short` | Số nguyên ngắn | `short x;` | `%hd` |
| `long` | Số nguyên dài | `long x;` | `%ld` |
| `unsigned int` | Số nguyên không dấu | `unsigned int x;` | `%u` |
| `float` | Số thực độ chính xác đơn | `float x;` | `%f` |
| `double` | Số thực độ chính xác kép | `double x;` | `printf: %f`, `scanf: %lf` |
| `long double` | Số thực độ chính xác mở rộng | `long double x;` | `%Lf` |
| `void` | Không có giá trị / kiểu tổng quát trong một số ngữ cảnh | `void ham();` | Không có định dạng |

**Lưu ý:** kích thước byte và miền giá trị phụ thuộc trình biên dịch, hệ điều hành và kiến trúc. Dùng `sizeof(kieu)` để kiểm tra; không mặc định `int` luôn có cùng kích thước trên mọi máy.

## 1.5. Khai báo biến, hằng và ép kiểu

```c
int a, b;
float diem;
char ky_tu;
const float PI = 3.14159f;
#define MAX 100

/* Gan gia tri */
a = 5;
b = 2;
diem = (float)a / b;  /* 2.5, tranh chia nguyen */
```

- **Biến**: giá trị có thể thay đổi khi chương trình chạy.
- **Hằng**: giá trị không thay đổi trong quá trình chạy.
- **Ép kiểu**: `(float)a`, `(int)x`, `(double)n`.
- `a / b` với `a`, `b` kiểu nguyên thực hiện chia nguyên; ép kiểu ít nhất một toán hạng để có kết quả thực.
- `sizeof(a)` trả về số byte của đối tượng/kiểu dữ liệu, kiểu kết quả là `size_t`.

## 1.6. Các ký tự điều khiển thường gặp

| Ký hiệu | Ý nghĩa |
|---|---|
| `\n` | Xuống dòng |
| `\t` | Tab ngang |
| `\b` | Backspace |
| `\0` | Ký tự NULL kết thúc chuỗi |
| `\r` | Về đầu dòng |
| `\\` | In dấu `\\` |
| `\'` | In dấu nháy đơn |
| `\"` | In dấu nháy kép |

---

# 2. KIỂU DỮ LIỆU & PHÉP TOÁN

## 2.1. Toán tử số học

| Toán tử | Ý nghĩa | Ví dụ |
|---|---|---|
| `+` | Cộng | `a + b` |
| `-` | Trừ | `a - b` |
| `*` | Nhân | `a * b` |
| `/` | Chia | `a / b` |
| `%` | Chia lấy dư, chỉ áp dụng cho kiểu nguyên | `a % b` |
| `++` | Tăng 1 | `i++`, `++i` |
| `--` | Giảm 1 | `i--`, `--i` |

**Phân biệt tiền tố và hậu tố:**

```c
int i, a, b;
i = 3;
a = ++i;   /* i tang truoc: i = 4, a = 4 */
i = 3;
b = i++;   /* gan truoc: b = 3, sau do i = 4 */
```

## 2.2. Toán tử quan hệ và logic

| Nhóm | Toán tử | Ý nghĩa |
|---|---|---|
| Quan hệ | `<`, `>`, `<=`, `>=` | So sánh nhỏ/lớn |
| Bằng/khác | `==`, `!=` | So sánh bằng hoặc khác |
| Logic | `&&` | AND: cả hai điều kiện đúng |
| Logic | `||` | OR: ít nhất một điều kiện đúng |
| Logic | `!` | NOT: phủ định điều kiện |

Trong C, `0` được xem là sai (false), mọi giá trị khác `0` được xem là đúng (true). Kết quả của toán tử so sánh và logic là `0` hoặc `1`.

```c
if (n >= 0 && n % 2 == 0)
    printf("So chan khong am");
```

## 2.3. Toán tử gán và toán tử khác

| Toán tử | Tương đương |
|---|---|
| `a += b` | `a = a + b` |
| `a -= b` | `a = a - b` |
| `a *= b` | `a = a * b` |
| `a /= b` | `a = a / b` |
| `a %= b` | `a = a % b` |
| `dieu_kien ? x : y` | Nếu điều kiện đúng lấy `x`, sai lấy `y` |
| `,` | Tính các biểu thức từ trái qua phải, lấy kết quả biểu thức cuối |

```c
int max, a, b;
max = (a > b) ? a : b;
```

## 2.4. Thứ tự ưu tiên thường gặp (cao xuống thấp)

1. `()` — ngoặc biểu thức.
2. Toán tử một ngôi: `!`, `++`, `--`, `+`, `-`, `*` (giải tham chiếu), `&` (lấy địa chỉ), `sizeof`, ép kiểu.
3. `*`, `/`, `%`.
4. `+`, `-`.
5. `<`, `<=`, `>`, `>=`.
6. `==`, `!=`.
7. `&&`.
8. `||`.
9. `?:`.
10. Các phép gán `=`, `+=`, `-=`, ...
11. `,`.

Dùng ngoặc `()` khi muốn biểu thức rõ ràng, tránh phụ thuộc vào việc nhớ thứ tự ưu tiên.

## 2.5. Một số hàm toán học (`math.h`, `stdlib.h`)

Khai báo `#include <math.h>` khi dùng các hàm toán học.

| Hàm | Công dụng | Ví dụ |
|---|---|---|
| `sqrt(x)` | Căn bậc hai | `sqrt(25.0)` → `5.0` |
| `pow(x, y)` | Lũy thừa \(x^y\) | `pow(2.0, 3.0)` → `8.0` |
| `fabs(x)` | Giá trị tuyệt đối số thực | `fabs(-2.5)` → `2.5` |
| `abs(n)` | Giá trị tuyệt đối số nguyên | `abs(-5)` → `5` (`<stdlib.h>`) |

Ví dụ tính căn bậc hai: `can = sqrt((double)n);`.

## 2.6. Nhập và xuất dữ liệu (`stdio.h`)

### `printf()` — xuất dữ liệu

```c
printf("n = %d", n);
printf("x = %.2f", x);       /* 2 chu so thap phan */
printf("c = %c, s = %s", c, s);
printf("Dia chi = %p", (void *)p);
```

### `scanf()` — nhập dữ liệu

```c
int n;
float x;
double y;
char c;
char s[50];

scanf("%d", &n);
scanf("%f", &x);
scanf("%lf", &y);
scanf(" %c", &c);     /* co khoang trang truoc %c de bo qua newline */
scanf("%49s", s);     /* khong dung & voi mang char */
```

| Định dạng | Kiểu dữ liệu thường dùng | Ghi chú |
|---|---|---|
| `%d`, `%i` | `int` | Số nguyên |
| `%u` | `unsigned int` | Số nguyên không dấu |
| `%hd` | `short` | Số nguyên ngắn |
| `%ld` | `long` | Số nguyên dài |
| `%f` | `float` trong `scanf`; `float`/`double` trong `printf` | Số thực |
| `%lf` | `double` trong `scanf` | Số thực kép |
| `%Lf` | `long double` | Số thực mở rộng |
| `%c` | `char` | Một ký tự |
| `%s` | Mảng `char` | Chuỗi đến khoảng trắng |
| `%x`, `%X` | Số nguyên | Hệ 16 |
| `%o` | Số nguyên | Hệ 8 |
| `%e`, `%E` | Số thực | Dạng khoa học |
| `%p` | Con trỏ | In địa chỉ |

**Lỗi dễ gặp:** `scanf("%d", n)` sai; phải truyền địa chỉ `scanf("%d", &n)`. Riêng mảng ký tự khi nhập `%s` dùng `s`, không dùng `&s`.

### Nhập một dòng ký tự theo phong cách Dev-C++ cũ

```c
char hoten[80];
printf("Nhap ho ten: ");
gets(hoten);
printf("Ho ten: %s", hoten);
```

`gets()` được giữ trong cheat sheet theo cách học bạn yêu cầu, nhưng hàm này không giới hạn độ dài và có thể gây tràn bộ đệm; đã bị loại khỏi chuẩn C11. Chỉ dùng trong bài tập môi trường cũ, không dùng cho phần mềm thực tế. Với nhập liệu an toàn nên dùng `fgets()`.

---

# 3. CÁC LỆNH CÓ CẤU TRÚC

## 3.1. Rẽ nhánh `if`

### `if` không có `else`

```c
if (dieu_kien)
{
    /* Cong viec khi dieu kien dung */
}
```

### `if ... else`

```c
if (n % 2 == 0)
{
    printf("So chan");
}
else
{
    printf("So le");
}
```

### `if` lồng nhau / `else if`

```c
if (diem >= 8.0)
    printf("Gioi");
else if (diem >= 6.5)
    printf("Kha");
else if (diem >= 5.0)
    printf("Trung binh");
else
    printf("Yeu");
```

## 3.2. Lệnh `switch ... case`

```c
switch (lua_chon)
{
    case 1:
        printf("Cong");
        break;
    case 2:
        printf("Tru");
        break;
    default:
        printf("Lua chon khong hop le");
}
```

- Biểu thức `switch` thường có kiểu nguyên hoặc `char`.
- `case` phải là hằng nguyên/kiểu ký tự.
- `break` thoát khỏi `switch`; thiếu `break` có thể chạy tiếp xuống `case` sau (fall-through).
- `default` là nhánh mặc định, không bắt buộc.

## 3.3. Vòng lặp `for`

```c
int i;
for (i = 1; i <= n; i++)
{
    printf("%d ", i);
}
```

Cấu trúc: `for (khoi_tao; dieu_kien; cap_nhat)`. Vòng lặp kiểm tra điều kiện trước mỗi lần chạy.

### `for` lồng nhau

```c
int i, j;
for (i = 0; i < so_dong; i++)
{
    for (j = 0; j < so_cot; j++)
    {
        printf("*");
    }
    printf("\n");
}
```

## 3.4. Vòng lặp `while`

```c
while (dieu_kien)
{
    /* Lap khi dieu kien con dung */
}
```

Kiểm tra điều kiện trước khi thực hiện thân vòng lặp; có thể không chạy lần nào.

## 3.5. Vòng lặp `do ... while`

```c
do
{
    printf("Nhap so duong: ");
    scanf("%d", &n);
} while (n <= 0);
```

Thực hiện thân vòng lặp trước rồi mới kiểm tra điều kiện; luôn chạy ít nhất một lần. Phải có dấu `;` sau `while (...)`.

## 3.6. So sánh ba vòng lặp

| Vòng lặp | Kiểm tra điều kiện | Số lần chạy tối thiểu | Thường dùng |
|---|---|---:|---|
| `for` | Trước thân vòng lặp | 0 | Biết trước số lần lặp |
| `while` | Trước thân vòng lặp | 0 | Lặp theo điều kiện |
| `do...while` | Sau thân vòng lặp | 1 | Menu, nhập lại đến khi hợp lệ |

## 3.7. Các lệnh hỗ trợ vòng lặp

| Lệnh | Công dụng |
|---|---|
| `break;` | Thoát khỏi vòng lặp gần nhất hoặc `switch` gần nhất |
| `continue;` | Bỏ phần còn lại của lượt hiện tại, chuyển sang lượt tiếp theo |
| `return;` | Kết thúc hàm không trả giá trị |
| `return x;` | Kết thúc hàm và trả giá trị `x` |
| `goto nhan;` | Nhảy đến nhãn trong cùng hàm; thường nên tránh nếu không cần thiết |

```c
int i;
for (i = 1; i <= 10; i++)
{
    if (i == 5)
        continue;  /* Bo qua so 5 */
    if (i == 8)
        break;     /* Dung vong lap tai 8 */
    printf("%d ", i);
}
```

### Cú pháp `goto` (biết để đọc mã cũ)

```c
int i;
i = 1;
lap_lai:
printf("%d ", i);
i++;
if (i <= 5)
    goto lap_lai;
```

Nên ưu tiên `for`, `while`, `do...while`; `goto` chỉ nên dùng khi thật cần thiết.

---

# 4. KIỂU MẢNG & THAO TÁC TRÊN MẢNG

## 4.1. Mảng một chiều

Mảng là dãy hữu hạn các phần tử **cùng kiểu**, lưu liên tiếp trong bộ nhớ. Chỉ số phần tử trong C bắt đầu từ `0`.

```c
int a[5];
int b[5] = {2, 4, 6, 8, 10};
float diem[] = {7.5f, 8.0f, 9.0f};
char ten[30];
```

- `a[0]`: phần tử đầu tiên.
- `a[n - 1]`: phần tử cuối cùng nếu mảng có `n` phần tử.
- Mảng khai báo `int a[100]` có chỉ số hợp lệ `0..99`.
- C không tự kiểm tra truy cập vượt biên; truy cập ngoài mảng gây hành vi không xác định.

### Nhập và xuất mảng

```c
#include <stdio.h>

int main()
{
    int a[100];
    int n;
    int i;

    printf("Nhap n: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        printf("a[%d] = ", i);
        scanf("%d", &a[i]);
    }

    for (i = 0; i < n; i++)
        printf("%d ", a[i]);

    return 0;
}
```

Trong ví dụ, phải đảm bảo `0 <= n <= 100` trước khi nhập.

### Một số thao tác cơ bản

```c
int i;
int tong;
int max;
int dem_chan;

tong = 0;
max = a[0];
dem_chan = 0;
for (i = 0; i < n; i++)
{
    tong += a[i];
    if (a[i] > max)
        max = a[i];
    if (a[i] % 2 == 0)
        dem_chan++;
}
```

## 4.2. Tìm kiếm tuyến tính

Duyệt từng phần tử cho đến khi tìm thấy giá trị cần tìm hoặc hết mảng.

```c
int tim_kiem(int a[], int n, int x)
{
    int i;

    for (i = 0; i < n; i++)
    {
        if (a[i] == x)
            return i;
    }
    return -1;
}
```

Trả về chỉ số tìm thấy; `-1` nghĩa là không có. Đây là mẫu bổ sung để thực hành thao tác tìm kiếm trên mảng.

## 4.3. Sắp xếp đổi chỗ trực tiếp (mẫu trong slide)

Tài liệu Chương 4 gọi ví dụ này là sắp xếp chọn. Cụ thể, vòng lặp xét từng cặp `a[i]`, `a[j]` và đổi chỗ ngay khi gặp phần tử phù hợp; đây là cách đổi chỗ trực tiếp theo đúng mẫu code trên slide.

### Sắp xếp tăng dần

```c
void sap_xep_tang(int a[], int n)
{
    int i, j, tg;

    for (i = 0; i < n - 1; i++)
    {
        for (j = i + 1; j < n; j++)
        {
            if (a[i] > a[j])
            {
                tg = a[i];
                a[i] = a[j];
                a[j] = tg;
            }
        }
    }
}
```

Để sắp xếp **giảm dần**, đổi điều kiện thành `if (a[i] < a[j])` (đúng chiều so sánh ở ví dụ slide Chương 4, trang 12).

## 4.4. Mảng hai chiều

Mảng hai chiều thường biểu diễn ma trận gồm hàng và cột.

```c
#define DONG 10
#define COT 10

int a[DONG][COT];
int ma_tran[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Nhập và xuất ma trận

```c
int i, j;
int m, n;

for (i = 0; i < m; i++)
{
    for (j = 0; j < n; j++)
    {
        printf("a[%d][%d] = ", i, j);
        scanf("%d", &a[i][j]);
    }
}

for (i = 0; i < m; i++)
{
    for (j = 0; j < n; j++)
        printf("%4d", a[i][j]);
    printf("\n");
}
```

### Cộng hai ma trận cùng kích thước

```c
int i, j;
for (i = 0; i < m; i++)
{
    for (j = 0; j < n; j++)
        c[i][j] = a[i][j] + b[i][j];
}
```

### Tính tổng từng hàng và từng cột

```c
int i, j;
int tong;

/* Tong tung hang */
for (i = 0; i < m; i++)
{
    tong = 0;
    for (j = 0; j < n; j++)
        tong += a[i][j];
    printf("Tong hang %d = %d\n", i, tong);
}

/* Tong tung cot */
for (j = 0; j < n; j++)
{
    tong = 0;
    for (i = 0; i < m; i++)
        tong += a[i][j];
    printf("Tong cot %d = %d\n", j, tong);
}
```

### Truyền mảng hai chiều vào hàm

```c
#define MAX_COT 100

void in_ma_tran(int a[][MAX_COT], int m, int n)
{
    int i, j;

    for (i = 0; i < m; i++)
    {
        for (j = 0; j < n; j++)
            printf("%d ", a[i][j]);
        printf("\n");
    }
}
```

Khi truyền mảng 2 chiều kiểu này, số cột (`MAX_COT`) phải có trong khai báo tham số hàm để trình biên dịch tính đúng vị trí phần tử.

---

# 5. HÀM & PHẠM VI CỦA BIẾN

## 5.1. Khai báo, định nghĩa và gọi hàm

```c
/* Nguyen mau ham (prototype) */
int tong_hai_so(int a, int b);

int main()
{
    int kq;

    kq = tong_hai_so(3, 5);
    printf("Tong = %d", kq);
    return 0;
}

/* Dinh nghia ham */
int tong_hai_so(int a, int b)
{
    int tong;

    tong = a + b;
    return tong;
}
```

- **Kiểu trả về**: kiểu dữ liệu kết quả hàm, ví dụ `int`, `float`, `char`, `void`.
- **Tên hàm**: tên định danh hợp lệ.
- **Tham số hình thức**: các biến trong phần khai báo/định nghĩa hàm.
- **Tham số thực**: giá trị/biến truyền vào khi gọi hàm.
- `void` ở kiểu trả về nghĩa là hàm không trả kết quả.
- `return` kết thúc hàm; hàm có kiểu khác `void` cần trả về giá trị thích hợp.

## 5.2. Hàm `void`

```c
void xuat(int n)
{
    printf("Gia tri = %d\n", n);
}
```

Gọi hàm: `xuat(10);`.

## 5.3. Truyền tham trị và truyền tham biến qua con trỏ

**Truyền tham trị:** C sao chép giá trị đối số vào tham số hình thức; thay đổi tham số trong hàm không làm đổi biến gốc.

```c
void doi_gia_tri(int x)
{
    x = 100;
}
```

**Muốn thay đổi biến gốc:** truyền địa chỉ biến và nhận bằng con trỏ. Đây thường được gọi là mô phỏng truyền tham biến trong C.

```c
void doi_gia_tri(int *x)
{
    *x = 100;
}

int main()
{
    int a;

    a = 5;
    doi_gia_tri(&a);
    printf("a = %d", a);  /* 100 */
    return 0;
}
```

## 5.4. Phạm vi của biến

| Loại biến | Nơi khai báo | Thời gian tồn tại / phạm vi |
|---|---|---|
| Biến cục bộ | Trong hàm hoặc khối `{}` | Chỉ truy cập trong khối đó; thông thường tồn tại khi khối đang thực thi |
| Biến toàn cục | Ngoài các hàm | Có thể được các hàm phía sau truy cập theo quy tắc khai báo/phạm vi |
| Biến `static` cục bộ | Trong hàm, có từ khóa `static` | Chỉ truy cập trong hàm nhưng giữ giá trị giữa các lần gọi |
| Biến `static` ở mức file | Ngoài hàm, có `static` | Chỉ dùng trong file nguồn hiện tại |

```c
int dem_toan_cuc = 0;

void tang_dem()
{
    static int dem_lan_goi = 0;
    dem_lan_goi++;
    dem_toan_cuc++;
    printf("Lan goi: %d\n", dem_lan_goi);
}
```

**Khuyến nghị:** chỉ dùng biến toàn cục khi thật cần thiết; ưu tiên truyền tham số và trả kết quả qua hàm để dễ kiểm soát.

---

# 6. POINTER — CON TRỎ

## 6.1. Cú pháp cốt lõi

| Cú pháp | Ý nghĩa |
|---|---|
| `int x = 10;` | Khai báo biến số nguyên |
| `int *p = &x;` | `p` lưu địa chỉ của `x` |
| `&x` | Lấy địa chỉ của biến `x` |
| `*p` | Truy cập giá trị tại địa chỉ mà `p` trỏ tới |
| `p = NULL;` | Cho con trỏ về trạng thái null |
| `p + 1` | Trỏ đến phần tử kế tiếp theo kiểu dữ liệu |
| `(*p)++` | Tăng giá trị mà `p` trỏ tới |
| `p++` | Dời con trỏ sang phần tử kế tiếp |

```c
#include <stdio.h>

int main()
{
    int x;
    int *p;

    x = 10;
    p = &x;

    printf("Gia tri x: %d\n", x);
    printf("Dia chi x: %p\n", (void *)p);
    printf("Gia tri qua con tro: %d\n", *p);

    *p = 25;
    printf("x sau khi doi: %d\n", x);

    return 0;
}
```

## 6.2. Con trỏ và mảng

```c
#include <stdio.h>

int main()
{
    int a[3] = {10, 20, 30};
    int *p;

    p = a;  /* Tuong duong &a[0] trong phep gan nay */

    printf("%d\n", a[1]);      /* 20 */
    printf("%d\n", *(p + 1));  /* 20 */
    printf("%d\n", p[2]);      /* 30 */

    return 0;
}
```

- `a[i]` tương đương `*(a + i)`.
- `p[i]` tương đương `*(p + i)`.
- Khi truyền mảng một chiều vào hàm, tham số `int a[]` được xử lý như `int *a`.
- `sizeof(a)` tại nơi khai báo mảng lấy kích thước toàn bộ mảng; trong tham số hàm, `sizeof(a)` lấy kích thước con trỏ.

## 6.3. Truyền con trỏ vào hàm

C truyền đối số theo giá trị. Muốn hàm thay đổi biến bên ngoài, truyền địa chỉ của biến.

```c
#include <stdio.h>

void tang(int *x)
{
    (*x)++;
}

int main()
{
    int n;

    n = 5;
    tang(&n);
    printf("%d\n", n);  /* 6 */

    return 0;
}
```

## 6.4. Con trỏ cấp 2 (`**`) và mảng 2 chiều

### Con trỏ cấp 2

```c
#include <stdio.h>

int main()
{
    int x;
    int *p;
    int **pp;

    x = 7;
    p = &x;
    pp = &p;

    printf("%d\n", **pp);  /* 7 */

    return 0;
}
```

### Cấp phát mảng 2 chiều động bằng `int **`

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int rows;
    int cols;
    int **a;
    int i;
    int j;

    rows = 3;
    cols = 4;

    a = (int **)malloc(rows * sizeof(int *));
    if (a == NULL)
        return 1;

    for (i = 0; i < rows; i++)
    {
        a[i] = (int *)malloc(cols * sizeof(int));
        if (a[i] == NULL)
        {
            for (j = 0; j < i; j++)
                free(a[j]);
            free(a);
            return 1;
        }
    }

    /* Gan va su dung a[i][j] tai day */
    for (i = 0; i < rows; i++)
    {
        for (j = 0; j < cols; j++)
            a[i][j] = i + j;
    }

    /* Giai phong bo nho */
    for (i = 0; i < rows; i++)
        free(a[i]);
    free(a);

    return 0;
}
```

> `int **` không đồng nghĩa với mảng 2 chiều liền một khối. Trong ví dụ trên, mỗi hàng được cấp phát riêng.

## 6.5. Cấp phát bộ nhớ động (`stdlib.h`)

| Hàm | Công dụng | Ghi nhớ |
|---|---|---|
| `malloc(size)` | Cấp phát `size` byte | Nội dung chưa được khởi tạo; kiểm tra `NULL` |
| `calloc(count, size)` | Cấp phát cho `count` phần tử | Các byte được khởi tạo về 0 |
| `realloc(ptr, new_size)` | Thay đổi kích thước vùng nhớ | Dùng con trỏ tạm để tránh mất vùng nhớ cũ khi thất bại |
| `free(ptr)` | Giải phóng vùng nhớ động | Không giải phóng cùng vùng nhớ hai lần |

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n;
    int new_n;
    int i;
    int *a;
    int *tmp;

    n = 5;
    a = (int *)malloc(n * sizeof(int));
    if (a == NULL)
        return 1;

    for (i = 0; i < n; i++)
        a[i] = i;

    new_n = 10;
    tmp = (int *)realloc(a, new_n * sizeof(int));
    if (tmp == NULL)
    {
        free(a);  /* a van hop le neu realloc that bai */
        return 1;
    }
    a = tmp;

    free(a);
    a = NULL;

    return 0;
}
```

## 6.6. Các kiểu con trỏ cần phân biệt

```c
int *p1;              /* Co the thay doi gia tri ma p1 tro toi */
const int *p2;        /* Khong sua gia tri qua p2 */
int *const p3 = &x;   /* p3 khong duoc tro sang dia chi khac */
const int *const p4 = &x; /* Khong doi dia chi, khong sua gia tri qua p4 */
```

- `void *`: con trỏ tổng quát, có thể giữ địa chỉ của nhiều kiểu dữ liệu đối tượng.
- `NULL`: con trỏ null; không được giải tham chiếu con trỏ null.
- Con trỏ chưa khởi tạo, con trỏ đã `free`, hoặc truy cập ngoài phạm vi mảng đều có thể gây lỗi nghiêm trọng (undefined behavior).

---

# 7. STRING — CHUỖI KÝ TỰ

## 7.1. Kiến thức nền

- Trong C, chuỗi là mảng `char` kết thúc bằng ký tự `\0`.
- `char s[20] = "Hello";` cần 6 byte: 5 ký tự và 1 ký tự `\0`.
- `strlen(s)` đếm số ký tự trước `\0`, không tính `\0`.
- Mảng ký tự có thể sửa nội dung; không được sửa string literal thông qua con trỏ.

```c
char s1[] = "Hello";       /* Mảng có thể sửa */
const char *s2 = "Hello";  /* Không sửa nội dung qua s2 */
```

## 7.2. Các hàm `string.h` quan trọng

| Hàm | Công dụng | Ghi nhớ |
|---|---|---|
| `strlen(s)` | Lấy độ dài chuỗi | Không tính `\0` |
| `strcpy(dst, src)` | Sao chép `src` vào `dst` | `dst` phải đủ chỗ |
| `strncpy(dst, src, n)` | Sao chép tối đa `n` ký tự | Có thể không thêm `\0` nếu nguồn dài từ `n` trở lên |
| `strcat(dst, src)` | Nối `src` vào cuối `dst` | `dst` phải đủ chỗ cho kết quả |
| `strncat(dst, src, n)` | Nối tối đa `n` ký tự từ `src` | `dst` vẫn phải đủ chỗ cho kết quả và `\0` |
| `strcmp(a, b)` | So sánh hai chuỗi | `0`: bằng; âm: nhỏ hơn; dương: lớn hơn |
| `strncmp(a, b, n)` | So sánh tối đa `n` ký tự đầu | Dùng khi chỉ cần so sánh một phần |
| `strchr(s, c)` | Tìm lần xuất hiện đầu của ký tự `c` | Trả con trỏ hoặc `NULL` |
| `strrchr(s, c)` | Tìm lần xuất hiện cuối của `c` | Trả con trỏ hoặc `NULL` |
| `strstr(s, sub)` | Tìm chuỗi con `sub` trong `s` | Trả con trỏ hoặc `NULL` |
| `strtok(s, delim)` | Tách chuỗi thành các phần | Làm thay đổi chuỗi gốc; lần gọi tiếp theo truyền `NULL` |
| `strspn(s, accept)` | Đếm số ký tự đầu thuộc tập `accept` | Dừng ở ký tự đầu tiên không thuộc tập |
| `strcspn(s, reject)` | Đếm số ký tự đầu không thuộc tập `reject` | Hay dùng để tìm vị trí `\n` |
| `strpbrk(s, accept)` | Tìm ký tự đầu tiên thuộc tập `accept` | Trả con trỏ hoặc `NULL` |

### Ví dụ nhanh

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char a[30] = "Xin";
    char b[] = " chao";
    int cmp;

    printf("Do dai: %lu\n", (unsigned long)strlen(a));
    strcat(a, b);                 /* a = "Xin chao" */
    strcpy(a, "Hello");           /* a = "Hello" */

    cmp = strcmp(a, "Hello");
    if (cmp == 0)
        puts("Giong nhau");

    return 0;
}
```

**Lưu ý:** `strcmp` không đảm bảo chỉ trả về `-1`, `0`, `1`; chỉ cần xét âm, bằng 0 hay dương.

### Các hàm bổ sung được nêu trong slide Chương 7

| Hàm | Công dụng | Header / ghi chú |
|---|---|---|
| `strupr(s)` | Đổi chuỗi thành chữ hoa | Hàm mở rộng của một số compiler cũ, không thuộc ISO C chuẩn |
| `strlwr(s)` | Đổi chuỗi thành chữ thường | Hàm mở rộng của một số compiler cũ, không thuộc ISO C chuẩn |
| `strrev(s)` | Đảo ngược chuỗi | Hàm mở rộng của một số compiler cũ, không thuộc ISO C chuẩn |
| `atoi(s)` | Chuyển chuỗi thành `int` | `<stdlib.h>`; không báo lỗi chuyển đổi rõ ràng |
| `atof(s)` | Chuyển chuỗi thành `double` | `<stdlib.h>`; không báo lỗi chuyển đổi rõ ràng |
| `atol(s)` | Chuyển chuỗi thành `long` | `<stdlib.h>`; không báo lỗi chuyển đổi rõ ràng |

Muốn viết chương trình portable, không phụ thuộc `strupr`, `strlwr`, `strrev`; có thể tự duyệt chuỗi và dùng `toupper()` / `tolower()` trong `<ctype.h>`.

## 7.3. Hàm xử lý vùng nhớ trong `string.h`

| Hàm | Công dụng | Ghi nhớ |
|---|---|---|
| `memcpy(dst, src, n)` | Sao chép `n` byte | Hai vùng nhớ không được chồng lấn |
| `memmove(dst, src, n)` | Sao chép `n` byte khi vùng nhớ có thể chồng lấn | An toàn hơn `memcpy` trong trường hợp chồng lấn |
| `memset(ptr, value, n)` | Gán `n` byte đầu bằng `value` | Thường dùng `memset(a, 0, sizeof(a))` |
| `memcmp(a, b, n)` | So sánh `n` byte | Trả âm, 0 hoặc dương |
| `memchr(ptr, c, n)` | Tìm byte có giá trị `c` trong `n` byte đầu | Trả con trỏ hoặc `NULL` |

## 7.4. Nhập và xuất chuỗi

```c
#include <stdio.h>
#include <string.h>

int main()
{
    char s[100];

    printf("Nhap chuoi: ");
    if (gets(s) != NULL)
    {
        printf("Chuoi vua nhap: %s\n", s);
    }

    return 0;
}
```

- `scanf("%s", s)` chỉ đọc đến khoảng trắng.
- Nếu dùng `scanf`, giới hạn độ rộng, ví dụ `scanf("%99s", s)` với `char s[100]`.
- `gets(s)` đọc cả dòng, kể cả dấu cách, và tự bỏ ký tự xuống dòng. Tuy nhiên, `gets()` không giới hạn số ký tự nên có thể gây tràn bộ đệm; hàm này đã bị loại khỏi chuẩn C11. Chỉ dùng theo yêu cầu học tập trên Dev-C++ cũ, không dùng trong chương trình thực tế. Khi viết chương trình an toàn, ưu tiên `fgets(s, sizeof(s), stdin)`.

## 7.5. Tự viết một số hàm chuỗi cơ bản

```c
#include <stddef.h>

size_t do_dai(const char *s)
{
    size_t i;

    i = 0;
    while (s[i] != '\0')
        i++;

    return i;
}

void sao_chep(char *dst, const char *src)
{
    while ((*dst = *src) != '\0')
    {
        dst++;
        src++;
    }
}

int so_sanh(const char *a, const char *b)
{
    while (*a != '\0' && *a == *b)
    {
        a++;
        b++;
    }

    return (unsigned char)*a - (unsigned char)*b;
}
```

---



## 7.6. Kiểu cấu trúc (`struct`)

`struct` (kiểu mẫu tin) gom nhiều trường có thể khác kiểu dữ liệu vào cùng một kiểu dữ liệu. Truy cập trường bằng dấu chấm `.`.

### Khai báo cấu trúc

```c
struct SinhVien
{
    char ma[10];
    char hoten[40];
    int namsinh;
    float diem;
};

int main()
{
    struct SinhVien sv;

    strcpy(sv.ma, "SV01");
    strcpy(sv.hoten, "Nguyen Van A");
    sv.namsinh = 2005;
    sv.diem = 8.5f;

    printf("%s - %s - %d - %.2f", sv.ma, sv.hoten, sv.namsinh, sv.diem);
    return 0;
}
```

### Dùng `typedef` để đặt tên kiểu

```c
typedef struct
{
    char ma[10];
    char hoten[40];
    float diem;
} SinhVien;

int main()
{
    SinhVien sv;
    sv.diem = 9.0f;
    return 0;
}
```

### Mảng cấu trúc

```c
struct SinhVien ds[100];
int i, n;

for (i = 0; i < n; i++)
{
    printf("Nhap ma: ");
    scanf("%9s", ds[i].ma);
    printf("Nhap diem: ");
    scanf("%f", &ds[i].diem);
}
```

### Con trỏ cấu trúc và toán tử `->`

```c
struct SinhVien sv;
struct SinhVien *p;

p = &sv;
p->diem = 8.0f;       /* Tuong duong (*p).diem = 8.0f */
```

- Biến cấu trúc: `sv.diem`.
- Con trỏ cấu trúc: `p->diem`.
- `p->diem` tương đương `(*p).diem`.

### Cấu trúc lồng nhau

```c
struct NgayThang
{
    int ngay;
    int thang;
    int nam;
};

struct SinhVien
{
    char hoten[40];
    struct NgayThang ngaysinh;
    float diem;
};

int main()
{
    struct SinhVien sv;

    sv.ngaysinh.ngay = 1;
    sv.ngaysinh.thang = 1;
    sv.ngaysinh.nam = 2005;
    return 0;
}
```

**Lưu ý:** mảng ký tự trong `struct` có kích thước cố định; phải bảo đảm chuỗi nhập vừa vùng nhớ. Không dùng `fflush(stdin)` để xóa bộ đệm đầu vào vì hành vi này không được chuẩn C quy định.

---

# 8. KIỂU TẬP TIN (FILE) — ĐỌC VÀ GHI DỮ LIỆU

Thư viện cần dùng: `#include <stdio.h>`. Biến tập tin được khai báo bằng con trỏ `FILE *`.

## 8.1. Quy trình thao tác tập tin

1. Khai báo con trỏ tập tin: `FILE *fp;`.
2. Mở tập tin: `fp = fopen(duong_dan, che_do);`.
3. Kiểm tra mở thành công: `fp != NULL`.
4. Đọc/ghi dữ liệu.
5. Đóng tập tin: `fclose(fp);`.

## 8.2. Các chế độ mở file

| Chế độ | Ý nghĩa |
|---|---|
| `"r"` | Mở file văn bản có sẵn để đọc |
| `"w"` | Tạo mới hoặc xóa nội dung cũ rồi ghi file văn bản |
| `"a"` | Ghi nối vào cuối file văn bản; tạo file nếu chưa có |
| `"r+"` | Mở file văn bản có sẵn để đọc và ghi |
| `"w+"` | Tạo mới hoặc xóa nội dung cũ; cho phép đọc và ghi |
| `"a+"` | Mở để đọc và ghi nối cuối; tạo mới nếu chưa có |
| `"rb"` | Mở file nhị phân để đọc |
| `"wb"` | Tạo mới/xóa nội dung cũ rồi ghi file nhị phân |
| `"ab"` | Ghi nối cuối file nhị phân |
| `"rb+"`, `"r+b"` | Đọc và ghi file nhị phân có sẵn |
| `"wb+"`, `"w+b"` | Tạo mới/xóa nội dung cũ; đọc và ghi nhị phân |
| `"ab+"`, `"a+b"` | Đọc và ghi nối cuối file nhị phân |

## 8.3. Các hàm xử lý file văn bản

| Hàm | Công dụng | Ví dụ |
|---|---|---|
| `fopen(path, mode)` | Mở tập tin | `fp = fopen("data.txt", "r");` |
| `fclose(fp)` | Đóng tập tin | `fclose(fp);` |
| `fprintf(fp, fmt, ...)` | Ghi theo định dạng | `fprintf(fp, "%d\n", n);` |
| `fscanf(fp, fmt, ...)` | Đọc theo định dạng | `fscanf(fp, "%d", &n);` |
| `fputc(ch, fp)` | Ghi một ký tự | `fputc('A', fp);` |
| `fgetc(fp)` | Đọc một ký tự, trả về `int` để giữ được `EOF` | `ch = fgetc(fp);` |
| `fputs(s, fp)` | Ghi chuỗi | `fputs(s, fp);` |
| `fgets(s, n, fp)` | Đọc tối đa `n-1` ký tự từ file stream | `fgets(s, sizeof(s), fp);` |
| `feof(fp)` | Kiểm tra cờ kết thúc file sau lần đọc | `feof(fp)` |

**Phân biệt quan trọng:** `gets(s)` đọc từ bàn phím (`stdin`) theo yêu cầu học tập của bạn; `fgets(s, n, fp)` đọc từ một luồng file cụ thể. Hai hàm này không thay thế trực tiếp cho nhau. Không dùng `gets()` trong chương trình thực tế.

### Ghi file văn bản

```c
#include <stdio.h>

int main()
{
    FILE *fp;
    int i;
    int n;
    int a[100];

    printf("Nhap n: ");
    scanf("%d", &n);
    for (i = 0; i < n; i++)
        scanf("%d", &a[i]);

    fp = fopen("mang.txt", "w");
    if (fp == NULL)
    {
        printf("Khong mo duoc file");
        return 1;
    }

    fprintf(fp, "%d\n", n);
    for (i = 0; i < n; i++)
        fprintf(fp, "%d ", a[i]);

    fclose(fp);
    return 0;
}
```

### Đọc file văn bản

```c
#include <stdio.h>

int main()
{
    FILE *fp;
    int n;
    int i;
    int x;

    fp = fopen("mang.txt", "r");
    if (fp == NULL)
    {
        printf("Khong mo duoc file");
        return 1;
    }

    if (fscanf(fp, "%d", &n) == 1)
    {
        for (i = 0; i < n; i++)
        {
            if (fscanf(fp, "%d", &x) == 1)
                printf("%d ", x);
            else
                break;
        }
    }

    fclose(fp);
    return 0;
}
```

Cách đọc ký tự đến cuối file:

```c
int ch;
while ((ch = fgetc(fp)) != EOF)
{
    putchar(ch);
}
```

Dùng `int ch`, không dùng `char ch`, vì `fgetc()` phải biểu diễn được mọi giá trị byte và giá trị đặc biệt `EOF`.

## 8.4. File nhị phân

File nhị phân lưu dữ liệu theo byte/khối. Hai hàm chính:

| Hàm | Cú pháp | Công dụng |
|---|---|---|
| `fwrite` | `fwrite(ptr, size, count, fp)` | Ghi `count` phần tử, mỗi phần tử `size` byte |
| `fread` | `fread(ptr, size, count, fp)` | Đọc tối đa `count` phần tử, mỗi phần tử `size` byte |

### Ghi và đọc một biến cấu trúc

```c
#include <stdio.h>

struct NgayThang
{
    int ngay;
    int thang;
    int nam;
};

int main()
{
    FILE *fp;
    struct NgayThang ngay;

    ngay.ngay = 7;
    ngay.thang = 11;
    ngay.nam = 2026;

    fp = fopen("ngay.dat", "wb");
    if (fp == NULL)
        return 1;

    if (fwrite(&ngay, sizeof(struct NgayThang), 1, fp) != 1)
    {
        fclose(fp);
        return 1;
    }
    fclose(fp);

    fp = fopen("ngay.dat", "rb");
    if (fp == NULL)
        return 1;

    if (fread(&ngay, sizeof(struct NgayThang), 1, fp) == 1)
        printf("%02d/%02d/%04d", ngay.ngay, ngay.thang, ngay.nam);

    fclose(fp);
    return 0;
}
```

**Lưu ý:** file nhị phân ghi trực tiếp cấu trúc có thể phụ thuộc kích thước kiểu, padding và kiến trúc máy; không đảm bảo trao đổi nhị phân giữa mọi hệ thống khác nhau.

## 8.5. Di chuyển con trỏ tập tin

| Hàm / hằng | Công dụng |
|---|---|
| `fseek(fp, offset, origin)` | Di chuyển vị trí đọc/ghi |
| `ftell(fp)` | Trả vị trí hiện tại (kiểu `long`) |
| `rewind(fp)` | Đưa vị trí về đầu file và xóa cờ lỗi/kết thúc |
| `SEEK_SET` | Tính offset từ đầu file |
| `SEEK_CUR` | Tính offset từ vị trí hiện tại |
| `SEEK_END` | Tính offset từ cuối file |
| `EOF` | Giá trị báo kết thúc/không có ký tự khi đọc |

```c
fseek(fp, 0L, SEEK_END);
length = ftell(fp);
rewind(fp);
```

## 8.6. Hàm không chuẩn / phụ thuộc trình biên dịch

- `fcloseall()` và `fputw()`/`fgetw()` xuất hiện trong một số trình biên dịch cũ nhưng không thuộc thư viện chuẩn C portable.
- Nên dùng `fclose()` cho từng file; dùng `fwrite()`/`fread()` để xử lý dữ liệu nhị phân.
- Kiểm tra kết quả `fopen`, `fscanf`, `fread`, `fwrite` và luôn đóng file sau khi sử dụng.

---

# 9. RECURSION — ĐỆ QUY

## 9.1. Khái niệm và cấu trúc

Đệ quy là kỹ thuật một hàm gọi lại chính nó để giải bài toán nhỏ hơn.

Một hàm đệ quy cần có:

1. **Điều kiện dừng (base case):** trả kết quả mà không gọi đệ quy tiếp.
2. **Bước đệ quy (recursive case):** đưa bài toán về trường hợp nhỏ hơn.
3. Mỗi lần gọi phải tiến gần đến điều kiện dừng.

Mẫu chung:

```c
KieuTraVe ten_ham(ThamSo n)
{
    if (dieu_kien_dung)
        return ket_qua_co_so;

    return ket_hop(ten_ham(bai_toan_nho_hon));
}
```

## 9.2. Các mẫu đệ quy thường gặp

### Giai thừa: `n!`

```c
long factorial(int n)
{
    if (n <= 1)
        return 1;

    return (long)n * factorial(n - 1);
}
```

### Tổng từ 1 đến n

```c
long sum_n(int n)
{
    if (n <= 0)
        return 0;

    return n + sum_n(n - 1);
}
```

### Fibonacci — bản đệ quy cơ bản

```c
long fib(int n)
{
    if (n <= 1)
        return n;

    return fib(n - 1) + fib(n - 2);
}
```

> Bản này dễ hiểu nhưng rất chậm khi `n` lớn vì tính toán lặp lại nhiều lần. Nên dùng vòng lặp hoặc quy hoạch động khi cần hiệu năng.

### Tổ hợp C(n, k) theo công thức Pascal

```c
long to_hop(int n, int k)
{
    if (n < 0 || k < 0 || k > n)
        return 0;
    if (k == 0 || k == n)
        return 1;

    return to_hop(n - 1, k - 1) + to_hop(n - 1, k);
}
```

Cách đệ quy này phù hợp minh họa công thức Pascal; tính nhiều lần cùng một giá trị nên không hiệu quả khi `n` lớn. Kiểu `long` có giới hạn và có thể tràn số.

### UCLN — thuật toán Euclid

```c
int gcd(int a, int b)
{
    if (b == 0)
        return a;

    return gcd(b, a % b);
}
```

### Tính tổng mảng bằng đệ quy

```c
long sum_array(const int a[], int n)
{
    if (n <= 0)
        return 0;

    return a[n - 1] + sum_array(a, n - 1);
}
```

### In mảng từ trái sang phải / phải sang trái

```c
#include <stdio.h>

void print_forward(const int a[], int n, int i)
{
    if (i >= n)
        return;

    printf("%d ", a[i]);
    print_forward(a, n, i + 1);
}

void print_reverse(const int a[], int n, int i)
{
    if (i >= n)
        return;

    print_reverse(a, n, i + 1);
    printf("%d ", a[i]);
}
```

Cách gọi: `print_forward(a, n, 0);` hoặc `print_reverse(a, n, 0);`.

### Đảo chuỗi tại chỗ bằng hai con trỏ và đệ quy

```c
#include <string.h>

void reverse_string(char *left, char *right)
{
    char temp;

    if (left >= right)
        return;

    temp = *left;
    *left = *right;
    *right = temp;

    reverse_string(left + 1, right - 1);
}

/* Cach goi:
char s[] = "hello";
if (s[0] != '\0')
    reverse_string(s, s + strlen(s) - 1);
*/
```

## 9.3. Đệ quy và ngăn xếp (call stack)

- Mỗi lần gọi hàm tạo một frame trên call stack, lưu tham số, biến cục bộ và điểm quay lại.
- Khi gặp điều kiện dừng, các lời gọi kết thúc lần lượt theo thứ tự ngược lại.
- Đệ quy quá sâu có thể gây tràn stack.
- Đệ quy không phải lúc nào cũng nhanh hơn vòng lặp; nên dùng vòng lặp khi bài toán đơn giản hoặc độ sâu quá lớn.

---

# 10. BẢNG NHỚ NHANH

| Muốn làm gì? | Dùng |
|---|---|
| Độ dài chuỗi | `strlen(s)` |
| Sao chép chuỗi | `strcpy(dst, src)` |
| Nối chuỗi | `strcat(dst, src)` |
| So sánh chuỗi | `strcmp(a, b)` |
| Tìm ký tự | `strchr(s, c)` |
| Tìm chuỗi con | `strstr(s, sub)` |
| Kiểm tra ký tự là chữ cái | `isalpha(c)` |
| Kiểm tra ký tự là chữ số | `isdigit(c)` |
| Kiểm tra ký tự trắng | `isspace(c)` |
| Đổi chữ thường thành hoa | `toupper(c)` |
| Đổi chữ hoa thành thường | `tolower(c)` |
| Nhập cả dòng (theo yêu cầu Dev-C++ cũ) | `gets(s)` — không an toàn; ưu tiên `fgets` trong dự án thực tế |
| Lấy địa chỉ biến | `&x` |
| Lấy giá trị qua con trỏ | `*p` |
| Con trỏ cấp 2 | `T **pp` |
| Cấp phát bộ nhớ | `malloc`, `calloc` |
| Đổi kích thước vùng nhớ | `realloc` |
| Giải phóng bộ nhớ | `free` |
| Điều kiện dừng đệ quy | `if (...) return ...;` |
| Gọi lại hàm | `ten_ham(bai_toan_nho_hon)` |
| In dữ liệu | `printf()` |
| Nhập dữ liệu | `scanf()` |
| Rẽ nhánh nhiều trường hợp | `switch ... case` |
| Lặp biết trước số lần | `for` |
| Lặp kiểm tra trước | `while` |
| Lặp thực hiện ít nhất một lần | `do ... while` |
| Thoát vòng lặp / bỏ qua lượt | `break` / `continue` |
| Khai báo mảng / phần tử | `int a[100];` / `a[i]` |
| Mảng 2 chiều | `int a[so_dong][so_cot];` |
| Khai báo hàm | `kieu_tra_ve ten_ham(tham_so);` |
| Truyền địa chỉ cho hàm | `ten_ham(&x);` |
| Truy cập trường struct | `sv.diem` hoặc `p->diem` |
| Mở/đóng file | `fopen()` / `fclose()` |
| Đọc/ghi file text | `fscanf`, `fprintf`, `fgetc`, `fputc`, `fgets`, `fputs` |
| Đọc/ghi file nhị phân | `fread()` / `fwrite()` |
| Di chuyển vị trí file | `fseek()`, `rewind()`, `ftell()` |

## 10.1. LỖI RẤT HAY GẶP

1. Quên `\0` ở cuối chuỗi tự tạo bằng tay.
2. Dùng `strcpy` hoặc `strcat` khi mảng đích không đủ dung lượng.
3. Nghĩ `strlen` tính cả `\0` — thực tế không.
4. So sánh chuỗi bằng `==` — phải dùng `strcmp` để so sánh nội dung.
5. Dùng `scanf("%s", s)` mà không giới hạn độ rộng.
6. Giải tham chiếu con trỏ `NULL`, con trỏ chưa khởi tạo hoặc con trỏ đã `free`.
7. Quên `free` vùng nhớ đã cấp phát động.
8. Gọi `realloc` trực tiếp lên con trỏ duy nhất rồi ghi đè kết quả; nên dùng con trỏ tạm.
9. Đệ quy không có điều kiện dừng hoặc không tiến gần điều kiện dừng.
10. Đệ quy quá sâu dẫn đến tràn stack.

## 10.2. LƯU Ý RIÊNG KHI DÙNG DEV-C++

- Lưu mã nguồn C với đuôi `.c`, ví dụ `main.c`.
- Các ví dụ dùng thống nhất dạng `int main()` theo yêu cầu.
- Dùng `int i;` ở đầu hàm hoặc đầu khối lệnh, sau đó mới viết `for (i = 0; i < n; i++)`.
- Tránh viết `for (int i = 0; ...)` khi muốn tương thích với trình biên dịch C đời cũ.
- Khai báo các biến ở đầu khối `{ ... }`, trước những câu lệnh thực thi.
- Dùng `/* ... */` cho chú thích để tương thích với chuẩn C cũ; không dùng `//` trong mã cần C90.
- Các ví dụ dùng `long` cho kết quả đệ quy để tránh phụ thuộc `long long` của C99. Tuy nhiên, `long` vẫn có giới hạn; kết quả lớn có thể tràn số.


---

## Tài liệu đối chiếu

Nội dung tổng hợp dựa trên 8 bài giảng PDF được cung cấp:

- Chương 1: Các khái niệm cơ bản — chương trình, thuật toán, cấu trúc chương trình C.
- Chương 2: Kiểu dữ liệu và phép toán — kiểu dữ liệu, toán tử, biến/hằng, nhập xuất.
- Chương 3: Các lệnh có cấu trúc — `if`, `switch`, `for`, `while`, `do...while`, `break`, `continue`, `goto`.
- Chương 4: Kiểu mảng dữ liệu — mảng một chiều, hai chiều, nhập xuất, sắp xếp chọn, thao tác ma trận.
- Chương 5: Hàm và phạm vi của biến — nguyên mẫu hàm, tham số, truyền tham trị/tham biến, phạm vi biến.
- Chương 6: Kiểu con trỏ — địa chỉ, giải tham chiếu, cấp phát động, liên hệ con trỏ và mảng.
- Chương 7: Kiểu chuỗi và kiểu cấu trúc — chuỗi, hàm xử lý chuỗi, `struct`, mảng cấu trúc, con trỏ cấu trúc.
- Chương 8: Kiểu tập tin — file văn bản, file nhị phân, đọc/ghi và di chuyển con trỏ file.

Phần `ctype.h`, các mẫu đệ quy và một số ghi chú an toàn/di động được giữ lại từ cheat sheet cũ hoặc bổ sung để hỗ trợ ôn tập; chúng không phải toàn bộ đều được trình bày thành mục riêng trong 8 PDF.
