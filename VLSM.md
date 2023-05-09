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

## **Topology**
![topology_vlsm](https://user-images.githubusercontent.com/93761311/236967633-9b3602bf-92a1-4fcb-97ea-8d0979bdbf2f.png)

|Mạng con (subnet) | Số lượng máy |
|:----------------:|:------------:|
|Subnet A          |1             |
|Subnet B          |80-100        |
|Subnet C          |40-52         |
|Subnet D          |20-29         |
|Subnet E          |12            |
|Subnet F          |5             |


## **Các thành phần trong sơ đồ mạng**
| Phần cứng         | Số lượng  | Mô tả                             |
| :------------     |:---------:| :-----                            |
| Router (1841)     | 1         |Thiết bị kết nối nhiều nhánh mạng  |
| Switch (2950-24)  | 1         |Thiết bị kết nối các thành phần trong cùng mạng|
| PC                | 3         |Các máy tính - thiết bị đầu cuối   |
| Cáp console       |1          |Kết nối PC1 (cổng RS) với Router (cổng console)|
| Cáp chéo          |1          |Kết nối PC1 (cổng Fa) với Router (cổng Fa0/0) |
| Cáp thẳng         |3          |Kết nối PC2 và PC3 (cổng Fa) với Switch (cổng Fa0/2, Fa0/3) và Switch (cổng Fa0/1) với Router (cổng Fa0/1)  |

#### ****NOTE: Khi chia subnet đa phần sẽ chia theo kiểu từ số lượng máy lớn nhất đến nhỏ nhất*. Cho nên trong bài này, tui sẽ chia subnet từ Subnet B trước và cuối cùng là Subnet A.

## **Bài làm**
### **I. Chia subnet**
### **1. Chia subnet cho Subnet B (subnet max = 100).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.0| 255.255.255.128 |172.20.0.1|172.20.0.126|172.20.0.127|
#### ****Do số lượng máy trong Subnet B nằm trong khoảng 80-100 máy, nên số lượng subnet cần để chia là max=100*
### **2. Chia subnet cho Subnet C (subnet max = 52).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.128| 255.255.255.192 |172.20.0.129|172.20.0.190|172.20.0.191|
### **3. Chia subnet cho Subnet D (subnet max = 29).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.192| 255.255.255.224 |172.20.0.193|172.20.0.222|172.20.0.223|
### **4. Chia subnet cho Subnet E (subnet max = 12).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.224| 255.255.255.240 |172.20.0.225|172.20.0.238|172.20.0.239|
### **5. Chia subnet cho Subnet F (subnet max = 5).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.240| 255.255.255.248 |172.20.0.241|172.20.0.246|172.20.0.247|
### **6. Chia subnet cho Subnet A (subnet max = 1).**
| Network Address (Đ/c mạng)         | Mask (Mặt nạ mạng)  | First Host Address (Đ/c mạng đầu tiên)| Last Host Address (Đ/c mạng cuối cùng)|Broadcast (Đ/c quảng bá)|
| :------------     |:---------:| :-----                            |:---------|:-----
| 172.20.0.248| 255.255.255.252 |172.20.0.249|172.20.0.250|172.20.0.251|
### **II. Định cấu hình cho topology.**
>Như topology đã được cung cấp, các Subnet C, D, E, F là dự phòng, cho nên định cấu hình chỉ áp dụng đối với Subnet A và B.

- Subnet A: có 1 Host **(PC1)** và 1 giao diện Router1 **(Fa0/0)**. Vì vậy, PC1 sẽ dùng địa chỉ IP **đầu tiên** (đã liệt kê ở I.6), và giao diện Fa0/0 của Router1 sẽ dùng địa chỉ IP **cuối cùng**
- Subnet B: có 2 Host **(PC2 và PC3)**, 1 Switch và 1 giao diện Router1 **(Fa0/1)**. Vì vậy, PC1 và PC2 sẽ làn lượt dùng địa chỉ IP **đầu tiên và thứ hai** (đã liệt kê ở I.1), và giao diện Fa0/1 của Router 1 dùng địa chỉ IP **cuối cùng**. Switch không có chức năng của tầng 3 nên không yêu cầu định cấu hình địa chỉ IP.
### **Bảng thông tin địa chỉ IP cho các thiết bị trong topology*

### **2. Chia subnet cho Subnet C (subnet max = 52).**
|Device (Thiết bị)|Subnet|Đ/c IP| Mask (Mặt nạ mạng)|Gateway|
| :------------     |:---------:| :-----                            |:---------|:-----
|PC1| 172.20.0.248/30 |172.20.0.249|255.255.255.252|172.20.0.250|
|R1 - Fa0/0| 172.20.0.248/30 |172.20.0.250|255.255.255.252|172.20.0.250|
|PC2| 172.20.0.0/25 |172.20.0.1|255.255.255.128|172.20.0.126|
|PC3| 172.20.0.0/25 |172.20.0.2|255.255.255.128|172.20.0.126|
|R1 - Fa0/1| 172.20.0.0/25 |172.20.0.126|255.255.255.128|172.20.0.126|
#### **Gateway chính là địa chỉ IP của giao diện router trong cùng nhánh mạng*

### **II. Cấu hình thiết bị trên packet tracer.**
>Từ bảng thông tin ở phần II, có thể tiến hành cấu hình địa chỉ IP cho các thiết bị trong topology và kiểm tra kết nối giữa chúng.