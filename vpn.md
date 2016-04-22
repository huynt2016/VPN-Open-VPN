# VPN-OpenVPN
## A.VPN
### I.Tổng quan
#### 1.Khái niệm
- VPN (Virtual Private Network) là một công nghệ xây dựng một mạng riêng ảo trong môi trường mạng công cộng nhằm đáp ứng nhu cầu chia sẻ thông tin, truy cập từ xa.

#### 2.Chức năng
- Tính xác thực: Để thiết lập một kết nối VPN thì trước hết cả hai phải xác thực lẫn nhau để khẳng định rằng mình đang trao đổi thông tin với người mình mong muốn chứ không phải là một người khác
- Tính toàn vẹn: Đảm bảo dữ liệu không bị thay đổi hay có bất kỳ sự xáo trộn nào trong quá trình truyền dẫn.
- Tính bảo mật: Người gửi có thể mã hóa các gói dữ liệu trước khi truyền qua mạng công cộng và dữ liệu sẽ được giải mã ở phía thu. Bằng cách như vậy, không một ai có thể truy nhập thông tin mà không được phép. Thậm chí nếu có lấy được thì cũng không đọc được.

### II.Kỹ thuật VPN
#### 1.Yêu cầu
- Tính bảo mật dữ liệu: Bảo vệ nội dung bản tin không bị nhìn thấy hay dịch ra từ nguồn không rõ ràng hay không có quyền.
- Tính toàn vẹn dữ liệu: Đảm bảo những nội dung bản tin không bị thay đổi khi đi từ nguồn đến đích.
- Tính không thoái thác: Xác nhận bản tin đã được gửi và gửi đến người nhận.
- Chứng thực bản tin: Đảm bảo bản tin được gửi từ nguồn xác thực và được gửi tới đích xác thực.

#### 2.Kỹ thuật tạo đường hầm (Tunneling)
##### a.Khái niệm đường hầm
- Là một phương thức sử dụng hạ tầng mạng có sẵn để chuyển dữ liệu từ một mạng này sang mạng khác mà trong đó dữ liệu được đảm bảo tính riêng tư và có thể kiểm soát được quá trình truyền.
- Hầu hết các VPN đều dựa vào kỹ thuật Tunneling để tạo một mạng riêng trên nền tảng Internet. Về bản chất, đây là quá trình đặt gói tin vào một lớp header chứa thông tin định tuyến có thể truyền qua hệ thống mạng trung gian thưo những đường ống riêng. Đường đi logic của gói tin trên mạng tới đích được gọi là Tunnel.

##### b.Các giao thức
- Kỹ thuật Tunneling yêu cầu 3 giao thức khác nhau:
  <ul>
  <li>Giao thức truyền tải (Carrier Protocol) là giao thức được sử dụng bởi mạng có thông tin đang đi qua.</li>
  <li>Giao thức mã hóa dữ liệu (Encapsulating Protocol) là giao thức được bọc quanh gói dữ liệu gốc (như IPSec, L2F, PPTP, L2TP).</li>
  <li>Giao thức gói tin (Passenger Protocol) là giao thức của dữ liệu gốc được truyền đi (như IPX, NetBeui, IP).</li>
  </ul>
- Để thiết lập một tunnel, cả hai phía client và server phải sử dụng cùng một giao thức. Kỹ thuật tunnel có thể dựa vào các giao thức tunnel ở lớp 2 (data-link), lớp 3 (network) hoặc lớp 4 (transport)
  <ul>
  <li>Kỹ thuật VPN lớp 2 gồm có: L2F, PPTP, L2TP.</li>
  <li>Kỹ thuật VPN lớp 3 gồm có: IPSec, L2TPv3, MPLS</li>
  <li>Kỹ thuật VPN lớp 4 gồm có: SSL, TLS</li>
  </ul>

<img src="http://i.imgur.com/8AoaBVw.png">

##### c.Giao thức kết nối Điểm tới Điểm (Point to Point Protocol - PPP)

- Trong mạng máy tính , Point-to-Point Protocol ( PPP ) là một liên kết dữ liệu giao thức dùng để thiết lập một kết nối trực tiếp giữa hai nút .Nó có thể cung cấp kết nối xác thực , truyền mã hóa (sử dụng ECP , RFC 1968 ) và nén .
- Trong quá trình cấu hình, duy trì và chấm dứt liên kết, PPP đi qua một vài giai đoạn được quy định trong sơ đồ trạng thái sau đây:

<img src="http://i.imgur.com/nxF4y70.png">

  <ul>
  <li>*Link Dead*(Lớp vật lý không sẵn sàng): Xảy ra khi liên kết thất bại hoặc một bên ngắt kết nối</li>
  <li>*Link Establishment Phase*(Giai đoạn thiết lập liên kết): Giai đoạn này Link Control Protocol(LCP) được sử dụng để thiết lập kết nối thông qua trao đổi các gói tin Configure. Trao đổi hoàn thành khi gói tin Configure-Ack được cả hai bên gửi và nhận.</li>
  <li>*Authentication*(Giai đoạn xác thực): Giai đoạn này là tùy chọn. Nó cho phép các bên để xác nhận lẫn nhau trước khi kết nối được thiết lập. Nếu thành công, điều khiển đi đến giai đoạn giao thức lớp mạng.</li>
  <li>*Network-Layer Protocol Phase*: Giai đoạn này là nơi Network Control Protocol (NCP) được gọi. Đóng cửa các giao thức mạng cũng xảy ra trong giai đoạn này.</li>
  <li>*Link Termination Phase*(Giai đoạn chấm dứt): Giai đoạn này đã đóng cửa các kết nối. Điều này có thể xảy ra nếu có một lỗi xác thực, có quá nhiều lỗi checksum mà hai bên quyết định phá bỏ liên kết tự động, các liên kết bất ngờ thất bại, hoặc một bên quyết định để treo kết nối.</li>
  </ul>
  
##### d.Giao thức Layer 2 Forwarding (L2F)
- Giao thức này được phát triển đầu tiên bởi Cissco để tạo một tunnel cho các giao thức IP, Apple Talk và IPX thông qua kết nối PPP.
- Cơ chế: L2F tập trung những tunnel vào một home-gateway. L2F sử dụng những bản tin UDP port 1701 để thiết lập kết nối. Khi một tunnel được thiết lập thì những gói tin đã được đóng gói theo L2F được truyền đi song song với dữ liệu điều khiển L2F.	
- Quá trình tạo tunnel L2F như sau: Ban đầu một số User quay số tới Network Access Server (NAS), đàm phán PPP và đc NAS chứng thực với giao thức Password Authentication Protocol (PAP) hoặc Challenge Handshake Authentication Protocol (CHAP). Nếu hợp lệ nó sẽ thiết lập một tunnel đến gateway router của mạng riêng cần truy cập. Gateway này có nhiệm vụ kiểm tra tính hợp lệ của thông tin phía người dùng cung cấp, sau đó cấp địa chỉ và quyền truy cập cho máy ở xa.
- L2F hiện nay sử dụng phương thức mã hóa dựa trên Internet Protocol Security (IPsec) để đảm bảo an toàn dữ liệu trong quá trình chuyển đi.
- Cấu trúc của gói L2F

<img src="http://i.imgur.com/xdblEHO.png">

- Trong đó:
  <ul>
  <li>F: Trường “Offset” có mặt nếu bit này được thiết lập</li>
  <li>K: Trường “Key” có mặt nếu bit này được thiết lập</li>
  <li>P (Priority): Gói này là một gói ưu tiên nếu bit này được thiết lập</li>
  <li>S: Trường “Sequence” có mặt nếu bit này được thiết lập</li>
  <li>Reserved: luôn được đặt là 00000000</li>
  <li>Version: Phiên bản chính của L2F dùng để tạo gói. 3 bit luôn là 111</li>
  <li>Protocol: Xác định giao thức đóng gói L2F</li>
  <li>Sequence: Số chuỗi được đưa ra nếu trong L2F Header bit S=1</li>
  <li>Multiplex ID: nhận dạng một kết nối riêng trong một đường hầm (tunnel)</li>
  <li>Client ID: Giúp tách đường hầm tại những điểm cuối</li>
  <li>Length: chiều dài của gói (tính bằng byte) không bao gồm phần checksum</li>
  <li>Offset: Xác định số byte trước L2F header, tại đó dữ liệu tải tin được bắt đầu.</li>
  <li>Trường này có khi bit F=1</li>
  <li>Key: Trường này được trình bày nếu bit K được thiết lập. Đây là một phần của quá trình nhận xác thực</li>
  <li>Checksum: Kiểm tra tổng của gói. Trường checksum có nếu bit C=1</li>
  </ul>

##### e.Giao thức Point to Point Tunneling protocol (PPTP)
- Giao thức PPTP được xây dựng dựa trên chức năng của PPP, cung cấp khả năng quay số truy cập tạo ra một đường hầm bảo mật thông qua Internet đến site đích.
- PPTP sử dụng giao thức bọc gói định tuyến chung GRE (Generic Routing Encapsulation) được mô tả lại để đóng gói và tách gói PPP, giao thức này cho phép PPTP mềm dẻo xử lý các giao thức khác không phải IP như: IPX, NETBEUI.
- Do PPTP dựa trên PPP nên nó cũng sử dụng PAP, CHAP để xác thực. PPTP có thể sử dụng PPP để mã hoá dữ liệu nhưng Microsoft đã đưa ra phương thức mã hoá khác mạnh hơn đó là mã hoá điểm – điểm MPPE (Microsoft Point- to- Point Encryption) để sử dụng cho PPTP.
- Một ưu điểm của PPTP là được thiết kế để hoạt động ở lớp 2 (lớp liên kết dữ liệu) trong khi IPSec chạy ở lớp 3 của mô hình OSI. Bằng cách hỗ trợ việc truyền dữ liệu ở lớp thứ 2, PPTP có thể truyền trong đường hầm bằng các giao thức khác IP trong khi IPSec chỉ có thể truyền các gói IP trong đường hầm.
- Cấu trúc của gói PPTP

<img src="http://i.imgur.com/52MaQ96.png">

- Đóng gói khung PPP
Phần tải PPP ban đầu được mật mã và đóng gói với phần tiêu đề PPP để tạo ra khung PPP. Sau đó, khung PPP được đóng gói với phần tiêu đề của phiên bản sửa đổi giao thức GRE.
Đối với PPTP, phần tiêu đề của GRE được sử đổi một số điểm sau:
  <ul>
  <li>Một bit xác nhận được sử dụng để khẳng định sự có mặt của trường xác nhận 32 bit</li>
  <li>Trường Key được thay thế bằng trường độ dài Payload 16bit và trường nhận dạng cuộc gọi 16 bit. Trường nhận dạng cuộc goi Call ID được thiết lập bởi PPTP client trong quá trình khởi tạo đường hầm PPTP</li>
  <li>Một trường xác nhận dài 32 bit được thêm vào</li>
  <li>GRE là giao thức cung cấp cơ chế chung cho phép đóng gói dữ liệu để gửi qua mạng IP</li>
  </ul>
- Đóng gói các gói GRE, phần tải PPP đã được mã hoá và phần tiêu đề GRE được đóng gói với một tiêu đề IP chứa thông tin địa chỉ nguồn và đích cho PPTP client và PPTP server.
- Đóng gói lớp liên kết dữ liệu: Do đường hầm của PPTP hoạt động ở lớp 2 – Lớp liên kết dữ liệu trong mô hình OSI nên lược đồ dữ liệu IP sẽ được đóng gói với phần tiêu đề (Header) và phần kết thúc (Trailer) của lớp liên kết dữ liệu. Ví dụ, Nếu IP datagram được gửi qua giao diện Ethernet thì sẽ được đóng gói với phần Header và Trailer Ethernet. Nếu IP datagram được gửi thông qua đường truyền WAN điểm tới điểm thì sẽ được đóng gói với phần Header và Trailer của giao thức PPP.

##### f.Giao thức Layer 2 Tunneling Protocol (L2TP)
- Giao thức đường hầm lớp 2 L2TP là sự kết hợp giữa hai giao thức PPTP và L2F- chuyển tiếp lớp 2.
- Giống như PPTP, L2TP là giao thức đường hầm, nó sử dụng tiêu đề đóng gói riêng cho việc truyền các gói ở lớp 2. Một điểm khác biệt chính giữa L2F và PPTP là L2F không phụ thuộc vào IP và GRE, cho phép nó có thể làm việc ở môi trường vật lý khác.
- L2TP định nghĩa riêng một giao thức đường hầm dựa trên hoạt động của L2F. Nó cho phép L2TP truyền thông qua nhiều môi trường gói khác nhau như X.25, Frame Relay, ATM.
- Do L2TP là giao thức ở lớp 2 nên nó cho phép người dùng sử dụng các giao thức điều khiển một cách mềm dẻo không chỉ là IP mà có thể là IPX hoặc NETBEUI. Cũng giống như PPTP, L2TP cũng có cơ chế xác thực PAP, CHAP hay RADIUS.
- Cấu trúc gói L2TP

<img src="http://i.imgur.com/9bNG8bb.png">

- Đóng gói L2TP: Phần tải PPP ban đầu được đóng gói với một PPP header và một L2TP header.
- Đóng gói UDP: Gói L2TP sau đó được đóng gói với một UDP header, các địa chỉ nguồn và đích được đặt bằng 1701.
- Đóng gói IPSec: Tuỳ thuộc vào chính sách IPSec, gói UDP được mật mã và đóng gói với ESP IPSec header và ESP IPSec Trailer, IPSec Authentication Trailer.
- Đóng gói IP: Gói IPSec được đóng gói với IP header chứa địa chỉ IP ngưồn và đích của VPN client và VPN server.
- Đóng gói lớp liên kết dữ liệu: Do đường hầm L2TP hoạt động ở lớp 2 của mô hình OSI- lớp liên kết dữ liệu nên các IP datagram cuối cùng sẽ được đóng gói với phần header và trailer tương ứng với kỹ thuật ở lớp đường truyền dữ liệu của giao diện vật lý đầu ra.

##### g.Giao thức bảo mật IP – Internet Protocol Security (IPSec)
- IPSec không phải là một giao thức. Nó là một khung của các tập giao thức chuẩn mở cho phép những nhà quản trị mạng lựa chọn thuật toán, các khoá và phương pháp nhận thực để cung cấp sự xác thực dữ liệu, tính toàn vẹn dữ liệu, và sự tin cậy dữ liệu.
- IPSec là sự lựa chọn cho bảo mật tổng thể các VPN, là phương án tối ưu cho mạng của công ty. Nó đảm bảo truyền thông tin cậy trên mạng IP công cộng đối với các ứng dụng.
- IPsec tạo những đường hầm bảo mật xuyên qua mạng Internet để truyền những luồng dữ liệu. Mỗi đường hầm bảo mật là một cặp những kết hợp an ninh để bảo vệ luồng dữ liệu giữa hai Host.
- Khung giao thức IPSEC

<img src="http://i.imgur.com/fS4CIUE.png">

	Một số giao thức chính được khuyến khích sử dụng khi làm việc với IPSec.
– Giao thức bảo mật IP (IPSec)
  <ul>
  <li>AH (Authentication Header)</li>
  <li>ESP (Encapsulation Security Payload)</li>
  </ul>
– Mã hoá bản tin
  <ul>
  <li>DES (Data Encryption Standard)</li>
  <li>DES (Triple DES)</li>
  </ul>
– Các chức năng toàn vẹn bản tin
  <ul>
  <li>HMAC (Hash – ased Message Authentication Code)</li>
  <li>MD5 (Message Digest 5)</li>
  <li>SHA-1 (Secure Hash Algorithm -1)</li>
  </ul>
– Nhận thực đối tác (peer Authentication)
  <ul>
  <li>Rivest, Shamir, and Adelman (RSA) Digital Signatures</li>
  <li>RSA Encrypted Nonces</li>
  </ul>
– Quản lý khoá
  <ul>
  <li>DH (Diffie- Hellman)</li>
  <li>CA (Certificate Authority)</li>
  </ul>
– Kết hợp an ninh
  <ul>
  <li>IKE (Internet Key Exchange)</li>
  <li>ISAKMP (Internet Security Association and Key Management Protocol)</li>
  </ul>
  
##### h.Kỹ thuật Transport Layer Security (TLS)
- TLS cung cấp thêm lớp bảo mật tại lớp Transport trong mô hình OSI.
- Giữa Client và Server sẽ gửi cho nhau các gói tin bao gồm các mã khóa và thuật toán biến đổi (Như DES hay 3DES)
- TLS Handshake Protocol

<img src="http://i.imgur.com/6IPaS6L.png">

##### i.Kỹ thuật Secure Socket Layer (SSL)
- SSL không phải là một giao thức đơn mà là 2 lớp giao thức, như minh họa dưới đây

<img src="http://i.imgur.com/HWcG38Y.png">

- Giao thức SSL Record: SSL Record Protocol cung cấp 2 dịch vụ cho kết nối SSL
  <ul>
  <li>*Confidentiality*(Tính cẩn mật): Handshake Protocol định nghĩa 1 khóa bí mật được chia sẻ, khóa này được sử dụng cho mã hóa quy ước các dữ liệu SSL.</li>
  <li>*Message Integrity*(Tính toàn vẹn thông điệp): Handshake Protocol cũng định nghĩa 1 khóa bí mật được chia sẻ, khóa này được sử dụng để hình thành MAC (mã xác thực message)</li>
  <li>Hoạt động của SSL Record Protocol cụ thể được minh họa trong hình sau:</li>
  </ul>

<img src="http://i.imgur.com/TqCY4It.png">

- Giao thức SSL Change Cipher Spec: Đây là một giao thức khá đơn giản. Nó bao gồm một message đơn 1 byte giá trị là 1, với mục đích là sinh ra trạng thái tiếp theo để gán vào trạng thái hiện tại và trạng thái hiện tại cập nhật lại bộ mã hóa để sử dụng trên kết nối này

<img src="http://i.imgur.com/qrbOwN0.png">

- Giao thức SSL Alert: Giao thức này dùng để truyền cảnh báo liên kết SSL với đầu cuối bên kia. Mỗi message trong giao thức này gồm có 2 byte. Byte đầu giữ giá trị cảnh bảo hoặc nguy hiểm. Nếu mức độ là nguy hiểm (Ví dụ: Message không thích hợp, MAC không chính xác .v.v.v.) thì lập tức SSL ngắt kết nối. Byte thứ 2 chứa một mã chỉ ra cảnh báo đặc trưng.
- Giao thức SSL Handshake: Giao thức này cho phép server và client chứng thực với nhau và thương lượng cơ chế mã hóa, thuật toán MAC và khóa mật mã được sử dụng để bảo vệ dữ liệu được gửi trong SSL Record. SSL Handshake Protocol thường được sử dụng trước khi dữ liệu của ứng dụng được truyền đi.

<img src="http://i.imgur.com/PPZcHEw.png">

#### 3.Các loại VPN thông dụng
##### a.Remote Access
- VPN Remote Access còn được gọi là virtual private dial-up network (VPDN), là một kết nối từ người dùng đến mạng LAN công ty. Thường là nhu cầu của 1 tổ chức có nhiều nhân viên hay khách hàng muốn truy cập vào LAN công ty từ nhiều địa điểm từ xa. Vì lý do này, giải pháp Remote Access còn được gọi là client/server.

<img src="http://i.imgur.com/JIcPnCo.png">

##### b.Site to Site
- VPN Site to Site là việc sử dụng mật mã dành cho nhiều người để kết nối nhiều điểm cố định với nhau thông qua mạng công cộng.
- Trong VPN Site to Site, ta cũng có thể chia ra 2 loại là Intranet VPN và Extranet VPN:
  <ul>
  <li>Intranet VPN: Một tổ chức lớn có nhiều bộ phận, khu vực, chi nhánh muốn kết nối với nhau một cách an toàn và bí mật thông qua môi trường Internet. Họ sẽ tạo ra VPN Intranet kết nối LAN với LAN.</li>
  <li>Extranet VPN: Một tổ chức có nhiều mối quan hệ với những tổ chức khác như đối tác, khách hàng ..., họ có thể xây dựng Extranet VPN để kết nối LAN tổ chức này đến LAN tổ chức khác thông qua môi trường Internet</li>
  </ul>
## B.OpenVPN
## C.Tham khảo
- http://luanvan.co/luan-van/nghien-cuu-va-trien-khai-mang-rieng-ao-vpn-30951/
- https://tools.ietf.org/html/rfc1661
- http://www.vnpro.vn/giao-thuc-dinh-huong-lop-2-l2f-va-duong-ham-diem-diem-pptp-trong-vpn/
- http://www.vnpro.vn/giao-thuc-duong-ham-lop-2-l2tp-va-giao-thuc-bao-mat-ip-ipsec-trong-vpn/
- http://www.slideshare.net/conglongit90/giao-thc-bo-mt-ssl
