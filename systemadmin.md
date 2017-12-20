check disk
load average : tai vuot nguong ? thi canh bao (1,5,15)
gui email canh bao.


<h4> Check disk: </h4>

```sh
#!/bin/sh
echo "IP	         Used    Free   Total  Percent	Partition"

ip="$(ifconfig | grep -A 1 'enp3s0' | tail -1 | cut -d ':' -f 2 | cut -d ' ' -f1)"
df -H | grep -vE 'Filesystem|tmpfs|cdrom' | while read output;

do
  used=$(echo $output | awk '{print $3}' )
  free=$(echo $output | awk '{print $4}' )
  total=$(echo $output | awk '{print $2}' )
  percent=$(echo $output | awk '{print $5}' | cut -d'%' -f1 )
  partition=$(echo $output | awk '{print $1}')

  if [ $percent -ge 10 ]; then
	echo "Running out of space \"$partition ($percent%)\" "
  fi
echo " \"$ip	 $used	 $free	 $total	 $percent%	 $partition\" "
done

```


<h4> Load average </h4>
- Loadavg là gì và được tính toán như thế nào?
- Loadavg ở ngưỡng ra sao thì hợp lý?
- Những yếu tố nào ảnh hưởng đến loadavg?
- Phương án phân tích và xử lý khi server tải cao thế nào?

**A/** `Loadavg là gì và được tính toán như nào?`
- Trước tiên bắt đầu với systemload hay còn gọi là load.
Systemload thể hiện số công việc hiện tại hệ thống đang thực thi.
Một server hoàn toàn nhàn rỗi có load là 0
Mỗi tiến trình đang chạy hoặc chờ đợi cpu xử lý sẽ add giá trị 1 vào load
Ví dụ với load = 5 => Có 5 process đang chạy hoặc chờ xử lý (Thread running, waiting)
Nhưng chúng ta thường nghe đến khái niệm loadavg

*Tại sao không phải là load mà là loadavg???*

Ví dụ thế này để các bạn dễ hình dung:

- Tại 1 phần trăm giây đầu tiên Load = 0 vì server đang rảnh rỗi.
- Tại phần trăm giây tiếp theo Load = 5 vì thời điểm có 5 proces cần xử lý.
- Tại phần trăm giây sau đó Load = 99 rất lớn vì thời điểm có rất nhiều process chạy qua hệ thống.

Các con số Load cho mỗi một thời điểm này không có ý nghĩa nhiều trong việc đánh giá tải của hệ thống.

Loadavg thể hiện tải trung bình của hệ thống qua mỗi đoạn thời gian: cho thấy trung bình có bao nhiều process mà server phải thực hiện.

Hay nói cách khác: giá trị Loadavg cho ta thấy được trung bình khối lượng công việc hệ thống phải xử lý trong mỗi khoảng thời gian: 1 phút, 5 phút và 15 phút.

```sh
cat /proc/loadavg  
or uptime
```

Hiểu giá trị console này như sau:

*Trong 1 phút gần đây trung bình có 3 process cần được xử lý (3 thread running, waiting)
Tương tự như vậy có trung bình 5 process xử lý trong vòng 5 phút và 4 process xử lý trong vòng 15 phút.*

**Loadavg ở ngưỡng ra sao thì hợp lý?**

Điều này còn phụ thuộc vào server có bao nhiêu CPU. Có thể xem số CPU của sever bằng lệnh sau:

```sh
cat /sys/devices/system/cpu/online  
```

**Note** `Với 4 CPUs chúng ta có thể xử lý với mức Loadavg <= 4.00 là mức lý tưởng.`


- Case 1: Server 4 CPUs với loadavg = 1.00

Process vừa tạo được hoàn toàn sử dụng CPU với tốc độ xử lý bình thường, performance đạt 100%. Loadavg lúc này tăng lên 1.00. ( mỗi CPU 25%)

- Case 2: Server 4 CPUs với loadavg = 4.00

4 process chạy đồng thời đang phân chia sử dụng 4 CPUs mà server đang có. Tốc độ xử lý vẫn đạt 100%. Loadavg lúc này đã tăng lên 4.00

- Case 3: Server 4 CPUs với loadavg = 8.00

Loadavg thời điểm này lên tới 8.00 gấp đôi số CPU hiện có. Mỗi process chỉ còn sử dụng được 50% CPU => Tốc độ xử lý chậm đi tương ứng.

Những yếu tố nào ảnh hưởng đến loadavg?

  - Cpu Utilazion
  - Disk I/O
  - Network Traffic
