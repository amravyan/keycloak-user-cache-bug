How to reproduce:

1. Run the cluster
```
docker-compose up
```

2. Create some users

    2.1 Open http://localhost:8080 in web browser

    2.2 Login, username: admin, password: admin

    2.3 Create 5 users in realm Master

3. Warmup caches on other nodes

    3.1 Open http://localhost:8081 in web browser

    3.2 Login username: admin, password: admin

    3.3 Open "Users" in realm Master

    3.4 Open http://localhost:8082 in web browser

    3.5 Repeat steps 3.2-3.3

4. Check cache stats, exec curl. All queries must return non-zero result
```
curl -s http://127.0.0.1:8080/auth/metrics | grep entries | grep users | grep -v '#'
curl -s http://127.0.0.1:8081/auth/metrics | grep entries | grep users | grep -v '#'
curl -s http://127.0.0.1:8081/auth/metrics | grep entries | grep users | grep -v '#'
```

5. Create client scope
    5.1 Open http://localhost:8080 in web browser

    5.2 Login, username: admin, password: admin

    5.3 Create client scope

6. Check cache stats, exec curl. Now only first curl will return non-zero result.
```
curl -s http://127.0.0.1:8080/auth/metrics | grep entries | grep users | grep -v '#'
curl -s http://127.0.0.1:8081/auth/metrics | grep entries | grep users | grep -v '#'
curl -s http://127.0.0.1:8081/auth/metrics | grep entries | grep users | grep -v '#'
```
