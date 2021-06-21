Python操作MySQL

---

### 环境配置

安装python-mysql包

```python
pip install mysqlclient
```

连接MySQL

```python
try:
    self.conn = MySQLdb.connect(
        host="localhost",
        user="root",
        passwd="951226",
        db="foundation_user",
        port=3306,
        charset="utf8"
    )
 except MySQLdb.Error as e:
        print('Error:%s' % e)
```

