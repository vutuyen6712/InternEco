```sh
#!/bin/sh
```

Một dòng chú thích (comment) trong ngôn ngữ shell bắt đầu
bằng ký tự #. Tuy nhiên Ở đây có một chú thích hơi đặc biệt đó là #!/bin/sh. Đây thực sự không phải là chú thích.

- Cặp ký tự #! là chỉ thị yêu cầu gọi `shell sh` trong thư mục /bin. Shell sh sẽ chịu trách nhiệm thông dịch các lệnh nằm trong tập tin script được tạo.

**shell** : Shell là chương trình giữa bạn và Linux (hay nói chính xác hơn là giữa bạn với nhân Linux). Mỗi lệnh bạn gõ ra sẽ được Shell diễn dịch rồi chuyển tới nhân Linux. Nói một cách dễ hiểu Shell là bộ diễn dịch ngôn ngữ lệnh, ngoài ra nó còn tận dụng triệt để các trình tiện ích và chương trình ứng dụng có trên hệ thống…

**sh** `Shell Bourne`:  là Shell nguyên thuỷ có mặt trên hầu hết các hệ thống Unix/Linux…Nó rất hữu dụng cho việc lập trình Shell nhưng nó không xử lý tương tác người dung như các Shell khác…

**bash** `Bourne Again Shell`  :  là phần mở rộng của sh, nó kế thừa những gì sh đã có và phá huy những gì sh chưa có…Nó có giao diện lập trình rất mạnh và linh hoạt…Cùng với giao diện lệnh dễ dung…Đây là Shell được cài đặt mặc định trên các hệ thống Linux.

**NOTE**: sh là nguyên thủy và bash có thể  diễn dịch và điều khiển các lệnh của sh.

*Có thể chỉ định #!/bin/bash làm shell thông dịch thay cho sh, vì trong Linux thật ra
sh và bash là một. Tuy nhiên như đã nêu, trên các hệ Unix vẫn sử dụng shell sh làm
chuẩn, vì vậy vẫn là một thói quen tốt cho lập trình viên nếu sử dụng shell sh. Khi tiếp cận với UNIX, ta sẽ cảm thấy quen và thân thuộc với shell này hơn. Nên chạy
script trong một shell phụ (như gọi sh chẳng hạn), khi đó mọi thay đổi về môi
trường mà script gây ra không ảnh hưởng đến môi trường làm việc chính.*

![image](./images/avatar.jpg)


MSql work bench
