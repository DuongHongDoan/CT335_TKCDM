# **Lab 1: Cấu hình thiết bị Cisco cơ bản**
## **Topology**
---
![topology_Lab1](https://user-images.githubusercontent.com/93761311/219910346-3779e290-8334-4c94-8db5-759c7759d2d3.PNG)

## **Các thành phần trong sơ đồ mạng**
---
| Phần cứng         | Số lượng  | Mô tả                             |
| :------------     |:---------:| :-----                            |
| Router (1841)     | 1         |Thiết bị kết nối nhiều nhánh mạng  |
| Switch (2950-24)  | 1         |Thiết bị kết nối các thành phần trong cùng mạng|
| PC                | 3         |Các máy tính - thiết bị đầu cuối   |
| Cáp console       |1          |Kết nối PC1 (cổng RS) với Router (cổng console|
| Cáp chéo          |1          |Kết nối PC1 (cổng Fa) với Router (cổng Fa0/0) |
| Cáp thẳng         |3          |Kết nối PC2 và PC3 (cổng Fa) với Switch (cổng Fa0/2, Fa0/3) và Switch (cổng Fa0/1) với Router (cổng Fa0/1)  |
## **Kịch bản**
---
>Cho địa chỉ IP là **198.133.219.0/24**, với **4 bit** mượn cho mạng con.

>Số lượng mạng con có thể sử dụng tối đa:  **$2^4$ = 16**.

>Số lượng máy chủ có thể sử dụng trên mỗi mạng con: **$2^4 - 2$ = 14**.

>Do ta mượn 4 bit phần host để làm mạng con -> địa chỉ IP ta có dạng **198.133.219.0/28**, tương đương với mặt nạ mạng con là **255.255.255.240** (11111111.11111111.11111111.<span style="color: red">1111</span>0000)

### *<span style="color: yellow">Bảng địa chỉ</span>*
---
![IP_Address_Table](https://user-images.githubusercontent.com/93761311/219934973-e508a411-c13b-4b2a-9d02-6bae87de0822.PNG)

##### *<span style="color: red">\*Các địa chỉ IP đầu tiên và cuối cùng trong một subnet, cách làm tương tự như 3 dòng đầu đã làm mẫu</span>*
## **Bài làm**
---
### **1.** Định cấu hình ***global*** cho Router 
#### **1.1.** Kết nối vật lý
>Kết nối các cáp console, cáp chéo, cáp thẳng theo **Mô tả** trong phần **Các thành phần trong sơ đồ mạng**
#### **1.2.** Kết nối PC1 với Router qua Terminal
![Desktop_PC1](https://user-images.githubusercontent.com/93761311/219912187-c308afd1-fa42-48f6-a813-e0699a8d0f89.PNG)

>Nhấp vào **PC1** -> Chọn **Desktop** -> Chọn **Terminal**.

![Terminal](https://user-images.githubusercontent.com/93761311/219913003-714d796c-a3f7-431e-a8e0-63b203f8d82f.PNG)

>Chọn các thông số như hình -> Nhấn **OK**

![CLI_Terminal](https://user-images.githubusercontent.com/93761311/219913923-f4726225-1424-45f7-bbbc-0897f009b067.PNG)

>Hiện ra giao diện dòng lệnh -> Nhập **no** -> Nhấn **Enter**

>Chọn **no** để cho ta sẽ ở chế độ cấu hình bằng dòng lệnh, thay vì sử dụng giao diện (GUI) để cấu hình.

>Sau khi chúng ta thực hiện các dòng trên, thì bây giờ chúng ta đang ở *chế độ thực thi người dùng* (User EXEC mode), Đây là chế độ "chỉ xem", chỉ có một số lệnh cơ bản để xem cấu hình của router. Với dấu nhắc lệnh là "**>**".

>Để vào *chế độ đặc quyền* (Privilleged EXEC mode), ta sử dụng lệnh **enable**. Đây là chế độ dùng để cấu hình, giám sát và quản lý router -> nó rất quan trọng nên cần phải được bảo mật. Với dấu nhắc lệnh là "**#**".

![privilleged_EXEC_mode](https://user-images.githubusercontent.com/93761311/219917177-e3262dc1-49b4-41cc-9ff0-7ef6c40522a9.PNG)

#### **1.3.** Cấu hình tên router trong cấu hình ***global***
>Tại chế độ *privilleged EXEC* ta nhập lệnh "**config terminal**" hoặc câu lệnh rút gọn "**conf t**", để vào chế độ *global configuration*. Dấu nhắc lệnh là "**Router(config)#**"

![global_conf](https://user-images.githubusercontent.com/93761311/219919145-cc4c1edb-352b-48dd-b72e-f84543152726.PNG)

>Định cấu hình tên cho router là Router1, với lệnh: 
`hostname <hostname>`

![hostname](https://user-images.githubusercontent.com/93761311/219920664-f76416ff-7cba-435c-9c4c-08658fa87bc5.PNG)

>Chúng ta sẽ thấy có sự thay đổi ngay lập tức khi ta nhấn **Enter**, bởi vì đây là chương trình thông dịch nên khi mỗi lần Enter thì câu lệnh phía trên sẽ đc thực hiện ngay lập tức. Và khi thực hiện câu lệnh đặt tên cho router, thì router đã chuyển sang "**Router1(config)#**"

#### **1.4.** Cấu hình biểu ngữ (banner) MOTD
>MOTD banner là chứa nội dung pháp lý của một tổ chức về ủy quyền và hình phạt đối với các truy cập trái phép, ghi nhật ký kết nối và luật hiện hành.

>MOTD banner được hiển thị đầu tiên trước dấu nhắc đăng nhập.

>Sử dụng câu lệnh sau để đặt banner :
`banner motd % <TEXT message> %`

![banner](https://user-images.githubusercontent.com/93761311/219922065-445a3139-7ef3-4b37-902d-c43e6bf04d45.PNG)

### **2.** Cấu hình mật khẩu truy cập trên Router1
>Mật khẩu truy cập được cài đặt ở chế độ *privileged EXEC*. Mật khẩu sẽ được đặt ở những điểm vào của người dùng như : console, AUX, vty (virtual lines) và đặc biệt là chế độ *privileged EXEC*, vì nó kiểm soát quyền truy cập vào chế độ cấu hình.
#### **2.1.** Cấu hình mật khẩu chế độ *privileged EXEC*
>Cisco hỗ trợ 2 câu lệnh để cài đặt mật khẩu truy cập chế độ privileged EXEC : `enable password <passsword>` -> mã hóa yếu, dễ bị đánh cắp, và `enable secret <password>` -> an toàn hơn, vì nó sử dụng giải thuật băm MD5, mã hóa tốt, tránh bị đánh cắp.

>Sử dụng lệnh sau để cấu hình mật khẩu vào chế độ *privileged EXEC* :
```enable secret <password>```

![pw_privileged](https://user-images.githubusercontent.com/93761311/219932765-0489f598-e5d5-4abd-a77e-543f4ce22cd7.PNG)
#### **2.2.** Cấu hình mật khẩu *console*
>Console là cổng quản lý, được sử dụng để kết nối trực tiếp hoặc qua USB tốc độ thấp với host, để cấp quyền truy cập bên ngoài băng tần (out-of-Band). Sử dụng các dòng lệnh sau:
```
line console 0
password <passwd>
login
```
![pw_console](https://user-images.githubusercontent.com/93761311/219933038-ed47f060-c776-481f-a64d-5cf733158aa6.PNG)
#### **2.3.** Cấu hình mật khẩu *virtual lines (vty)* - đường truyền ảo
>Virtual lines là các đường truyền ảo kiểm soát quyền truy cập Telnet đến router. Chỉ đc đặt tối đa 5 đường ảo (0-4), nếu không đặt mật khẩu cho Telnet thì mặc định quyền truy cập trên đường truyền ảo đó bị chặn. Sử dụng các dòng lệnh sau để cấu hình:
```
line vty 0 4
password <passwd>
login
```
![pw_vty](https://user-images.githubusercontent.com/93761311/219933541-cafb0090-a91f-472d-8bb7-1103849460f0.PNG)
### *<span style="color: red">\*Lưu ý :</span>*
>Để trở lại chế độ trước đó, ta dùng lệnh `exit`

>Để trở lại chế độ *privileged EXEC*, ta dùng lệnh `end` hoặc tổ hợp phím "**Ctrl+Z**"

### **3.** Cấu hình các giao diện trên Router1
>Từ **Bảng địa chỉ**, chúng ta sẽ lấy "subnet0" là nhánh mạng **198.133.219.0/28** để đặt nhánh mạng cho kết nối của PC1 với Router, như vậy ta có:
- Địa chỉ IP đầu tiên trong nhánh mạng sẽ đc đặt cho PC1: **198.133.219.1**
- Địa chỉ IP cuối cùng trong nhánh mạng sẽ được đặt cho giao diện Fa0/0 của Router1: **198.133.219.14**
>Chúng ta sẽ lấy "subnet1" là nhánh mạng **198.133.219.16/28** để đặt nhánh mạng cho kết nối của Switch với Router. Địa chỉ IP cuối trong nhánh mạng sẽ được đặt cho giao diện Fa0/1 của Router1: **198.133.219.30** 
#### **3.1.** Cấu hình cho giao diện Fa0/0 của Router1
>Vào giao diện Fa0/0 -> Viết một mô tả cho giao diện -> Đặt địa chỉ IP và mặt nạ mạng cho giao diện -> Bật giao diện lên.

![Fa00_Config](https://user-images.githubusercontent.com/93761311/219935831-6bdc7bc1-925b-4a86-9357-fd05c23f0c8f.PNG)
#### **3.2.** Cấu hình cho giao diện Fa0/1 của Router1
>Vào giao diện Fa0/1 -> Viết một mô tả cho giao diện -> Đặt địa chỉ IP và mặt nạ mạng cho giao diện -> Bật giao diện lên.

![Fa01_Config](https://user-images.githubusercontent.com/93761311/219936132-2a4cb966-b977-4673-877f-6eddab444b90.PNG)
##### *<span style="color: red">\*Câu lệnh `int Fa0/1` là câu lệnh viết tắt của `interface Fa0/1`.</span>*
#### **3.3.** Cấu hình cho PC1 (cổng Fa)
![Desktop_PC1](https://user-images.githubusercontent.com/93761311/219912187-c308afd1-fa42-48f6-a813-e0699a8d0f89.PNG)

>Nhấp vào **PC1** -> Chọn **Desktop** -> Chọn **IP Configuration**.

![Config_IP_PC1](https://user-images.githubusercontent.com/93761311/219936509-cf4ffeb2-ddcc-4734-bd9e-59b0e8789413.PNG)
>Tại mục **IP Configuration** -> Chọn **Static** -> Điền các thông số như hình -> Thoát.
#### **3.4.** Kiểm tra kết nối mạng.
![Desktop_PC1](https://user-images.githubusercontent.com/93761311/219912187-c308afd1-fa42-48f6-a813-e0699a8d0f89.PNG)

>Nhấp vào **PC1** -> Chọn **Desktop** -> Chọn **Command Prompt**.

![ping_PC1toRouter](https://user-images.githubusercontent.com/93761311/219936759-88cdf3eb-f5b7-4b9c-8d1f-f4c5d9d1e4cd.PNG)
> Tại giao diện dòng lệnh thực hiện câu lệnh `ping 198.133.219.14` (ping giao diện của Router1 - cổng mặc định). Nếu kết quả như hình là thành công. Nếu kết quả là "**Request timeout**" thì kiểm tra lại thông số cấu hình.

### **4.** Lưu file cấu hình của Router1
>Cấu hình RAM là lưu trữ **running-configuration** và bộ nhớ cấu hình NVRAM là lưu trữ **startup-configuration**. Để các cấu hình tồn tại khi khởi động lại hoặc khởi động lại nguồn, cấu hình RAM phải được sao chép vào RAM không biến đổi (NVRAM). Điều này không xảy ra tự động, NVRAM phải được cập nhật thủ công sau khi thực hiện bất kỳ thay đổi nào.

#### **4.1.** Xem nội dung RAM.
>Hiển thị cấu hình RAM, ta sử dụng câu lệnh sau: `show running-config` (lệnh này được chạy ở chế độ *privileged EXEC*)

![run-config](https://user-images.githubusercontent.com/93761311/219937294-94e74d7f-ab0f-4383-9298-5b955e6e8035.PNG)

>Có dòng chứa "--More--", do thông tin hiển thị quá dài nếu muốn xem tiếp thông tin ta có thể nhấn **Enter** để xem tiếp. 

#### **4.2.** Lưu cấu hình RAM vào NVRAM.
>Lưu cấu hình RAM vào NVRAM để lưu trữ cấu hình cho lần thực hiện sau, khi router được bật nguồn, tránh cấu hình lại. Sử dụng câu lệnh `copy running-config startup-config`

![copy_RAMtoNVRAM](https://user-images.githubusercontent.com/93761311/219937709-acb95b3c-6028-4577-a7d7-ffdc88ddfede.PNG)

### **5.** Cấu hình Switch
>Cấu hình switch cũng tương tự như cấu hình router. Chỉ là không đặt địa chỉ IP trên các cổng giao diện, trừ trường hợp cần thiết.
#### **5.1** Kết nối vật lý.
>Nối ba dây cáp thẳng ở ba cổng của switch với PC2, PC3 và Router1, theo **Mô tả** trong phần **Các thành phần trong sơ đồ mạng**.
#### **5.2** Cấu hình tên switch trong cấu hình ***global***
>Nhấp vào Switch1 -> Chọn tab **CLI** -> Nhấn **Enter** -> chuyển sang chế độ *user EXEC*.

![CLI_Switch](https://user-images.githubusercontent.com/93761311/219938363-252d3e14-cc8d-4c62-bd2e-f832dd2c10b0.PNG)

>Tại chế độ *User EXEC* ta nhập lệnh `enable` hoặc `en`, để vào chế độ *privileged EXEC*. Dấu nhắc lệnh là "**Router#**"

>Tại chế độ *privilleged EXEC* ta nhập lệnh `config terminal` hoặc câu lệnh rút gọn `conf t`, để vào chế độ *global configuration*. Dấu nhắc lệnh là "**Router(config)#**"

![userToprivileged](https://user-images.githubusercontent.com/93761311/219938495-3305e4d5-ea29-4473-a754-a66807a87ddf.PNG)

>Cấu hình tên cho Switch

![hostname_Sw](https://user-images.githubusercontent.com/93761311/219938584-e3e03531-55ad-426f-9fd4-de50748961ef.PNG)
#### **5.3** Cấu hình biểu ngữ MOTD
![banner_switch](https://user-images.githubusercontent.com/93761311/219938715-c7805195-9f36-4982-abd1-7c99b09ba80d.PNG)
#### **5.4** Cấu hình mật khẩu chế độ *privileged EXEC*
![passwd_privi_Sw](https://user-images.githubusercontent.com/93761311/219938862-00e56a5d-cede-4b87-bf6a-2dd6c16b0099.PNG)
#### **5.5** Cấu hình mật khẩu console
![passwd_console_Sw](https://user-images.githubusercontent.com/93761311/219938944-b6af268c-189c-4f84-96f8-8c8452cd4af8.PNG)
#### **5.6** Cấu hình mật khẩu virtual lines (vty)
>Trên switch có 16 đường truyền ảo, nên sẽ cấu hình vty từ 0-15

![passwd_vty_Sw](https://user-images.githubusercontent.com/93761311/219939097-0fe27648-783a-4ef1-afc2-9c18ebf79aad.PNG)
#### **5.7** Cấu hình mô tả giao diện kết nối của switch.
![Sw_interface_Config](https://user-images.githubusercontent.com/93761311/219939308-8f5f35c0-8339-484e-96e7-e85e8afb999d.PNG)
#### **5.8** Lưu cấu hình RAM vào NVRAM
![copy_RAMtoNVRAM_Switch](https://user-images.githubusercontent.com/93761311/219939428-5e6bf1ee-2b79-460b-8935-8243443b3ae8.PNG)
#### **5.9** Cấu hình địa chỉ IP cho PC2, PC3.
>PC2 và PC3 nằm trên cùng nhánh mạng **198.133.219.16/28**, nên địa chỉ IP lần lượt của hai PC được cấp là **198.133.219.17** và **198.133.219.18**. Địa chỉ cổng mặc định (default gateway) của hai PC, chính là giao diện Fa0/1 của Router1 **198.133.219.30**
>Làm cấu hình tương tự như PC1.
#### **5.10** Kiểm tra kết nối mạng.
- ping từ PC2 qua PC3:
![ping_PC2toPC3](https://user-images.githubusercontent.com/93761311/219939845-cb94b99f-114c-4c41-94dc-52928fd231b5.PNG)

- ping từ PC2 qua PC1:
![ping_PC2toPC1](https://user-images.githubusercontent.com/93761311/219939859-be01f6b5-3659-4a4c-817c-0927f74c3545.PNG)
