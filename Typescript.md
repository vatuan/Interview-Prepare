---------------------------------------------- TYPESCRIPT--------------------------------------------------------
# Table of Contents
- [1. TS LÀ GÌ? TẠI SAO NÊN SỬ DỤNG TS](#1-ts-la-gi-tai-sao-nen-su-dung-ts)
- [2. SỰ KHÁC NHAU GIỮA any VÀ unknown ? ](#su-khac-nhau-giua-any-va-unknown-)
- [3. LÀM THẾ NÀO ĐỂ ĐỊNH NGHĨA CÁC THUỘC TÍNH TÙY CHỌN TRONG TS?](#lam-the-nao-de-dinh-nghia-cac-thuoc-tinh-tuy-chon-trong-ts)
- [Contact](#contact)

## 1. TS LÀ GÌ? TẠI SAO NÊN SỬ DỤNG TS
- TS là một dự án mã nguồn mở được phát triển bới Microsoft, nó có thể được coi là phiên bản
nâng cao của JS
- Lợi ích: Phát hiện lỗi sớm, dễ bảo trì và mở rộng dự án
- Basic types: bao gồm 8 loại : boolean, number, string, array, enum, any, void, unknown
- TS giúp ta biết được kiểu trả về của biến, func vd : let a:number = 100


## 2. SỰ KHÁC NHAU GIỮA any VÀ unknown ? 
- any
    + Không kiểm tra kiểu: any có thể chứa bất kì giá trị nào, và TS không kiếm tra kiểu của giá trị này
    + Linh hoạt nhưng nguy hiểm: bạn có thể sử dụng giá trị any và không cần kiểm tra kiểu -> có thể dẫn đến lỗi runtime
- unknown
    + An toàn hơn any: unknown cũng có thể chứa bất kì giá trị nào, nhưng bắt buộc phải kiểm tra kiểu trước khi sử dụng
    + Hữu ích cho các giá trị không rõ kiểu ban đầu 
Tóm lại
 any: Không giới hạn, bỏ qua kiểm tra kiểu, Dễ xảy ra lỗi không mong muốn
 unknown: An toàn hơn, yêu cầu kiểm tra kiểu trước khi sử dụng, Nên sử dụng nếu không rõ kiểu ban đầu



## 3. LÀM THẾ NÀO ĐỂ ĐỊNH NGHĨA CÁC THUỘC TÍNH TÙY CHỌN TRONG TS?
- Sử dụng dấu ? sau tên thuộc tính
- Nếu muốn tất cả thuộc tính của 1 interface hay 1 type thành tùy chọn thì dùng Partial (là 1 utility type)
- Lưu ý:
    + Kiểm tra giá trị trước khi sử dụng: thuộc tính tùy chọn có thể không tồn tại -> cần kiểm tra trước khi sdung để tránh lỗi runtime
    + Nếu thuộc tính không tồn tại có thể cung cấp giá trị mặc định bằng cách sử dụng toán tử nullish (??)
    + Thuộc tính tùy chọn (?) khác với kiểu undefined. TS không tự động gán giá trị undefined cho thuộc tính tùy chọn
    + Khi sử dụng kế thừa (extends) hoặc mở rộng interface, các thuộc tính tùy chọn có thể bị ghi đè

-----------------------------------------------***------------------------------------------------------------------

## 4. SỰ KHÁC BIỆT GIỮA TYPE VÀ INTERFACE LÀ GÌ ? 
- Trong TS, cá type và interface đều được sử dụng để định nghĩa kiểu dữ liệu, nhưng chúng có 1 số điểm khác biệt 
về cách sử dụng và khả năng mở rộng
* Điểm chung
    - cả 2 đều có thể dùng để định nghĩa cấu trúc của 1 object,
    - đều có thể kết hợp nhiều kiểu nhưng cách sử dụng khác nhau, sdung interface với extends, sdung type với Intersection (&)
* Điểm khác 
    - Kế thừa: interface thí dùng extends, type thì dùng Intersection (&)
      VD:
        interface Person {
          name: string;
        }
        
        interface Employee extends Person {
          position: string;
        }
        ==> type 
        type Person = {
          name: string;
        };
        type Employee = Person & {
          position: string;
        };
    - Hợp kiểu (hay còn gọi là Union typ): interface không hỗ trợ, type thì có --> VD type Role = "Admin" | "User" | "Guest";
    - Mở rộng: interface có thể khai báo lại để thêm thuộc tính,  type thì không (nếu vẫn làm thì sẽ gặp lỗi Duplicate identifier 'User')
      VD:
        interface User {
          name: string;
        }
        
        // Bổ sung thêm thuộc tính
        interface User {
          age: number;
        } ==> Hợp lệ
    - Khả năng sử dụng cho kiểu khác: type có thể sử dụng cho kiểu cơ bản, union types hoặc tuple, interface thì không
      VD:
        type ID = string | number; // union type
        type Coordinates = [number, number]; // tuple type
* Khi nào nên dùng interface hoặc type
- Nên dùng interface khi:
    + Mô tả cấu trúc của object
    + Cần mở rộng cấu trúc qua extends
    + Cần hợp tác với nhiều team, nơi interface dễ dàng bổ sung thuộc tính
- Nên dùng type khi:
    + Mô tả union types, tuple, hoặc những kiểu phức tạp không chỉ là object
    + Không cần khả năng mở rộng (extend) hoặc bổ sung thêm thuộc tính


## 5.  Generics TRONG TS LÀ GÌ?
- Là 1 tính năng mạnh mẽ trong TS giúp định nghĩa các kiểu dữ liệu linh hoạt, tái sử dụng và an toàn vè mặt kiểu
- Cho phép viết code mà kiểu dữ liệu có thể được định nghĩa khi sử dụng thay vì cố định tại thời điểm định nghĩa
- Hữu ích trong các trường hợp làm việc với danh sách, cấu trúc dữ liệu, hoặc APIs.

----------------------------------------------------***----------------------------------------------------------------

## 6. MỘT SỐ UTILITY HAY DÙNG ? 
- 
1. abstract class: 
- không thể tạo thể hiện với 1 abstract class mà phải tạo 1 class khác extend lại 
abstract class (không thể tạo đối tượng bằng từ khóa new với abstract class)
- khi 1 method được định nghĩa là 1 abstract thì nó không được triển khai, mà nó phải 
triển khai ở lớp con
- method của lớp abstract có tham số truyền vào, nhưng khi nó được triển khai ở lớp con kế 
thừa thì không nhất thiết phải nhận vào tham số truyền vào

2.Interface
- Một interface được định nghĩa chứ không được triển khai
- nếu 1 class implements interface thì phải overide lại method của interface
Ví dụ : 
```
interface People {
    name: string;
    eat(): void;
    sleep(): void;
}

class Superman implements People {   ==> dùng từ khóa implements 
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    eat(): void {
        console.log('eat');
    }
    sleep(): void {
        console.log('sleep');
    }
}
let tuan: People = new Superman('tuan');
tuan.eat();
tuan.sleep();

```

truffle suite
