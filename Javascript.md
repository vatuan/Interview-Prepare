# Những câu hỏi Javascript

### 1. Phân biệt var, let, const. Tại sao lại dùng cái này thay vì cái kia trong dự án

>Trước tiên phải làm rõ về scope trong JS là gì 

Scope trong JS chia làm 3 loại
1. Global (Phạm vi toàn cầu)
   
   Tức là tính từ khi khai báo ra biến hoặc hàm thuộc phạm vi global thì ở bất cứ đâu trong chương trình *đều có thể sử dụng* cái biến hoặc cái hàm đó
2. Code block (Phạm vi khối mã)

   Khi mà khai báo ra biến với từ khoá **let** hoặc **const** thì chỉ có thể truy cập biến này trong phạm vi khối mã thôi, ngoài khối mã thì không thể truy cập được

   Trong JS thì phạm vi khối mã được định nghĩa bằng cặp dấu {}

   Biến **var** không được liệt kê trong code block vì khi khai báo 1 biến **var** trong code block thì nó sẽ nhẩy ra vị trí bên ngoài code block
3. Local scope (Phạm vi hàm) 
   
   Là phạm vi được tạo ra bởi phần thân của hàm

#### Phân biệt sự khác nhau
- KHÁC NHAU VỀ BLOCK (PHẠM VI TRUY CẬP)

  Với biến **var**: Nếu được khai báo *trong phạm vi hàm* thì nó được coi là *local scope*, nếu được khai báo *ngoài phạm vi hàm* thì nó được coi là *global scope*, TH đăc biệt nếu nó được khai báo trong phạm vi code block thì nó sẽ nhẩy ra ngoài code block, Như vậy thì ****biến var chỉ có 2 phạm vi**** là local scope và global scope

  Với **let** và **const**: có cả 3 loại phạm vi
- KHÁC NHAU VỀ VIỆC ĐỊNH NGHĨA LẠI

   - **var** có thể được định nghĩa lại 1 cách dễ dàng, có thể dùng từ khóa 'var' hoặc không 
    ```js
       var foo = 100;
	   foo = 30; 
       var foo = 102    
       => log ra sẽ là 102
    ```

   - **let** cũng có thể được định nghĩa(đn) lại , nhưng sẽ chặt chẽ hơn var ,khi đn lại thì không thể dùng từ khóa 'let' được.
    ```js
      let bar = 100, 
      bar = 1000
       ==> log sẽ ra 1000
    ```
	    
   - const thì *không thể được* đn lại

- KHÁC NHAU VỀ HOISTING
  
   **var** được khởi tạo với giá trị là undefined 

   **let** và **const** không có bất kì giá trị khởi tạo nào, nếu sử dụng biến let và const trước
khi khai báo thì sẽ gặp lỗi Reference Error

#### Tại sao nên dùng let và const thay vì var
- let: cho những biến cần được gán lại bên trong block cụ thể và phải đảm bảo là chỉ truy cập biến let đó trong block đó
- const: đối với các biến không nên được gán lại, mang lại khả năng đọc mã tốt hơn và an toàn hơn
- var: tránh sử dụng trong project mới vì nó có thể dẫn đến lỗi và nhầm lẫn do block của nó




### 2.Event loop là gì ? Cách nó xử lý các tác vụ bất đồng bộ như thế nào ?

Event loop là một thành phần của Javascript engine quản lý việc thực thi mã, đặc biệt là các tác vụ bất đồng bộ, giúp đảm bảo rằng các tác vụ không bị blocking

Javascript là ngôn ngữ đơn luồng nhưng nó có thể giải quyết các tác vụ bất đồng bộ nhờ Event Loop


Thử liên tưởng đến 1 trang web cần call hàng trăm API nhưng mà người dùng vẫn có thể click button hay bất kì hành động nào lên trang web mà trang web vẫn không bị block, đó chính là nhờ Event Loop


#### Event loop xử lí các tác vụ bất đồng bộ như thế nào ? 
Javascript hoạt động trong môi trường **đơn luồng**, nghĩa là nó chỉ có thể thực thi *một tác vụ tại một thời điểm*. Tuy nhiên, để hỗ trợ các tác vụ bất đồng bộ mà không chặn luồng chính, Javascript sử dụng Event loop kêt hơp với 1 số thành phần khác như sau:

1. **Call stack (Ngăn xếp gọi)**: Đây là nơi các hàm được thực thi, khi 1 hàm được gọi nó sẽ được đẩy vào ngăn xếp, khi hoàn thành nó sẽ được loại khỏi ngăn xếp
2. **Web APIs**: Các API này được cung cấp bởi trình duyệt (hoặc Node.js trong môi trường máy chủ) và bao gồm các API để xử lí các tác vụ bất đồng bộ như ```setTimeout```, các yêu cầu HTTP và các DOM event
3. **Callback Queue (Hàng đợi callback)**: Khi một tác vụ bất đồng bộ hoàn thành, hàm callback sẽ được thêm vào Hàng đợi Callback (Callback Queue) 
   
#### Các bước của Vòng lặp sự kiện

1. **Thực thi đồng bộ**: Javacript bắt đầu bằng việc thực thi tất cả các đoạn mã đồng bộ, sẽ được đẩy vào Call stack và xử lí ngay lập tức
2. **Web API**: Tại Call stack, khi các hàm bất đồng bộ (như ```setTimeout``` hoặc các yêu cầu fetch) được gọi, chúng sẽ được chuyển đến Web APIs, nơi này chúng sẽ chạy độc lập với Call stack
3. **Callback Queue**: Khi một tác vụ bất đồng bộ hoàn tất thì Web API sẽ đẩy hàm call back đến Callback Queue
4. **Kiểm tra vòng lặp sự kiện**: Vòng lặp sự kiện (Event Loop) liên tục kiểm tra Call Stack, Khi Call Stack trống thì Event Loop sẽ đẩy hàm tiếp theo từ Callback Queue và Call Stack để thực thi