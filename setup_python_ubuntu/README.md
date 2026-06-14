# Khởi chạy python ảo trên ubuntu
- tải các package cần thiết:
```sh
sudo apt update
sudo apt install python3-venv python3-full
```
- Tạo môi trường ảo 
```
mkdir ~/venvs
python3 -m venv ~/venvs
source ~/venvs/bin/activate
```
<br>
=> khi này có thể tải, sử dụng python3 mà không ảnh hưởng đến python chính của ubuntu
<br>
- Thoát khỏi chế độ venvs

```
deactivate
```


