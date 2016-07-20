## 1. Các thủ thuật đơn giản với oracle
- Do yêu cầu của quản trị nên đôi lúc phải xài đến DB Oracle (Mình chuyên về MySQL), nên note lại một số thủ thuật để sau tìm lại cho dễ.

### a. Reset mật khẩu trong trường hợp gặp lỗi **ORA-28001: THE PASSWORD HAS EXPIRED**
- Kết nối vào database bằng user sys
<ul>
<li>*sqlplus /nolog*</li>
<li>*connect / as sys*</li>
</ul>
- Hoặc kết nối từ SQL Developer

- Thực hiện câu lệnh sau: `select * from dba_profiles;`
<ul>
<li>- kiểm tra thông tin trường PASSWORD_LIFE_TIME</li>
<li>- Nếu giá trị PASSWORD_LIFE_TIME đang limit là 180 thì thực hiện lệnh sau để xóa bỏ tính năng này: ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;</li>
</ul>

- Thực hiện unlock mà giữ nguyên password cũ:
<ul>
<li>Lấy giá trị cột username và user_id: select * from dba_users;</li>
<li>Lấy giá trị cột password mà cột user trùng với user_id ở trên: select * from sys.user$;</li>
<li>Set password user user: alter user "USERNAME" identified by values 'F894844C34402B67';</li>
<li>Unlock user: alter user USERNAME account unlock;</li>
<li>Kiểm tra lại kết quả: select username,account_status from dba_users;</li>
</ul>

- Nếu cột account_status tương ứng với username vừa unlock chuyển sang OPEN là được.

### b. Export và import database