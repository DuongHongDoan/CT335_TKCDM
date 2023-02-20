# **Chia địa chỉ mạng con theo kiểu VLSM**
## **1**. Giới thiệu
- Variable Length Subnet Masks (VLSM) là kiểu chia mạng con thành các mạng có độ dài khác nhau, với số lượng máy trên mỗi nhánh mạng khác nhau, từ đó giúp tránh lãng phí địa chỉ IP.

- VLSM là kiểu phân chia địa chỉ **không phân lớp**, nên nó sẽ tránh gây lãng phí địa chỉ IP hơn phân lớp.

## **2**. Cách chia mạng con kiểu VLSM
- Đề bài thường sẽ cho một địa chỉ IP và mặt nạ mạng của nó, ở dạng **IPAddress/mask**, và số lượng máy ở mỗi nhánh mạng cần được định cấu hình.
- Chúng ta bắt đầu cấu hình với nhánh mạng con có số lượng máy nhiều nhất đến thấp nhất. (Có thể ngược lại theo yêu cầu đề bài)
- Cách chia:
  - Đầu tiên ta xác định nhánh mạng con nào có số lượng máy nhiều nhất, và chọn số lượng máy ở nhánh đó.
  - Tính số bit mượn cho phần mạng (n) và phần máy (m):
    - Phần máy: ***$2^m$ - 2 >= số máy cần dùng***. Từ đó suy ra **m**.
    - Phần mạng: ***m + n = 32 - mask***. Từ đó suy ra **n**.
  - Từ m và n ta có thể tính được khoảng địa chỉ IP mà nhánh mạng có thể được cấp địa chỉ IP cho các thiết bị trong đó, bằng việc phân tách nhị phân.

***\***Hãy xem ví dụ để biết rõ hơn về cách thức chia mạng con, chớ đọc cách chia cũng chưa hình dung ra được đâu, hehe :))***
## **3**. Ví dụ
>Cho nhánh mạng **172.20.0.0/24**, hãy thiết kế địa chỉ IP theo các yêu cầu sau:

|Mạng con (subnet) | Số lượng máy |
|:----------------:|:------------:|
|Subnet A          |1             |
|Subnet B          |80-100        |
|Subnet C          |40-52         |
|Subnet D          |20-29         |
|Subnet E          |12            |
|Subnet F          |5             |