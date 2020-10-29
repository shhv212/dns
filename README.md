# DNS  

## MỤC LỤC  

[1. DNS là gì? Cách thức hoạt động của DNS](#dnsandhowtowork)  
[2. Các loại bản ghi DNS](#banghidns)  
[3. Primary và secondary DNS, Round-Robin DNS, Reverse DNS](#typeofdns)  
[4. Thực hành](#thuchanh)  

*********************************

<a name="dnsandhowtowork"></a>  
### 1. DNS là gì? Cách thức hoạt động của DNS  

**DNS** (viết tắt là Domain Name System) là một máy chủ phân giải tên miền. Một cách dễ hiểu hơn là nó chuyển tên miền mà người dùng nhập vào thành địa chỉ IP tương ứng và ngược lại. Nó giúp máy tính và người dùng internet có thể giao tiếp với nhau vì người dùng chỉ cần nhớ tên miền mà không cần nhớ địa chỉ IP là dãy số phức tạp, việc còn lại là của DNS server.  
 
Cách hoạt động của DNS: 

<img src="https://i.imgur.com/ZUI93JV.png">  

Hình trên đây là cách mà `DNS server` hoạt động: Ví dụ như người dùng muốn truy cập tên miền `www.google.com` thì đầu tiên máy tính sẽ gửi một yêu cầu truy vấn địa chỉ IP tương ứng với tên miền đó đến `Local DNS server` (hay là `Internal DNS server`). `Local DSN server` sẽ tìm trong cơ sở dữ liệu của mình xem có tìm thấy thông tin về địa chỉ IP ứng với tên miền mà người dùng yêu cầu hay không. Nếu có nó sẽ trả về cho máy client thông tin này. Còn nếu không tìm thấy nó sẽ gửi yêu cầu truy vấn đến máy chủ ROOT server ( đây là máy chủ tên miền làm việc ở mức cao nhất). Khi đó máy chủ ROOT server sẽ thông báo cho máy chủ Local DNS server biết rằng máy chủ nào đang quản lý tên miền `.com`. Từ đó máy chủ Local DNS server sẽ tiếp tục gửi yêu cầu đến máy chủ Top Level Domain server về thông tin địa chỉ IP, máy chủ này cũng sẽ thông báo cho máy client này biết về máy chủ nào đang quản lý tên miền `google.com`. Dù bây giờ đã có địa chỉ IP của `google.com` nhưng dịch vụ web của máy client yêu cầu là dịch vụ www, máy chủ Local `DNS server` vẫn phải hỏi tiếp máy chủ `Name server` thông tin về dịch vụ www ứng với tên miền `google.com`. Tất nhiên `Name server` sẽ cho `Local DNS server` biết về địa chỉ IP của `www.google.com`. Lúc này việc cần làm `Local DNS server` gửi thông tin này về máy client của người dùng và họ sẽ sử dụng địa chỉ IP này để kết nối tới server của trang web chứa tên miền `www.google.com`.  

<a name="banghidns"></a>
### 2. Các loại bản ghi DNS  

**Bản ghi** dùng để lưu trữ thông tin dùng trong việc ánh xạ các tên miền và địa chỉ IP tương ứng với nó, xác định máy chủ sẽ phân giải tên miền này, ... Các loại bản ghi thường được chứa trong `DNS Zone` .  
 
Một số loại bản ghi thông dụng :  

- **SOA record** : đây chính là bản ghi lưu trữ các thông tin chính về domain của DNS server hay việc chuyển đổi giữa các DNS Zone. Chỉ có duy nhất một bản ghi này trong mỗi DNS Zone. Dưới đây là một ví dụ về phân tích bản ghi SOA :  

<img src="https://i.imgur.com/mwwE5h2.png">  

Trên ví dụ ta thấy có những thông số cần lưu ý sau đây:  

```
- Tên của máy chủ chính  
- Tên địa chỉ mail phản hồi  
- Thời gian cập nhật máy chủ DNS mới nhất (thường được sử dụng theo định dang yyyymmddss)  
- Số giây thực hiện cập nhật mỗi lần (15 phút)  
- Số giây thực hiện cập nhật bị lỗi và phải lấy lại bản ghi đã lưu trước đó cho lần cập nhật kế tiếp (15 phút)  
- Thời gian hết hạn của các bản ghi DNS và sau đó nó sẽ bị gỡ bỏ (30 phút)  
- TTL - đây là thời gian mà máy chủ DNS sẽ lưu giữ thông tin của bản ghi, nếu như TTL lớn thì nếu như thay đổi thông tin tên miền trên máy chủ DNS thì nó sẽ càng lâu bị cập nhật. (mặc định là 1 phút)  
``` 

- **Name Server record** : bản ghi này chỉ ta tên của các name server cho tên miền tương ứng. 

<img src="https://i.imgur.com/VKl0FXD.png">  

Trên đây ta thấy ứng với tên miền www.google.com có 4 name servers.  

- **A record** : đây là một record khá quan trọng nhằm ánh xạ và chuyển từ tên miền qua địa chỉ IP để có thể truy cập web.  

<img src="https://i.imgur.com/vjto6Zc.png">  

Trong ví dụ, tên miền google.com có địa chỉ IP là 172.217.26.142.  

- **CNAME record** : đây là bản ghi giúp nhiều tên miền khác nhau có thể trỏ cùng về một địa chỉ IP khác nhau.  

- **AAAA record** : đây là bản ghi sử dụng để chuyển đổi tên miền sang một địa chỉ IP 128-bit IPv6.  

<img src="https://i.imgur.com/2muQIco.png">  

- **MX record** : đây là bản ghi dùng để xác định được mail server cho một tên miền cụ thể.  

<img src="https://i.imgur.com/Z0bDHJV.png">  

Trên đây ta sẽ có chỉ số ưu tiên tương ứng với tên của mỗi mail server đối với tên miền `google.com`.  

- **PTR record** : đây là bản ghi dùng chuyển địa chỉ IP về tên miền tương ứng. 

<img src="https://i.imgur.com/6E82VAE.png">  

<a name="typeofdns"></a>  
### 3. Primary và secondary DNS, Round-Robin DNS, Reverse DNS  

- **Primary DNS** là nơi mà lưu trữ và quản lý chính thông tin về các tên miền, các tên miền sẽ được tạo, sửa đổi và cập nhật và sửa đổi ở đây, sau đó được sao lưu sang cho các Secondary DNS.  

- **Secondary DNS** là nơi lưu trữ phụ cho các `Primary DNS` khi bị quá tải hoặc khi `Primary DNS` bị chết thì `Secondary DNS` sẽ lên thay cho đến khi `Primary DNS` hoạt động trở lại. Không bắt buộc phải có nhưng khuyến khích các hệ thống nên sử dụng `Secondary DNS` .  

- **Round Robin DNS** là một phương pháp cân bằng tải dựa trên việc sắp xếp và lựa chọn thứ tự của các máy chủ theo một trình tự cố định. Việc này cũng giống như việc chúng ta chia công việc đảm nhận từ 1 server cho nhiều server khác nhau.  

Ví dụ: Một trang web có một tên miền duy nhất và chứa cùng một nội dung giống nhau trên 3 máy chủ khác nhau tương ứng với 3 địa chỉ IP khác nhau. Khi người dùng thứ nhất thực hiện truy cập đến trang web này thì yêu cầu nó sẽ được phân chia về cho máy chủ 1, yêu cầu người dùng thứ hai sẽ được để gửi về địa chỉ IP của máy chủ 2 và cuối cùng yêu cầu của người dùng thứ ba sẽ được gửi về máy chủ 3. Tương tự như vậy xoay vòng lại thì người dùng thứ tư khi gửi yêu cầu sẽ được gửi đến máy chủ 1.  

<img src="https://i.imgur.com/YiAzirx.png">  

- **Reserve DNS** là hệ thống phân giải tên miền ngược. Ngược lại hoàn toàn so với DNS server là phân giải tên miền là người dùng nhập vào thành địa chỉ IP thì Reverse DNS sẽ chuyển từ địa chỉ IP sang tên miền tương ứng.  

<a name="thuchanh"></a>  
### 4. Thực hành  

Trong ví dụ này ta sẽ sử dụng 2 máy ảo: 1 máy ảo Ubuntu dùng làm `DNS server` và 1 máy ảo CentOS để dùng làm `Client`. Trên máy ảo Ubuntu, ta cài đặt dịch vụ `Bind9` để cấu hình DNS server và một các zone cho domain thuctap.azdigi.com. Trên máy ảo CentOS ta sẽ trỏ DNS về địa chỉ IP của máy Master và thực hiện lookup để lấy được các record của domain thuctap.azdigi.com. Dùng tcmpdump hoặc wireshark để bắt các gói tin UDP về quá trình truy vấn DNS.  

Đầu tiên trên máy Master chúng ta sẽ cài đặt dịch vụ Bind9 trên đó. lưu ý cần cập nhật các ứng dụng và hệ thống trước khi cài đặt vì có thể phát sinh lỗi :  

<img src="https://i.imgur.com/5GhNs13.png">  

Sau đó chúng ta kiểm tra lại cấu hình mạng của hệ thống bằng lênh `ifconfig` trước khi cấu hình IP tĩnh:  

<img src="https://i.imgur.com/8uDH8Wp.png">  

Ta thấy địa chỉ hiện tại do DHCP cấp cho máy là `192.168.229.139`, tiến hành đặt lại IP tĩnh cho máy về IP `192.168.10.2` và gateway là `192.168.10.1` :

<img src="https://i.imgur.com/MXrAeev.png">  

Và đây là kết quả thu được sau đó:  

<img src="https://i.imgur.com/wYHcIE0.png">  

Sử dụng lệnh `ls` để liệt kê toàn bộ file đang hiện có trong thư mục `/etc/bind` vừa được cài đặt, ta mở file `named.conf.options` để sửa một số thông tin như bên dưới, mục đích chủ yếu là để forward tất cả những yêu cầu mà DNS server mà mình tạo ra này không thể phân giải được sang cho những DNS server khác:  

<img src="https://i.imgur.com/mC01CCu.png">  

<img src="https://i.imgur.com/OLEHwlW.png">  

Tiếp theo là mở file `named.conf.local` lên để thêm các zone cho domain `azdigi.com` vào đó:  

<img src="https://i.imgur.com/tkoHrST.png">  

<img src="https://i.imgur.com/Q3etNhA.png">  

Ở trên ta sử dụng 2 zone chứa 2 file database để lưu trữ những cặp địa chỉ IP và domain phục vụ cho việc truy vấn DNS và truy vấn DNS ngược.

Bây giờ ta thực hiện cấu hình cho 2 file db này đó là `db.forward.com` và `db.reverse.com`. Trước tiên là file `db.forward.com` :  

<img src="https://i.imgur.com/YqosB9c.png">  

<img src="https://i.imgur.com/kz73vnW.png">

Chúng ta tạo 3 sub domain cho domain `azdigi.com` đó là `thuctap.azdigi.com` , `hochiminh.azdigi.com` và `huusang.azdigi.com`.  

Đối với file `db.reverse.com`:  

<img src="https://i.imgur.com/3nVAbff.png">  

<img src="https://i.imgur.com/5uafgDq.png">  

Chủ yếu ở file forward chúng ta dùng `A record` còn ở file reverse dùng `PTR record`.

Bây giờ chúng ta sẽ thêm địa chỉ IP và domain của DNS server được tạo ra vào trong file `resolv.conf` vì trong file này liệt kê tất cả những DNS server có trong mạng của chúng ta:  

<img src="https://i.imgur.com/GBjCuvB.png">  

<img src="https://i.imgur.com/equVrK3.png">  

Cuối cùng khởi động lại dịch vụ Bind9:  

<img src="https://i.imgur.com/PSHMIGN.png">  

Đến đây chúng ta đã hoàn tất quá trình cài đặt DNS server cho máy Ubuntu, có thể thực hiện lookup như sau:  

<img src="https://i.imgur.com/K6ezWpk.png">  

Và thực hiện truy vấn ngược DNS hay còn gọi là `rDNS`, chuyển từ địa chỉ IP sang tên miền:  

<img src="https://i.imgur.com/OQgqrm6.png">  

Đến lúc này ta có thể dùng máy `Client` là máy CentOS được cấu hình một địa chỉ IP cùng mạng với DNS server và sử dụng địa chỉ DNS là địa chỉ của máy `Master` :

<img src="https://i.imgur.com/vdSJwRF.png">  

Từ máy Client có thể tra được các record của domain "azdigi.com" dựa vào lênh `lookup` :

<img src="https://i.imgur.com/nTsg4yZ.png">  

<img src="https://i.imgur.com/lBwZX8o.png">  

Ngay khi thực hiện truy vấn DNS, có thể dùng tcpdump để bắt các gói tin UDP:  

<img src="https://i.imgur.com/Q0lKqRr.png">  

Kết quả:  

<img src="https://i.imgur.com/JodBGVW.png">  

<img src="https://i.imgur.com/k4t3tT6.png">  

Chúng ta có thể sử dụng wireshark để thấy rõ hơn các gói tin UDP được gửi như thế nào:  

<img src="https://i.imgur.com/DNahX6m.png">  

Ta có thể thấy rõ các gói tin request từ phía `Client` gửi thông tin tên miền đến máy `Master` và được DNS server trả lại kết quả là địa chỉ IP tương ứng.