# **Lab 2: Cấu hình định tuyến tĩnh cơ bản**
## **Topology**
![topo_static_route](https://user-images.githubusercontent.com/93761311/236977049-f4cce4cd-eba7-4a9c-9fa2-0ae06fa1ca96.png)
## **Các thành phần trong sơ đồ mạng**
| Phần cứng         | Số lượng  | Mô tả                             |
| :------------     |:---------:| :-----                            |
| Router (1841)     | 3         |Thiết bị kết nối nhiều nhánh mạng  |
| Switch (2950-24)  | 3         |Thiết bị kết nối các thành phần trong cùng mạng|
| PC                | 3         |Các máy tính - thiết bị đầu cuối   |
| Cáp Serial DCE          |2          |Kết nối các Router lại với nhau |
| Cáp thẳng         |6          |Kết nối các PC với Switch và Switch với Router  |
## **Bảng địa chỉ**
|Device         |Interface (giao diện)|Đ/c IP| Subnet Mask (mặt nạ mạng con) | Default Gateway (Cổng mặc định)|
| :------------     |:---------:| :-----                            |:----|:----
| R1 (1841)     |Fa0/0, S0/0/0|172.16.3.1, 172.16.2.1|255.255.255.0| N/A
| R2 (1841)     |Fa0/0, S0/0/0, S0/0/1|172.16.1.1, 172.16.2.2, 192.168.1.2|255.255.255.0| N/A
| R3 (1841)     |Fa0/0, S0/0/1|192.168.2.1, 192.168.1.1|255.255.255.0| N/A
|PC1     |NIC|172.16.3.10|255.255.255.0| 172.16.3.1
|PC2     |NIC|172.16.1.10|255.255.255.0| 172.16.1.1
|PC3     |NIC|192.168.2.10|255.255.255.0| 192.168.2.1
## **Kịch bản**
- Kết nối vật lý các thiết bị lại với nhau như topo đã được cung cấp.
- Cấu hình địa chỉ IP cho các thiết bị như đã cho ở *Bảng địa chỉ*.
- Kiểm tra kết nối giũa các thiết bị. Đầu tiên là các thiết bị kết nối trực tiếp với nhau trong cùng nhánh mạng. Sau đó là kết nối giữa các thiết bị trên các nhánh mạng khác (sẽ không thể kết nối được, do chưa định tuyến cho các router).
- Định tuyến tĩnh cho các router có trong topo.
- Kiểm tra kết nối giữa các thiết bị trên nhánh mạng khác nhau (thành công).
## **Bài làm**
### **I**. Cấu hình vật lý.
- Switch với PC và Router: dùng cáp thẳng nối ở giao diện bất kì của Switch với giao diện PC và Router đã quy định sẵn trong *Bảng địa chỉ*. (Thường thì sẽ dùng giao diện switch từ nhỏ đến lớn, nếu như bài không có yêu cầu giao diện cụ thể)
- Các Router với nhau: dùng cáp serial DCE, với đầu có đồng hồ (hay được thể hiện trong topo là chữ DCE) thì khi gắn cáp sẽ gắn dây đó đầu tiên vào giao diện có chữ DCE, và đầu còn lại sẽ gắn vào giao diện Router còn lại.
### **II.** Cấu hình luận lý.
#### **1.** Cấu hình cơ bản cho Routers
>Như đã làm ở Lab 1 (Cấu hình thiết bị cơ bản), ta sẽ cấu hình hostname, enable secret, cấu hình console và virtual... (gọi tắt là cấu hình **global** cho Router)
### *1 số lệnh cơ bản khác*
`R1# no ip domain-lookup` : tránh đợi lâu khi đánh lệnh sai

`R1(config-line) logging synchronous` : tránh viết lệnh bị ngắt khi có thông báo cắt ngang của phần mềm. (Lệnh này được viết khi đang ở cấu hình console và virtual)

`R1(config-line): exec-timeout <minus> <second>` : đặt thời gian logout cho việc nhập lại passwd (mặc định là 10 phút).

>Cấu hình giao diện LAN cho các Router (xem lại Lab 1, nếu như bạn quên!)

>Cấu hình giao diện WAN cho các Router, tương tự như LAN nhưng có thêm 1 câu lệnh `clock rate 64000` (câu lệnh này chỉ dùng ở giao diện có đồng hồ hay có chữ DCE, và tùy bài ta sử dụng 64000 hay các thông số khác).

>Sau khi đã cấu hình cơ bản xong, dùng lệnh `show ip route` để xem bảng vạch đường hiện tại. Ta sẽ thấy bảng vạch đường của Router mới chỉ có thấy các đường kết nối trực tiếp, các kết nối khác vẫn chưa thấy.

![R1-show-ip-route](https://user-images.githubusercontent.com/93761311/236985142-daae19ed-0e20-4299-b7a2-fe1df20f9117.png)

#### **2.** Cấu hình địa chỉ IP cho PCs.
>Cấu hình địa chỉ IP cho các PC như đã cung cấp ở *Bảng địa chỉ*, có thể xem lại cách cấu hình ở Lab 1 nếu như quên!.

#### **3.** Kiểm tra kết nối.
**3.1.** Từ PC đến default gateway của nó --> **thành công**
![ping_PC_Gw](https://user-images.githubusercontent.com/93761311/236986051-dbbe0a4f-f59a-4f59-91bc-e2fe51f432cc.png)
**Nếu không thành công thì kiểm tra lại cáp và cấu hình 1 lần nữa*

**3.2.** Từ Router2 đến Router khác --> **thành công**
![ping_R2_R](https://user-images.githubusercontent.com/93761311/236986628-38236f94-ddaa-445d-bab9-641a9eeaf121.png)
**Nếu không thành công thì kiểm tra lại cáp và cấu hình 1 lần nữa*

**3.3.** Từ PC đến PC khác nhánh mạng và R1 đến R3 --> **không thành công**
![image](https://user-images.githubusercontent.com/93761311/236988100-8c678474-0e02-4bfc-9671-192145cc15a5.png)

**Không thành công là bởi vì các router chưa được cấu hình vạch đường, chỉ biết các kết nối trực tiếp cùng nhánh mạng, các kết nối nhánh mạng khác không biết, và khi gói tin đưa đến router và trong bảng vạch đường không thấy thì sẽ vứt gói tin đi*

#### **4.** Cấu hình vạch đường tĩnh.
**4.1.** Cấu hình tĩnh bằng địa chỉ router kế tiếp (Next-Hop).
`R1(config)# ip route <đ/c mạng đi đến> <mặt nạ mạng con đi đến> <đ/c IP giao diện Net-Hop> `

**4.2.** Cấu hình tĩnh bằng giao diện đi ra của router (Exit Interface).

`R1(config)# ip route <đ/c mạng đi đến> <mặt nạ mạng con đi đến> <tên giao diện đi ra> `

**Tên giao diện đi ra ví dụ như **Serial0/0/0***

**4.3.** Cấu hình tĩnh mặc định. (Cách này thường dùng nhất)
`R1(config)# ip route 0.0.0.0 0.0.0.0 <đ/c IP giao diện Net-Hop or interface name> `

**Câu lệnh này có nghĩa là đến nhánh mạng bất kì bằng địa chỉ IP hoặc giao diện của router biên*
#### **5.** Kiểm tra bảng vạch đường.
> Dùng lệnh `show ip route` để kiểm tra bảng vạch đường của từng router. Lúc này, các router đã có thể nhìn thấy tất cả các nhánh mạng có trong topo, có thể trao đổi thông tin các gói tin qua lại mà không bị vứt gói.

![show-ip-route-routed](https://user-images.githubusercontent.com/93761311/237002928-4baac9ea-98ee-4b69-b9ea-70f43020576b.png)

