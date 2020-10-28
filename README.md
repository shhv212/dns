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
###. 2. Các loại bản ghi DNS  



