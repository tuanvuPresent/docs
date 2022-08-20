# Clean code

## Thế nào là clean code

* Dễ đọc
* Dễ thay đổi
* Dễ bảo trì

## Quy tắc đặt tên

#### 1. Dùng những tên thể hiện được mục đích

{% code lineNumbers="true" %}
```java
// Bad 
int a // elapsed time in days
// Good
int elapsedTimeInDays
```
{% endcode %}

#### 2. Tạo sự khác biệt rõ ràng

Hãy phân biệt tên theo cách cung cấp cho người đọc những khác biệt rõ ràng.

{% code lineNumbers="true" %}
```java
getCustomer();
getCustomerInfo();
getCustomerData();
```
{% endcode %}

#### 3. Dùng những tên phát âm được

{% code lineNumbers="true" %}
```javascript
// Bad
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
};
// Good
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
};
```
{% endcode %}

#### 4. Dùng những tên tìm kiếm được

#### 5. Không cần phải thêm các thành phần tiền tố

#### 6. Tên lớp nên sử dụng danh từ hoặc cụm danh từ không nên là một động từ

#### 7. Tên các phương thức nên là động từ ( get, set, is)

#### 8. Chọn một từ cho mỗi khái niệm

{% hint style="info" %}
fetch, retrieve, get là các phương thức có cùng chức năng nên thống nhất 1 cách đặt
{% endhint %}

#### 9. Magic number

{% code lineNumbers="true" %}
```javascript
// Bad practice
for (let i = 0; i < 60; i++) {
  // do something
}

// Good practice
const MINUTES_OF_THE_HOUR = 60;
for (let i = 0; i < MINUTES_OF_THE_HOUR; i++) {
  // do something
}
```
{% endcode %}

#### 10. Viết hoa các giá trị không đổi

{% code lineNumbers="true" %}
```java
  const DAYS_IN_A_YEAR = 365;
```
{% endcode %}

## Cách viết hàm

* Nguyên tắc đầu tiên của hàm là chúng phải nhỏ. Nguyên tắc thứ hai là chúng phải nhỏ hơn nữa
* Hàm thực một việc
* Tránh dùng nhiều vòng lặp lồng nhau

{% hint style="info" %}
{% code overflow="wrap" %}
```
“HÀM CHỈ NÊN THỰC HIỆN MỘT VIỆC. CHÚNG NÊN LÀM TỐT VIỆC ĐÓ, VÀ CHỈ LÀM DUY NHẤT VIỆC ĐÓ”
```
{% endcode %}
{% endhint %}

* Nguyên tắc stepdown

{% hint style="info" %}
{% code overflow="wrap" %}
```
Chúng tôi muốn code được đọc tuần tự từ trên xuống. Chúng tôi muốn mọi hàm được theo sau bởi các hàm có cấp độ trừu tượng lớn hơn để chúng tôi có thể đọc chương trình. Và khi chúng tôi xem xét một danh sách các khai báo hàm, mức độ trừu tượng của chúng phải được giảm dần
```
{% endcode %}
{% endhint %}

{% hint style="info" %}
{% code overflow="wrap" lineNumbers="true" %}
```
Nguyên tắc Đơn nhiệm: Mỗi lớp chỉ nên chịu trách nhiệm về một nhiệm vụ cụ thể nào đó mà thôi.
Nguyên tắc Đóng & mở: Chúng ta nên hạn chế việc chỉnh sửa bên trong một Class hoặc Module có sẵn, thay vào đó hãy xem xét mở rộng chúng.
```
{% endcode %}
{% endhint %}

* Đối số của hàm

{% hint style="info" %}
{% code overflow="wrap" %}
```
Hàm có một đối số đầu vào sẽ là tốt “đứng thứ 2” (tốt nhất vẫn là hàm không có đối số).
```
{% endcode %}
{% endhint %}

## Comment

{% hint style="info" %}
```
“Đừng biến đống code gớm ghiếc của bạn thành comment – hãy viết lại nó” 
BRIAN W. KERNIGHAN AND P. J. PLAUGHER
```
{% endhint %}

Một số comment là cần thiết hoặc có ích. Chúng ta sẽ xem xét một vài trường hợp mà tôi cho là xứng đáng để bạn bỏ công ra viết. Tuy nhiên, hãy nhớ rằng comment thật sự tốt là comment không cần phải viết ra.

* COMMENT PHÁP LÝ
* COMMENT CUNG CẤP THÔNG TIN
* GIẢI THÍCH MỤC ĐÍCH
* TODO COMMENTS

## Format Code

Luôn format code

## Class

* Lớp nên nhỏ

{% hint style="info" %}
{% code overflow="wrap" %}
```
Nguyên tắc đơn trách nhiệm (SRP) nêu rõ rằng một lớp hoặc mô-đun nên có một và chỉ một lý do để thay đổi. Nguyên tắc này cung cấp cho chúng ta cả định nghĩa về trách nhiệm và hướng dẫn về quy mô của lớp. Các lớp chỉ nên có một trách nhiệm — chỉ một lý do để thay đổi.
```
{% endcode %}
{% endhint %}

## Một số nguyên tắc

* Single Responsibility Principle (SRP)
* Open Closed Principle (OCP)
* Dependency Inversion Principle (DIP)
