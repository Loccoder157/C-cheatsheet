# CHEAT SHEET C — STRING, CON TRỎ & ĐỆ QUY

> Phiên bản dùng phong cách khai báo biến kiểu C90 để dễ biên dịch trên Dev-C++/MinGW đời cũ: **khai báo biến ở đầu khối lệnh** và khai báo biến đếm `int i;` bên ngoài vòng `for`.
>
> Lưu file với đuôi `.c` và biên dịch ở chế độ C, không phải C++.

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

# 1. STRING — CHUỖI KÝ TỰ

## 1.1. Kiến thức nền

- Trong C, chuỗi là mảng `char` kết thúc bằng ký tự `\0`.
- `char s[20] = "Hello";` cần 6 byte: 5 ký tự và 1 ký tự `\0`.
- `strlen(s)` đếm số ký tự trước `\0`, không tính `\0`.
- Mảng ký tự có thể sửa nội dung; không được sửa string literal thông qua con trỏ.

```c
char s1[] = "Hello";       /* Mảng có thể sửa */
const char *s2 = "Hello";  /* Không sửa nội dung qua s2 */
```

## 1.2. Các hàm `string.h` quan trọng

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

## 1.3. Hàm xử lý vùng nhớ trong `string.h`

| Hàm | Công dụng | Ghi nhớ |
|---|---|---|
| `memcpy(dst, src, n)` | Sao chép `n` byte | Hai vùng nhớ không được chồng lấn |
| `memmove(dst, src, n)` | Sao chép `n` byte khi vùng nhớ có thể chồng lấn | An toàn hơn `memcpy` trong trường hợp chồng lấn |
| `memset(ptr, value, n)` | Gán `n` byte đầu bằng `value` | Thường dùng `memset(a, 0, sizeof(a))` |
| `memcmp(a, b, n)` | So sánh `n` byte | Trả âm, 0 hoặc dương |
| `memchr(ptr, c, n)` | Tìm byte có giá trị `c` trong `n` byte đầu | Trả con trỏ hoặc `NULL` |

## 1.4. Nhập và xuất chuỗi

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

## 1.5. Tự viết một số hàm chuỗi cơ bản

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

# 2. POINTER — CON TRỎ

## 2.1. Cú pháp cốt lõi

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

## 2.2. Con trỏ và mảng

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

## 2.3. Truyền con trỏ vào hàm

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

## 2.4. Con trỏ cấp 2 (`**`) và mảng 2 chiều

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

## 2.5. Cấp phát bộ nhớ động (`stdlib.h`)

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

## 2.6. Các kiểu con trỏ cần phân biệt

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

# 3. RECURSION — ĐỆ QUY

## 3.1. Khái niệm và cấu trúc

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

## 3.2. Các mẫu đệ quy thường gặp

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

## 3.3. Đệ quy và ngăn xếp (call stack)

- Mỗi lần gọi hàm tạo một frame trên call stack, lưu tham số, biến cục bộ và điểm quay lại.
- Khi gặp điều kiện dừng, các lời gọi kết thúc lần lượt theo thứ tự ngược lại.
- Đệ quy quá sâu có thể gây tràn stack.
- Đệ quy không phải lúc nào cũng nhanh hơn vòng lặp; nên dùng vòng lặp khi bài toán đơn giản hoặc độ sâu quá lớn.

---

# 4. BẢNG NHỚ NHANH

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

# 5. LỖI RẤT HAY GẶP

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

## 6. LƯU Ý RIÊNG KHI DÙNG DEV-C++

- Lưu mã nguồn C với đuôi `.c`, ví dụ `main.c`.
- Các ví dụ dùng thống nhất dạng `int main()` theo yêu cầu.
- Dùng `int i;` ở đầu hàm hoặc đầu khối lệnh, sau đó mới viết `for (i = 0; i < n; i++)`.
- Tránh viết `for (int i = 0; ...)` khi muốn tương thích với trình biên dịch C đời cũ.
- Khai báo các biến ở đầu khối `{ ... }`, trước những câu lệnh thực thi.
- Dùng `/* ... */` cho chú thích để tương thích với chuẩn C cũ; không dùng `//` trong mã cần C90.
- Các ví dụ dùng `long` cho kết quả đệ quy để tránh phụ thuộc `long long` của C99. Tuy nhiên, `long` vẫn có giới hạn; kết quả lớn có thể tràn số.
