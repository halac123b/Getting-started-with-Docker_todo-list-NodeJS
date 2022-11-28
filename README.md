# Getting-started-with-Docker_todo-list-NodeJS
Docker đc viết bằng Golang, kế thừa một số feature từ Ubuntu kernel <br>

`docker run -i -t ubuntu <bash_cmd>` <br>
-> Nếu không có image "ubuntu" locally, Docker sẽ pull từ registry đã configure, giống như khi ta run "docker run ubuntu"<br>
-> Docker tạo container mới, giống như run "docker container create" <br>
-> Docker allocate read-write system cho container, as its final layer. Cho phép container đang chạy có thể read-write các file trong local filesystem của nó.<br>
-> Docker tạo 1 network interface để container connect to default network.<br>
 Bao gồm gán IP address cho container. By default, container có thể connect external network bằng network của host machine<br>
-> Khởi động container và execute `<bash_cmd>` <br>
  `-i` : "interactively", keep stdin open -> nhập input bằng bàn phím<br>
  `-t` : attach to terminal -> output log hiển thị ra terminal<br>

`docker build -t <image_name> .`
-> build 1 container image mới bằng file "Dockerfile" / update lại image đã có<br>
-> `-t` flag giúp đặt tên cho image: <image_name><br>
-> `.` tell Docker tìm Dockerfile trong folder hiện tại<br>

`docker run -dp <host_port>:<computer_container> <image_name>`<br>
-> Run container từ image<br>
-> `-d` : run container in detached mode (run in background)<br>
-> `-p` : create map giữa port của máy host với port của container<br>
-> Bây giờ trên link `localhost/<port>` sẽ có app, mà không cần connect từ máy host<br>
  -> Run docker sử dụng data từ volume có sẵn:<br>
  `docker run -dp 3000:3000 -v <volume_name>:/etc/todos <image_name>`<br>
    -> `-v` : volume flag |  `/etc/todos` : nơi lưu <br>
    -> Tại <volume_name> không cần phải có tên có sẵn, có thể declare tên chưa có hoặc folder nơi chứa volume thì Docker sẽ tự tạo volume<br>

`docker ps` -> "process status", hiện danh sách các container của Docker<br>
`docker image ls` -> hiện image list<br>

`docker stop <container_id>` -> stop container<br>
`docker rm <container_id>` -> remove container đã stop<br>
-> `docker rm -f <container_id>` : thêm flag `-f`-force, xóa ngay cả khi đang chạy<br>
`docker log <container_id>` -> show log của container<br>

`docker push <dockerhub_username>/<image_name>`<br>

`docker tag <image_name> <new_name>` -> thêm tag cho image<br>

`docker volume create <volume_name>` -> tạo <named volume>, giúp bảo quản đc data khi update và run container mới<br>
`docker volume inspect <volume_name>` -> hiển thị những thông tin chi tiết của volume, trong đó "Mountpoint" là nơi volume đc lưu trữ<br>

PS> `docker run -dp 3000:3000`<br>
      `-w /app -v "$(pwd):/app"`<br>
      `node:12-alpine `<br>
      `sh -c "apk add --no-cache python2 g++ make && npm install && npm run dev"`<br>
  -> Bind mount container giúp xem đc những thay đổi trong code ngay mà k cần phải run lại container mới (trong depen có "nodemon" giúp auto refresh khi code change)<br>

`docker network create <network_name>` -> tạo network, thứ cho phép các container có thể tương tác đc với nhau<br>
`docker run -it --network <network> nicolaka/netshoot`<br>
  -> netshoot là 1 library hữu ích khi làm việc với network<br>
   Trong terminal của netshoot:<br>
     `dig mysql` -> tra cứu IP address của host name <mysql><br>

`docker exec <container_id> <bash_cmd>`<br>
-> run cmd in container's terminal<br>

PS> `docker run -d`<br>
    ` --network todo-app --network-alias mysql `<br>
     `-v todo-mysql-data:/var/lib/mysql` <br>
    ` -e MYSQL_ROOT_PASSWORD=secret `<br>
     `-e MYSQL_DATABASE=todos `<br>
    ` mysql:5.7`<br>
-> Run container mySQL<br>
  `-v` khi chưa create volume này, Docker auto crteate<br>
  `--network` : sử dụng network, "--network-alias" : thêm 1 cách gọi khác cho network<br>
  `-e` : environment, đặt password (ở trên là "secret") và database_name ("todos")<br>

PS> `docker run -dp 3000:3000` <br>
   `-w /app -v "$(pwd):/app" `<br>
   `--network todo-app `<br>
   `-e MYSQL_HOST=mysql` <br>
   `-e MYSQL_USER=root` <br>
  ` -e MYSQL_PASSWORD=secret` <br>
  ` -e MYSQL_DB=todos `<br>
   `node:12-alpine` <br>
   `sh -c "npm install && npm run dev"`<br>

`docker exec -it <mysql_container_id> mysql -u root -p`<br>
-> run cmd in mysql_container<br>
  `-u` : username ("root") |  `-p`: password<br>

sql> `SHOW DATABASES;` -> show list các database<br>
sql> `exit` -> exit sql<br>

`docker compose up -d` -> build project từ composer<br>
`docker compose logs -f` -> Hiển thị chung các log của các service theo real time<br>
  `docker compose logs -f <service>` -> xem log 1 service cụ thể<br>
`docker compose down` -> remove project<br>
  -> cmd này k xóa đc volume, cần thêm flag `--volume`<br>

