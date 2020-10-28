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






